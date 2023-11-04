#include<windows.h>
#include<stdio.h>
#include<stdlib.h>
#include<sys/time.h>
#define N 5
#define total 25

int i,j,k, speed = 1;

int main(){
    int i, nextNumber, player1Won = 0, player2Won = 0;
    int player1[5][5], player2[5][5];
    int numbers[25] = {0};

    printf("WELCOME TO BINGO GAME.......\n");
    printf("Press Enter Twice to start the game.....");
    getchar();

    initializePlayersTicket(player1);
    initializePlayersTicket(player2);

    printf("Player 1's Ticket:- \n");
    printTicket(player1);
    printf("Player 2's Ticket:- \n");
    printTicket(player2);

    printf("Enter Speed from 1 (Fastest) to 10 (Slowest) : ");
    scanf("%d", &speed);

    if(speed<0 || speed>10){
        speed = 1;
        printf("Invalid input for speed. Taking speed value as 1 by default....\n");
    }

    printf("Press Enter Twice to start the game.....");
    getchar();


    while(!(player1Won || player2Won)){
         nextNumber = getValidRandomNumber( numbers );
         printf("Current Number: %d \n", nextNumber);

        crossNumber(nextNumber, player1);
        crossNumber(nextNumber, player2);

        printf("Player 1's Ticket:- \n");
        printTicket(player1);
        printf("Player 2's Ticket:- \n");
        printTicket(player2);


        player1Won = didPlayerWin(player1);
        player2Won = didPlayerWin(player2);


        if(player1Won && player2Won){
            printf("DRAW...");
        }
        else if(player1Won){
            printf("Player 1 Won....");
        }
        else if(player2Won){
            printf("Player 2 Won....");
        }

    }


    return 0;
}

int didPlayerWin(int player[N][N]){
    int count;
    for(i=0;i<N;i++){
        count = 0;
        for(j=0;j<N;j++){
            if(player[i][j] == -1){
                count++;
            }
        }
        if(count == 5){
            return 1;
        }
    }

    for(j=0;j<N;j++){
        count = 0;
        for(i=0;i<N;i++){
            if(player[i][j] == -1){
                count++;
            }
        }
        if(count == 5){
            return 1;
        }
    }
    count = 0;
    for(i=0;i<N;i++){
        if(player[i][i] == -1){
            count++;
        }
    }
    if(count == 5){
        return 1;
    }
    count = 0;
    for(i=0;i<N;i++){
        if(player[i][N-1-i] == -1){
            count++;
        }
    }
    if(count == 5){
        return 1;
    }

    return 0;
}

void printTicket( int player[N][N] ){
    for(i=0;i<26;i++){
        printf("-");
    }
    printf("\n");
    for(i=0;i<N;i++){
            printf("|");
        for(j=0;j<N;j++){
            if(player[i][j] == -1){
                printf("  X |");
            }
            else{
                printf("%3d |", player[i][j]);
            }

        }
        printf("\n");
        for(k=0;k<26;k++){
                printf("-");
        }
        printf("\n");
    }
        printf("\n");
        Sleep(speed*100);
}

void crossNumber(int target, int player[5][5]){
    for(i=0;i<N;i++){
        for(j=0;j<N;j++){
            if( target == player[i][j]){
                player[i][j] = -1;

            }
        }
    }
}


void initializePlayersTicket(int player[5][5]){
    printf("Creating player's ticket...\n");
    int x,i,j;
    int crossedNumbers[25] = {0};
    for(i=0;i<N;i++){
       for(j=0;j<N;j++){
            x = getValidRandomNumber( crossedNumbers );
            player[i][j] = x;
            crossedNumbers[x-1] = 1;
            //printf("%d ", player[i][j]);
       }
       //printf("\n");
    }


}

int getValidRandomNumber( int *numbers ){
    int index = getRandomNumber();
    while(numbers[index] == 1){
        index++;
        if(index>=25){
            index -= 25;
        }
    }
    return index+1;

}

int getRandomNumber() {
    struct timeval te;
    gettimeofday(&te, NULL); // get current time
    long long milliseconds = te.tv_sec*1000LL + te.tv_usec/1000; // calculate milliseconds
    // printf("milliseconds: %lld\n", milliseconds);
    Sleep(100);
    return milliseconds%25;
}
