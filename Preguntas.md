Manejo de Punteros
Laboratorio Calificado
Jose M. Saavedra y David Miranda
11 de septiembre


P1
Necesitamos implementar la funci´on swap, que recibe dos variables. La funci´on swap debe intercambiar los
valores de las variables de entrada. Implementa dos versiones de la funci´on swap, de modo que se pueda
invocar como aparece comentado en el c´odigo Cod 1.


```c
1 # include < iostream >
2 // swap_v1
3
4 // swap_v2
5
6
7 int main ( int nargs , char * vargs []) {
8
9 int a = 10;
10 int b = 20;
11 swap_v1 (& a , & b ) ;
12 swap_v2 (a , b ) ;
13 std :: cout << " a : " << a << " b : " << b << std :: endl ;
14 return 0;
15 }

```

RESPUESTA:

```c
#include <iostream>

void swap_v1(int* a, int* b) {
    int auxiliar = *a;
    *a = *b;
    *b = auxiliar;
}


void swap_v2(int& a, int& b) {
    int auxiliar = a;
    a = b;
    b = auxiliar;
}
```

  
P2
Implementa RadixSort (ascendente) seg´un el esquema de Cod 2

```c
1 # include < iostream >
2
3 int main ( int nargs , char * vargs []) {
4 int ** A = new int *[ n ];
5 int k = 3;
6 int n = 4;
7
8 for ( int i = 0; i < n ; i ++) {
9 A [ i ] = new int [ k ];
10 }
11
12 A [0][0] = 3; A [0][1] = 2; A [0][2] = 5; // 325
13 A [1][0] = 2; A [1][1] = 1; A [1][2] = 8; // 218
14 A [2][0] = 5; A [2][1] = 2; A [2][2] = 3; // 523
15 A [3][0] = 1; A [3][1] = 1; A [3][2] = 3; // 113
16 radix (A , n , k ) ;
17 print (A , n , k ) ;
18 }
```     
RESPUESTA:

 ```c
# include <iostream>

void countingSort(int** A, int n, int d) {
    int** B = new int*[n];
    int count[10] = {0};

    for (int i = 0; i < n; i++) {
        count[A[i][d]]++;
    }

    for (int i = 1; i < 10; i++) {
        count[i] += count[i - 1];
    }

    for (int i = n - 1; i >= 0; i--) {
        B[count[A[i][d]] - 1] = A[i];
        count[A[i][d]]--;
    }

    for (int i = 0; i < n; i++) {
        A[i] = B[i];
    }

    delete[] B;
}

void radix(int** A, int n, int k) {
    for (int d = k - 1; d >= 0; d--) {
        countingSort(A, n, d);
    }
}

void print(int** A, int n, int k) {
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < k; j++) {
            std::cout << A[i][j];
        }
        std::cout << "\n";
    }
}

int main(int nargs, char* vargs[]) {
    int k = 3;
    int n = 4;
    int** A = new int*[n];

    for (int i = 0; i < n; i++) {
        A[i] = new int[k];
    }

    A[0][0] = 3; A[0][1] = 2; A[0][2] = 5; // 325
    A[1][0] = 2; A[1][1] = 1; A[1][2] = 8; // 218
    A[2][0] = 5; A[2][1] = 2; A[2][2] = 3; // 523
    A[3][0] = 1; A[3][1] = 1; A[3][2] = 3; // 113

    radix(A, n, k);
    print(A, n, k);

    return 0;
}
 ```    

 P3
Indica el error en el c´odigo de Cod 3, ¿qu´e ocurre con tal c´odigo?.
```c
1
2 # include < iostream >
3 int main ( int nargs , char * vargs []) {
4
5 int * A = new int [10];
6 int * B = A ;
7 for ( int i = 0; i < 10; i ++) {
8 A [ i ] = i ;
9 }
10 delete [] A ;
11 delete [] B ;
12 return 0;
13 }
    ```
RESPUESTA:
El error es que se intenta liberar la memoria dos veces. Como B apunta a la misma dirección que A, al hacer delete[] A esa memoria ya se liberó, y al hacer delete[] B el programa se cae con un error de ejecución porque intenta borrar memoria que ya no existe.

P4
Si se crea un arreglo como char* A = new char[10], y A empieza en la direcci´on 230, entonces ¿cu´al ser´a la
direcci´on de A[2]?. Verifica que tu respuesta es correcta implementado un peque˜no programa que te muestre
las direcciones de A, A[0], A[1], ..., A[9].

Respuesta:

La dirección de A[2] será 232. Esto ocurre porque el tipo de dato char ocupa exactamente 1 byte de memoria, por lo que cada posición consecutiva en el arreglo avanza de 1 en 1 a partir de la dirección inicial (A[0] en 230, A[1] en 231 y A[2] en 232). 

```c
#include <iostream>

int main() {
    char* A = new char[10];

    std::cout << "Dir A: " << (void*)A << "\n";
    for (int i = 0; i < 10; i++) {
        std::cout << "Dir A[" << i << "]: " << (void*)&A[i] << "\n";
    }

    delete[] A;
    return 0;
} 
``` 


P5
La funci´on createArray de Cod 4 tiene como objetivo crear un espacio de memoria para un arreglo de entrada
e inicializar los valores en -1. Sin embargo, el c´odigo no funciona como lo esperas. Identifica error y corrige
el c´odigo. Ahora, piensa en una soluci´on diferente a la que has propuesto.

```c    
1
2 # include < iostream >
3
4 void createArray ( int * A , int n ) {
5 A = new int [ n ];
6 for ( int i = 0; i < n ; i ++) {
7 A [ i ] = -1;
8 }
9 }
10
11 void print ( int * A , int n ) {
12 for ( int i = 0; i < n ; i ++) {
13 std :: cout << A [ i ] << " " ;
14 }
15 }
16
17 int main ( int nargs , char * vargs []) {
18 int * B = nullptr ;
19 int n = 10;
20 createArray (B , n ) ;
21 print (B , n ) ;
22 return 0;
23 }
Cod. 4: createArray


RESPUESTA:

El error es que el puntero se pasa por copia ya que solo se modifica la copia local y el puntero B del main sigue siendo nullptr, por lo que al hacer print(B, n) el programa falla


La correcion para que funcione sera pasar el puntero por referencia (int*&):

```cpp
void createArray(int*& A, int n) {
    A = new int[n];
    for (int i = 0; i < n; i++) {
        A[i] = -1;
    }
}
```

P6:

No lo entendi como podia implementarlo