BFS

BFS adalah metode pencarian yang dilakukan pada semua simpul dalam setiap level secara beurutan dari kiri ke kanan. Jika pada satu level belum ditemukan solusi, maka pencarian akan dilakukan pada level berikutnya dan seterusnya sampai ditemukan solusi.

/*Menggunakan branch and bound yang merupakan algoritma
          Breath First Search yang berjalan layaknya penggunaan queue */

        #include <bits/stdc++.h> 
        using namespace std; 
        #define M 3 

 	struct Node 
        { 
        // menyimpan parent node dari current node
        // membantu dalam melacak jejak ketika jawabannya ditemukan
        Node* parent; 

        // menyimpan matrix 
        int mat[M][M]; 

        // menyimpan kotak koordinat kosong 
        int a, b; 

        // menyimpan jumlah kotak yang salah tempat
        int cost; 

        // menyimpan jumlah gerakan sejauh ini
        int level; 
        }; 

        // Fungsi untuk mencetak M x M matrix 
        int printMatrix(int mat[M][M]) 
        { 
        for (int i = 0; i < M; i++){ 
            for (int j = 0; j < M; j++) 
            printf("%d ", mat[i][j]); 
            printf("\n"); 
            } 
        }       

        //Berfungsi untuk mengalokasikan node baru
        Node* newNode(int mat[M][M], int a, int b, int newA, 
            int newB, int level, Node* parent) 
        { 
            Node* node = new Node; 

        // atur pointer dari path ke root 
            node->parent = parent; 

        // copy data dari parent node ke current node 
            memcpy(node->mat, mat, sizeof node->mat); 

        //Pindahkan kotak dengan 1 posisi 
            swap(node->mat[a][b], node->mat[newA][newB]); 

        //Atur jumlah kotak yang salah tempat 
             node->cost = INT_MAX; 

        //Atur jumlah gerakan sejauh ini
            node->level = level; 

        //Perbarui koordinat kotak kosong baru
            node->a = newA; 
            node->b = newB; 
        return node; 
        } 

        //bawah, kiri, atas, kanan
            int row[] = { 1, 0, -1, 0 }; 
            int col[] = { 0, -1, 0, 1 }; 

     // Fungsi untuk memeriksa apakah (a, b) adalah koordinat matriks yang valid
             int isSafe(int a, int b) 
        { 
             return (a >= 0 && a < M && b >= 0 && b < M); 
        } 

        // mencetak path dari root node ke destination node 
            void printPath(Node* root){ 
                if (root == NULL) 
            return; 
                printPath(root->parent); 
                printMatrix(root->mat);     
                printf("\n"); 
        } 

	bool testing (int mat[M][M],int final[M][M]){
 	for (int i = 0; i < M; i++) 
	{ 
		for (int j = 0; j < M; j++) {
			if (mat [i] [j] != final [i] [j]){
				return false;
				} 
			}
		}
 		return true;
 	}

 	/* Berfungsi untuk memecahkan algoritma puzzle M * M - 1 menggunakan
           Branch dan Bound. a dan b adalah koordinat kotak kosong
           dalam kondisi awal */
          void solve(int initial[M][M], int a, int b, 
          int final[M][M]) 
          { 
        // Buat prioritas queue untuk menyimpan live nodes dari search tree  
            queue <Node*> pq;

        // buat root node and menghitung biayanya
           Node* root = newNode(initial, a, b, a, b, 0, NULL); 

        // Tambahkan root ke daftar live node 
            pq.push(root); 

        /* Menemukan live node dengan biaya paling sedikit,
          tambahkan childerns ke daftar live node dan
          menghapusnya dari daftar */
          
        while (!pq.empty()) 
        { 
        // Temukan live node dengan perkiraan biaya terendah
            Node* min = pq.front(); 

        // Node yang ditemukan dihapus dari daftar node hidup
        pq.pop(); 

        // jika min adalah answer node;
        if (testing (min->mat, final)) 
        { 
        // cetak path dari root ke destination; 
            	printPath(min); 
			printf ("Move: %d",min->level);
			return;
        } 

        /* lakukan untuk setiap child min
          maks 4 child untuk node */ 
        for (int i = 0; i < 4; i++) 
        { 
            if (isSafe(min->a + row[i], min->b + col[i])) 
        { 
        // buat child node dan menghitung harganya 
            Node* child = newNode(min->mat, min->a, 
            min->b, min->a + row[i], 
            min->b + col[i], 
            min->level + 1, min); 

        //Tambahkan child ke daftar dari live node
            pq.push(child); 
            } 
          } 
         } 
        } 
 
        int main() 
        { 
        // Konfigurasi awal 
        // Nilai 0 digunakan untuk ruang kosong 
            int initial[M][M] = 
        { 
            {1, 2, 3}, 
            {5, 6, 0}, 
            {7, 8, 4} 
        }; 

        //Konfigurasi final yang dapat dipecahkan
        //Nilai 0 digunakan untuk ruang kosong
        int final[M][M] = 
        { 
            {1, 2, 3}, 
            {5, 8, 6}, 
            {0, 7, 4} 
        }; 

        // Koordinat petak kosong dalam konfigurasi awal
           int a = 1, b = 2; 
            solve(initial, a, b, final); 
                return 0; 
        }
DFS

