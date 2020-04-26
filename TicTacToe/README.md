Dengan minimax ini, AI bisa membuat keputusan yang memungkinkan dirinya tidak terkalahkan, 
karena AI akan membuat simulasi setiap kemungkinan yang ada dan memastikan pergerakan dia 
selanjutnya benar benar kemenangan.

#include <stdio.h>

	//Pada bagian dibawah ini untuk mendeteksi pada kolom apakah sudah terisi karakter X atau O atau bahkan kosong 
    char gridChar(int i) 
	{
    	switch(i) 
		{
        	case -1:
         		return 'X';
       		case 0:
        		return ' ';
        	case 1:
         		return 'O';
        }
    }	

	//Untuk mencetak papan tic tac toe.
  	void draw(int b[9]) 
	{
  		printf(" %c | %c | %c\n",gridChar(b[0]),gridChar(b[1]),gridChar(b[2]));
  		printf("---+---+---\n");
  		printf(" %c | %c | %c\n",gridChar(b[3]),gridChar(b[4]),gridChar(b[5]));
  		printf("---+---+---\n");
  		printf(" %c | %c | %c\n",gridChar(b[6]),gridChar(b[7]),gridChar(b[8]));

  	}


  	/* untuk mengecek setelah player melakukan langkah / mengisi karakter pada 
  	papan itu mencapai kemenangan (sudah terbentuk garis). */
  
  	int win(const int board[9]) 
	{	//menentukan apakah seorang pemain telah menang, return 0 sebaliknya.
  		unsigned wins[8][3] = {{0,1,2},{3,4,5},{6,7,8},{0,3,6},{1,4,7},{2,5,8},{0,4,8},{2,4,6}};
  		int i;
   		for(i = 0; i < 8; ++i) 
		{
			if(board[wins[i][0]] != 0 &&
     		board[wins[i][0]] == board[wins[i][1]] &&
     		board[wins[i][0]] == board[wins[i][2]])
  	    	return board[wins[i][2]];

  		}
  		return 0;
  	}

  	//untuk berjalannya tic tac toe dengan menggunakan metode minimax.
  	int minimax(int board[9], int player) 
	{	//posisi pemain (giliran mereka) pada papan
  		int winner = win(board);
  		if(winner != 0) return winner*player;
  		int move = -1;
  		int score = -2;
  		int i;
  		for(i = 0; i < 9; ++i) 
		{//untuk semua bergerak,
  			if(board[i] == 0) 
			{
      			board[i] = player;//mencoba untuk bergerak
      			int thisScore = -minimax(board, player*-1);
      			if(thisScore > score) 
				{
          			score = thisScore;
          			move = i;
      			}
      			board[i] = 0;//Setel ulang papan setelah dicoba
  			}
  		}

  	if(move == -1) return 0;
  	return score;
  	}

  	//Fungsi untuk computer sebagai lawan untuk berjalan menentukan letak karakter.
  	void computerMove(int board[9]) 
	{
  		int move = -1;
  		int score = -2;
   		int i;
  		for(i = 0; i < 9; ++i) 
		{
    		if(board[i] == 0) 
			{
    		 	board[i] = 1;
   	   			int tempScore = -minimax(board, -1);
   	 	  		board[i] = 0;
      			if(tempScore > score) 
				{
          			score = tempScore;
          			move = i;
     		    }
 		 	}
  		}
		//returns skoe berdasarkan  minimax tree pada node yang diberikan 
  		board[move] = 1;
  	}


  	//Fungsi untuk player utama berjalan menentukan letak karakter.
  	void playerMove(int board[9]) 
	{
  		int move = 0;
   		do 
		{
	  		printf("\nInput move ([0..8]): ");
 	 		scanf("%d", &move);
	  		printf("\n");
  		} 
   		while (move >= 9 || move < 0 && board[move] == 0);
  		board[move] = -1;
  	}


  	int main() 
  	{
  		int board[9] = {0,0,0,0,0,0,0,0,0};

		//kotak komputer 1, kotak pemain -1.
   		printf("Computer: O, You: X\nPlay (1)st or (2)nd? ");
   		int player=0;
  		scanf("%d",&player);
  		printf("\n");

  		unsigned turn;
  		for(turn = 0; turn < 9 && win(board) == 0; ++turn)
		{
  			if((turn+player) % 2 == 0)
      		computerMove(board);
  			else 
			{
      			draw(board);
      			playerMove(board);
      		}
		}

		switch(win(board))
		{
 	 		case 0:
      			printf("Draw.\n");
      			break;
  			case 1:
      			draw(board);
      			printf("You lose. How Shameful.\n");
      			break;
  			case -1:
      			printf("You win. Impressive!\n");
      			break;
  		}
  	}
