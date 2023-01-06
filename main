#include <iostream>
#include "fstream"
#include "cstdlib"
#include "unistd.h"

bool **create_universe_two_dim_array(int rows, int colums);
void create_living_cells(bool **arr, std::ifstream *file);
void copy_world_array(bool **arr, bool **old_arr, int rows, int colums);
void delete_two_dim_array(bool **arr, int rows, int colums);
void print_array(bool **arr, int rows, int colums);
int get_count_lining_cells(bool **arr, int rows, int colums);
void next_generation(bool **arr, bool **old_arr, int rows, int colums );
void read_point_neighbors(int nb[][2], int x, int y);
int count_live_neighbors(bool **arr, int rows, int colums, int x, int y);
int comparison_world(bool **arr, bool **old_arr, int rows, int colums);

int main(int argc, char** argv){
    
    std::ifstream file ("data.txt");
    
    int rows = 0;           int colums = 0;
    int generationSum = 0;  int live_cells = -1;

    bool gameContinue = false;

    file >> rows;
    file >> colums;

    bool **generation = create_universe_two_dim_array(rows, colums);
    bool **old_generation = create_universe_two_dim_array(rows, colums);

    create_living_cells(generation, &file);

    //main cycle -->
    while (live_cells != 0 && !gameContinue) {
        std::system("clear");
        ++generationSum;

        print_array(generation, rows, colums);

        gameContinue = comparison_world(generation, old_generation, rows, colums) == 0;
        live_cells = get_count_lining_cells(generation, rows, colums);


        std::cout << "Generation: " << generationSum << ". ";
        std::cout << "Alive cells: " << live_cells << std::endl;
         if (gameContinue) {
            std::cout << "The world has stagnated. Game over" << std::endl;
        }

        if (live_cells == 0) {
            std::cout << "All cells are dead. Game over" << std::endl;
        }

        sleep(2);

        copy_world_array(generation, old_generation, rows, colums);
        next_generation(generation, old_generation, rows, colums);
    }
    
    //<--

    file.close();
    delete_two_dim_array(generation, rows, colums);
    delete_two_dim_array(old_generation, rows, colums);   

    return 0;
}

bool **create_universe_two_dim_array(int rows, int colums){
    bool **arr = new bool * [rows];

    //filling an array with dead cells
    for(int i = 0; i < rows; i++){
        arr[i] = new bool [colums];
        for(int j = 0; j < colums; j++){
            arr[i][j] = 0;
        }
    }
    
    return arr;
}

void create_living_cells(bool **arr, std::ifstream *file){
    //setting the initial coordinates of living cells
    for(int i = 0; *file >> i;){
        for(int j = 0; *file >> j; ){
        arr[i][j] = 1;
        break;
        }
    }
}

void print_array(bool **arr, int rows, int colums){
    
    for (int i = 0; i < rows ; i++) {
        for(int j = 0; j < colums; j++){
            if (arr[i][j] == 1){
                std::cout << "*";    
            } else {
                std::cout << "-";
            }
            std::cout << " ";
        } 
        std::cout << std::endl;
    }
}

void delete_two_dim_array(bool **arr, int rows, int colums){
    
    for (int i = 0; i < rows; i++){
	   delete arr[i];
    }
    
    delete [] arr;
}

void copy_world_array(bool **arr, bool **old_arr, int rows, int colums){
    for (int i = 0; i < rows; i++){
        for(int j = 0; j < colums; j++){
            old_arr[i][j] = arr[i][j];
        }
    }
}

int get_count_lining_cells(bool **arr, int rows, int colums){
    
    int res = 0;

    for (int i = 0; i < rows; i++){
        for(int j = 0; j < colums; j++){
            if (arr[i][j] == 1){
            res++;            
            }
        }
    }
    return res;
}

void read_point_neighbors(int nb[][2], int x, int y){
    int i, j;
    int k = 0;

    for (i = x - 1; i <= x + 1; i++) {
        for (j = y - 1; j <= y + 1; j++) {
            if (i == x && j == y) {
                continue;
            }
            nb[k][0] = i;
            nb[k][1] = j;
            k++;
        }
    }
}

int count_live_neighbors(bool **arr, int rows, int colums, int x, int y){
    int count = 0;
    int i;
    int nb[8][2];
    int _x, _y;

    read_point_neighbors(nb, x, y);

    for (i = 0; i < 8; i++) {
        _x = nb[i][0];
        _y = nb[i][1];

        if (_x < 0 || _y < 0) {
            continue;
        }
        if (_x >= rows || _y >= colums) {
            continue;
        }

        if (arr[_x][_y] == 1) {
            count++;
        }
    }

    return count;
}

void next_generation(bool **arr, bool **old_arr, int rows, int colums ){

    int live_nb;
    bool p;

    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < colums; j++) {
            p = old_arr[i][j];
            live_nb = count_live_neighbors(old_arr, rows, colums, i, j);

            if (p == 0) {
                if (live_nb == 3) {
                    arr[i][j] = 1;
                }
            } else {
                if (live_nb < 2 || live_nb > 3) {
                    arr[i][j] = 0;
                }
            }
        }
    }
}

int comparison_world(bool **arr, bool **old_arr, int rows, int colums){
    
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < colums; j++) {
            if (arr[i][j] != old_arr[i][j]) {
                return -1;
            }
        }
    }
    return 0;
}
