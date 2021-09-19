// ==== 지렁이 게임 Earthworm Game by jhjang, 2021.09. ====
// 
// 학생들이 직관적으로 이해할 수 있도록 게임판을 통째로 만드는 방법을 사용했다.
// 사실은 각 오브젝트(지렁이, +, -)의 좌표나 상태만 기억하는 편이 더 쉽게 구현할 수 있는데,
// 시간이 남는 사람은 한번 시도해보기 바람

#include<stdio.h>
#include <Windows.h>
#include <stdbool.h>
#include <limits.h>

#define WIDTH   18
#define HEIGHT  10

// 배열 등을 함수에 전달하는 것도 좋지만 이 경우는 전역 변수로 두면 가독성도 높고 작성하기도 편하다.
// You can deliver array('board') through function parameters. 
// However, in this case, it's much readable to define 'board' and the other variables as global variables.
char board[HEIGHT][WIDTH];

// 지렁이 현재 위치
// the current position of the worm
int x, y;

// 현재 방향(worm_x, worm_y의 증가/감소량)
// the current direction. i.e. increment/decrement values for 'worm_x' and 'worm_y'
int dx, dy;

void initialize(int, int);
void display();
bool is_blocked();
void turn();
void move();


// 게임판과 지렁이 초기화 initialize game baord & earthworm
void initialize(int start_x, int start_y) {
    // 위, 아래 가로벽 horizontal walls
    for (int i = 0; i < WIDTH; i++) {
        board[0][i] = board[HEIGHT - 1][i] = '#';
    }

    // 안쪽의 공백 inner space
    for (int i = 1; i < HEIGHT - 1; i++) {
        for (int j = 1; j < WIDTH - 1; j++) {
            board[i][j] = ' ';
        }
    }

    // 좌, 우 세로벽 vertical walls
    for (int i = 0; i < HEIGHT; i++) {
        board[i][0] = board[i][WIDTH - 1] = '#';
    }

    // 지렁이 초기 위치와 방향
    // initial position & direction of earthworm
    x = start_x;
    y = start_y;
    dx = dy = 1;
    board[x][y] = '@';
}

// 게임판 화면 출력 display the game board
void display() {
    for (int i = 0; i < HEIGHT; i++) {
        for (int j = 0; j < WIDTH; j++) {
            printf("%c", board[i][j]);
        }
        printf("\n");
    }
}

// 다음 이동할 위치를 조사한다.
// Investigate the next position in the current direction
bool is_blocked() {
    return board[x + dx][y + dy] != ' ';
}

void turn() {
    if (board[x + dx][y] != ' ')
        dx = -dx;
    else
        dy = -dy;
}

void move() {
    board[x][y] = ' ';
    x += dx;
    y += dy;
    board[x][y] = '@';
}

int main(void)
{
    initialize(1, 1);
    while (1) {
        while (is_blocked()) {
            turn();
        }
        move();
        display();
        Sleep(500);
        system("cls");
    }
    return 0;
}

