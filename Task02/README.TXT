#include <stdio.h>
#include <string.h>

int main(void) {
    char name[50];

    printf("Donor name: ");

    if (fgets(name, sizeof(name), stdin) == NULL) {
        fprintf(stderr, "Input error\n");
        return 1;
    }

    name[strcspn(name, "\n")] = '\0';

    FILE *fp = fopen("donors.txt", "r");

    if (fp == NULL) {
        perror("fopen donors.txt");
        return 1;
    }

    fclose(fp);

    return 0;
}
