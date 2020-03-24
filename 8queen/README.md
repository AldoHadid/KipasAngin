https://github.com/AldoHadid/KipasAngin/issues/1#issue-586738006

#include <stdio.h>
#include <conio.h>   
#include <Windows.h>   
#define N 8  
//Untuk mewarnai karakter
void Color(int col) 
{  
    HANDLE hConsole = GetStdHandle(STD_OUTPUT_HANDLE);  
    SetConsoleTextAttribute(hConsole, col);  
}    
//Untuk print solusi terakhir
void printSolution(int board[N][N]) 
{  
    for (int i = 0; i < N; i++) 
	{  
        for (int j = 0; j < N; j++) 
		{  
            if (board[i][j] == 1) 
			{  
                Color(2);  
                printf("%d ", board[i][j]);  
            } 
			else 
			{  
                Color(15);  
                printf("%d ", board[i][j]);  
            }  
        }  
        printf("\n");  
    }  
}  
//Untuk mengecek apakah ratunya aman jika diletakkan di suatu kolom dan baris.  
//Ini akan return true jika ratunya aman dan false jika kebalikannya.  
bool isSafe(int board[N][N], int row, int col) 
{  
    int i, j;  
	//Mengecek baris di sisi kiri  
    for (i = 0; i < col; i++)  
        if (board[row][i]) return false;  
	//Mengecek diagonal atas di sisi kiri   
    for (i = row, j = col; i >= 0 && j >= 0; i--, j--)  
        if (board[i][j]) return false;  
	//Mengecek diagonal bawah di sisi kiri  
    for (i = row, j = col; j >= 0 && i < N; i++, j--)  
        if (board[i][j]) return false;  
    return true;  
}   
//Kegunaan rekursif untuk memecahkan masalah N queens. 
bool solveNQUtil(int board[N][N], int col) 
{  
	//Base case: Jika semua ratu telah ditempatkan maka return true  
    if (col >= N) return true;  
	//Memperhitungkan kolom ini dan coba menempatkan ratu ini di semua baris satu per satu   
    for (int i = 0; i < N; i++) 
	{  
        if (isSafe(board, i, col)) 
		{  
			//Menempatkan ratu di papan[i][col] 
            board[i][col] = 1;  
			//Berulang untuk menempatkan sisa ratu 
            if (solveNQUtil(board, col + 1)) return true;  
			//Jika menempatkan ratu di papan [i] [col] tidak mengarah ke solusi, maka hapus ratu dari papan [i] [col]
            board[i][col] = 0;  
        }  
    }  
	//Jika ratu tidak bisa ditempatkan di baris manapun di kolom col maka return false\oj  
    return false;  
}  
/*Fungsi ini memecahkan masalah N Queen menggunakan Backtracking.
Terutama menggunakan solveNQUtil () untuk menyelesaikannya.
Return false jika ratu tidak dapat ditempatkan, 
sebaliknya return true dan mencetak penempatan ratu dalam bentuk 1s.
Harap dicatat bahwa mungkin ada lebih dari satu solusi, 
fungsi ini mencetak salah satu fungsi yang memungkinkan */ 
bool solveNQ() 
{  
    int board[N][N] = 
	{  
        {  
            0,  
            0,  
            0,  
            0  
        },  
        {  
            0,  
            0,  
            0,  
            0  
        },  
        {  
            0,  
            0,  
            0,  
            0  
        },  
        {  
            0,  
            0,  
            0,  
            0  
        }  
    };  
    if (solveNQUtil(board, 0) == false) 
	{  
        printf("Solution does not exist");  
        return false;  
    }  
    printSolution(board);  
    return true;  
}  
int main() 
{  
    solveNQ();  
    _getch();  
    return 0;  
} 
