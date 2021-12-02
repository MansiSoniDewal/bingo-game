# bingo-game
made a bingo game by adding some more features to it i.e., by taking input of no. of players by user itself, taking adjustbale size of arrays.

#include<iostream>
#include<conio.h>
#include<bits/stdc++.h>
using namespace std;

struct game
{
    int a[5][5][5];
    int scores[5];
    int numbers_called[25];
};

void welcome()
{    cout<<"Welcome To BINGO";     }

void create_random_matrices(struct game &obj,int nop)
{
    int a[25],i,j,k,num;
    int unsigned seed = 0;

    //making an array to shuffle
    for(i=0;i<25;i++)
        a[i] = i+1;
    
    //making random matrices in obj
    for(i=0;i<nop;i++)
    {
        //shuffling array a
        shuffle(a,a+25,default_random_engine(seed));
        
        num = 0;
        for(j=0;j<5;j++)
            for(k=0;k<5;k++)
                obj.a[i][j][k] = a[num++];
    }
}

void display_matrices(struct game &obj,int nop)
{
    int i,j,k;
    for(i=0;i<nop;i++)
    {
        cout<<"\n";
        for(j=0;j<5;j++)
        {
            for(k=0;k<5;k++)
                cout<<obj.a[i][j][k]<<" ";
            cout<<endl;
        }
        cout<<"-------------";
    }
}

void update_arrays(struct game &obj,int nop,int num)
{
    int i,j,k;
    for(i=0;i<nop;i++)
        for(j=0;j<5;j++)
        {
            for(k=0;k<5;k++)
                if(obj.a[i][j][k] == num)
                    obj.a[i][j][k] = 0;
        }
}

int check_row(int m[5][5])
{
    int i,j,score=0,count = 0;
    for(i=0;i<5;i++)
    {
        count = 0;
        for(j=0;j<5;j++)
            if(m[i][j] == 0)
                count += 1;
        if(count == 5)
            score+=1;
    }
    return score;
}

int check_column(int m[5][5])
{
    int i,j,score=0,count = 0;
    for(i=0;i<5;i++)
    {
        count = 0;
        for(j=0;j<5;j++)
            if(m[j][i] == 0)
                count += 1;
        if(count == 5)
            score+=1;
    }
    return score;
}

int check_diagonal(int m[5][5])
{
    int i,j,count=0,score=0;

    for(i=0;i<5;i++)
        if(m[i][i] == 0)
            count+=1;
    if(count == 5)
        score += 1;

    count = 0;
    for(i=0;i<5;i++)
        for(j=0;j<5;j++)
            if(i+j == 4 || m[i][j] == 0)
                count+=1;
    
    if(count == 5)
        score+=1;
    
    return score;
}

int update_scores(struct game &obj,int nop)
{
    int i,j,k,m[5][5],row,diagonal,column,flag=0;
    for(i=0;i<nop;i++)
    {
        for(j=0;j<5;j++)
            for(k=0;k<5;k++)
                m[j][k] = obj.a[i][j][k];
        row = check_row(m);
        column = check_column(m);
        diagonal = check_diagonal(m);
        obj.scores[i] = row+column+diagonal; 
        if(obj.scores[i] == 5)
            flag = 1;
    }
    return flag;
}

void play(struct game &obj,int nop)
{
    int count = 0,num,j,game_stop_flag = 0,i;
    while(game_stop_flag != 1)
    {
        cout<<"\n-----------------ROUND "<<count+1<<"---------------";
        display_matrices(obj,nop);
        a1:cout<<"\n> Player no. "<<((count+1)%nop)<<" enter a number which is not yet cut: ";
        cin>>num;

        for(i=0;i<count;i++)
            if(num == obj.numbers_called[i])
            {
                cout<<"\n Entered number has been cut already !!!";
                break;
                getch();
                goto a1; 
            } 

        obj.numbers_called[count] = num;
        update_arrays(obj,nop,num);
        game_stop_flag = update_scores(obj,nop);
        count++;       
    }
}

void result(struct game &obj,int nop)
{
    int i;
    cout<<"\n SCORES ";
    for(i=0;i<nop;i++)
        cout<<"\nPlayer 1: "<<obj.scores[i];
}

void start_play()
{
    int no_of_players;

    a1:cout<<"\n> Enter number of players playing the game (2-5): ";
    cin>>no_of_players;

    if(no_of_players>5 || no_of_players<2)
    {
        cout<<"\nWrong Choice !";
        goto a1;
    }
    else
    {
        struct game obj;
        int i;
        
        //initialising scores with 0
        for(i=0;i<no_of_players;i++)
            obj.scores[i] = 0;
        
        //initialising numbers_called with 0s
        for(i=0;i<25;i++)
            obj.numbers_called[i] = 0;

        //Creating Random Matrices for all players 
        create_random_matrices(obj,no_of_players);

        //playing game real
        play(obj,no_of_players);

        //showing the result
        result(obj,no_of_players);
    }
}

void instructions()
{cout<<"Instructions";}

void menu()
{
    char ch;
    do
    {
        cout<<"\n\tMENU"
            <<"\n\n1. Play Game"
            <<"\n2. Instructions"
            <<"\n3. Exit"
            <<"\n\n> Enter your choice (1-3) : ";
        cin>>ch;
        switch(ch)
        {
            case '1':start_play();
                     cout<<"Thank You For playing the game!";
                     break;
            case '2':instructions();
                     break;
            case '3':cout<<"\nThanks for coming !"
                         <<"\nTerminating....";
                     break;
            default :cout<<"\nWrong Input!!" ;
                     break;
        }
        getch();      
    }while(ch!='3' || ch!='1');    
}

int main()
{
    welcome();
    menu();
    return 0;
}
