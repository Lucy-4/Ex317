#include <stdio.h>
#include <string.h>
#include <mpi.h>

#define MASTER_RANK 0

int main(int argc, char **argv) {
    MPI_Init(&argc, &argv);

    int rango, tam, i;
    char palabra[100];
    char invertido[100];
    int es_palindrome = 1;

    MPI_Comm_rank(MPI_COMM_WORLD, &rango);
    MPI_Comm_size(MPI_COMM_WORLD, &tam);

    if (rango == MASTER_RANK) {
        printf("Introduce una palabra: ");
        scanf("%s", palabra);
    }

    MPI_Bcast(&palabra, 100, MPI_CHAR, MASTER_RANK, MPI_COMM_WORLD);

    int len = strlen(palabra);
    int fragmento= len / tam;
    int ini = rango * fragmento;
    int fin = (rango + 1) * fragmento;

    for (i = ini; i < fin; i++) {
        invertido[len - 1 - i] = palabra[i];
    }

    MPI_Allgather(&invertido[ini], fragmento, MPI_CHAR, &invertido, fragmento, MPI_CHAR, MPI_COMM_WORLD);

    if (rango == MASTER_RANK) {
        for (i = 0; i < len; i++) {
            if (palabra[i] != invertido[i]) {
                es_palindrome = 0;
                break;
            }
        }

        if (es_palindrome) {
            printf("%s es un palindromo.\n", palabra);
        } else {
            printf("%s no es un palindromo.\n", palabra);
        }
    }

    MPI_Finalize();
    return 0;
}