DFS adalah metode Pencarian dilakukan pada suatu simpul dalam setiap level dari yang paling kiri. Jika pada level yang terdalam, solusi masih belum juga ditemukan, maka pencarian dilanjutkan pada simpul sebelah kanan dan simpul yang kiri (yang sudah dipindai) dapat dihapus dari memori. Jika pada level yang paling dalam tidak ditemukan solusi, maka pencarian dilanjutkan pada level sebelumnya, dan seterusnya hingga ditemukan solusi.

/*Menggunakan branch and bound yang merupakan algoritma
          Breath First Search yang berjalan layaknya penggunaan Stack */

        #include <bits/stdc++.h> 
        using namespace std; 
        #define M 3 

 	struct Node 
        { 
        // menyimpan parent node dari current node
        // membantu dalam melacak jejak ketika jawabannya ditemukan
        Node* parent; 

        // menyimpan matrix 
        int mat[M][M]; 

        // menyimpan kotak koordinat kosong 
        int a, b; 

        // menyimpan jumlah kotak yang salah tempat
        int cost; 

        // menyimpan jumlah gerakan sejauh ini
        int level; 
        }; 

        // Fungsi untuk mencetak M x M matrix 
        int printMatrix(int mat[M][M]) 
        { 
        for (int i = 0; i < M; i++){ 
            for (int j = 0; j < M; j++) 
            printf("%d ", mat[i][j]); 
            printf("\n"); 
            } 
        }       

        //Berfungsi untuk mengalokasikan node baru
        Node* newNode(int mat[M][M], int a, int b, int newA, 
            int newB, int level, Node* parent) 
        { 
            Node* node = new Node; 

        // atur pointer dari path ke root 
            node->parent = parent; 

        // copy data dari parent node ke current node 
            memcpy(node->mat, mat, sizeof node->mat); 

        //Pindahkan kotak dengan 1 posisi 
            swap(node->mat[a][b], node->mat[newA][newB]); 

        //Atur jumlah kotak yang salah tempat 
             node->cost = INT_MAX; 

        //Atur jumlah gerakan sejauh ini
            node->level = level; 

        //Perbarui koordinat kotak kosong baru
            node->a = newA; 
            node->b = newB; 
        return node; 
        } 

        //bawah, kiri, atas, kanan
            int row[] = { 1, 0, -1, 0 }; 
            int col[] = { 0, -1, 0, 1 }; 

     // Fungsi untuk memeriksa apakah (a, b) adalah koordinat matriks yang valid
             int isSafe(int a, int b) 
        { 
             return (a >= 0 && a < M && b >= 0 && b < M); 
        } 

        // mencetak path dari root node ke destination node 
            void printPath(Node* root){ 
                if (root == NULL) 
            return; 
                printPath(root->parent); 
                printMatrix(root->mat);     
                printf("\n"); 
        } 

	bool testing (int mat[M][M],int final[M][M]){
 	for (int i = 0; i < M; i++) 
	{ 
		for (int j = 0; j < M; j++) {
			if (mat [i] [j] != final [i] [j]){
				return false;
				} 
			}
		}
 		return true;
 	}

 	/* Berfungsi untuk memecahkan algoritma puzzle M * M - 1 menggunakan
           Branch dan Bound. a dan b adalah koordinat kotak kosong
           dalam kondisi awal */
          void solve(int initial[M][M], int a, int b, 
          int final[M][M]) 
          { 
        // Buat prioritas stack untuk menyimpan live nodes dari search tree  
            stack <Node*> pq;

        // buat root node and menghitung biayanya
           Node* root = newNode(initial, a, b, a, b, 0, NULL); 

        // Tambahkan root ke daftar live node 
            pq.push(root); 

        /* Menemukan live node dengan biaya paling sedikit,
          tambahkan childerns ke daftar live node dan
          menghapusnya dari daftar */
          
        while (!pq.empty()) 
        { 
        // Temukan live node dengan perkiraan biaya terendah
            Node* min = pq.front(); 

        // Node yang ditemukan dihapus dari daftar node hidup
        pq.pop(); 

        // jika min adalah answer node;
        if (testing (min->mat, final)) 
        { 
        // cetak path dari root ke destination; 
            	printPath(min); 
			printf ("Move: %d",min->level);
			return;
        } 

        /* lakukan untuk setiap child min
          maks 4 child untuk node */ 
        for (int i = 0; i < 4; i++) 
        { 
            if (isSafe(min->a + row[i], min->b + col[i])) 
        { 
        // buat child node dan menghitung harganya 
            Node* child = newNode(min->mat, min->a, 
            min->b, min->a + row[i], 
            min->b + col[i], 
            min->level + 1, min); 

        //Tambahkan child ke daftar dari live node
            pq.push(child); 
            } 
          } 
         } 
        } 
 
        int main() 
        { 
        // Konfigurasi awal 
        // Nilai 0 digunakan untuk ruang kosong 
            int initial[M][M] = 
        { 
            {1, 2, 3}, 
            {5, 6, 0}, 
            {7, 8, 4} 
        }; 

        //Konfigurasi final yang dapat dipecahkan
        //Nilai 0 digunakan untuk ruang kosong
        int final[M][M] = 
        { 
            {1, 2, 3}, 
            {5, 8, 6}, 
            {0, 7, 4} 
        }; 

        // Koordinat petak kosong dalam konfigurasi awal
           int a = 1, b = 2; 
            solve(initial, a, b, final); 
                return 0; 
        }
IDS

IDS adalah metode pencarian ruang strategi di mana pencarian mendalam-terbatas dijalankan berulang kali. IDS setara dengan luas-pertama pencarian, tetapi menggunakan memori lebih sedikit pada setiap iterasi.
