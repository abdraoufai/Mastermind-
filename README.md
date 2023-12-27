
#include <stdio.h>

#include <stdlib.h>

#include <string.h>

int compare(const void *a, const void *b) {

    return (*(char *)a - *(char *)b);

}




void printSubsets(char *arr, int n, int r) {

    char data[r + 1];

    data[r] = '\0';


    void combinationUtil(char *arr, int n, int r, int index, int data_index, char *data) {

        if (data_index == r) {

            printf("%s\n", data);

            return;

        }


        for (int i = index; i < n && n - i >= r - data_index; i++) {

            data[data_index] = arr[i];

            combinationUtil(arr, n, r, i + 1, data_index + 1, data);

        }

    }


    combinationUtil(arr, n, r, 0, 0, data);

}


int main() {

    char drawn_letters[] = "abcdefghij";

    int length = strlen(drawn_letters);



    qsort(drawn_letters, length, sizeof(char), compare);

    for (int r = length; r >= 2; r--) {

        printf("Subsets of length %d:\n", r);

        printSubsets(drawn_letters, length, r);

        printf("\n");

    }


    return 0;

}
