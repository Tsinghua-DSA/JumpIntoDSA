# 概论

- 从一开始，就以不容易出问题的方式写代码。
- 通过思考，而不是盲目试错来进行调试。
- 既调试有问题的程序，也调试“出问题的思路“
- 善于利用各种工具进行调试，选择适合自己的工具。
- 遇到困难时，积极向他人寻求关于调试的帮助，并从中学习。

参考书: Why Programs Fail


举例: 贪吃蛇程序

```c++
//gcc -o snake snake.c -lncurses
//g++ -o snake snake.cpp -lncurses
#include <ncurses.h>
#include <stdlib.h>
#include <time.h>
#include <string.h>

#define WIDTH 30
#define HEIGHT 10

int x, y, food_x, food_y, score;
int tailX[100], tailY[100];
int nTail;
enum eDirecton { STOP = 0, LEFT, RIGHT, UP, DOWN};
enum eDirecton dir;

void Setup() {
    initscr();
    clear();
    noecho();
    cbreak();
    curs_set(0);
    srand(time(0));
    x = WIDTH / 2;
    y = HEIGHT / 2;
    food_x = (rand() % (WIDTH - 2)) + 1;
    food_y = (rand() % (HEIGHT - 2)) + 1;
    score = 0;
    dir = STOP;
}

void Draw() {
    clear();
    for (int i = 0; i < WIDTH + 2; i++)
        mvprintw(0, i, "+");
    for (int i = 0; i < HEIGHT + 2; i++) {
        for (int j = 0; j < WIDTH + 2; j++) {
            if (i == 0 || i == HEIGHT + 1)
                mvprintw(i, j, "+");
            else if (j == 0 || j == WIDTH + 1)
                mvprintw(i, j, "+");
            else if (i == y && j == x)
                mvprintw(i, j, "O");
            else if (i == food_y && j == food_x)
                mvprintw(i, j, "*");
            else {
                for (int k = 0; k < nTail; k++) {
                    if (tailX[k] == j && tailY[k] == i) {
                        mvprintw(i, j, "o");
                        break;
                    }
                }
            }
        }
    }
    mvprintw(HEIGHT + 3, 0, "Score: %d", score);
    refresh();
}

void Input() {
    keypad(stdscr, TRUE);
    halfdelay(1);
    switch (getch()) {
        case 'a':
            dir = LEFT;
            break;
        case 'd':
            dir = RIGHT;
            break;
        case 'w':
            dir = UP;
            break;
        case 's':
            dir = DOWN;
            break;
        case 'x':
            dir = STOP;
            break;
    }
}

void Logic() {
    int prevX = tailX[0];
    int prevY = tailY[0];
    int prev2X, prev2Y;
    tailX[0] = x;
    tailY[0] = y;
    for (int i = 1; i < nTail; i++) {
        prev2X = tailX[i];
        prev2Y = tailY[i];
        tailX[i] = prevX;
        tailY[i] = prevY;
        prevX = prev2X;
        prevY = prev2Y;
    }
    switch (dir) {
        case LEFT:
            x--;
            break;
        case RIGHT:
            x++;
            break;
        case UP:
            y--;
            break;
        case DOWN:
            y++;
            break;
        default:
            break;
    }
    if (x == food_x && y == food_y) {
        score += 10;
        food_x = (rand() % (WIDTH - 2)) + 1;
        food_y = (rand() % (HEIGHT - 2)) + 1;
        nTail++;
    }
    if (x == 0 || x == WIDTH + 1 || y == 0 || y == HEIGHT + 1) {
        endwin();
        exit(0);
    }
    for (int i = 0; i < nTail; i++) {
        if (tailX[i] == x && tailY[i] == y) {
            endwin();
            exit(0);
        }
    }
}

int main() {
    Setup();
    while (1) {
        Draw();
        Input();
        Logic();
    }
    endwin();
    return 0;
}

```