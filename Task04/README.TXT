#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct {
    int id;
    char group[4];
} Donor;

typedef struct {
    Donor *data;
    int size, capacity;
} DonorList;

int add_donor(DonorList *list, int id, const char *group) {
    if (list == NULL || group == NULL)
        return 0;

    if (list->size == list->capacity) {
        int newCapacity = (list->capacity == 0) ? 2 : list->capacity * 2;

        Donor *p = realloc(list->data,
                           (size_t)newCapacity * sizeof(Donor));

        if (p == NULL)
            return 0;

        /* Old pointer remains valid if realloc fails */
        list->data = p;
        list->capacity = newCapacity;
    }

    list->data[list->size].id = id;

    strncpy(list->data[list->size].group, group, 3);
    list->data[list->size].group[3] = '\0';

    list->size++;

    return 1;
}

int main(void) {
    DonorList list = {NULL, 0, 0};

    const char *groups[] = {"O+", "A+", "B-", "AB+", "O-"};

    for (int i = 0; i < 5; i++) {
        if (!add_donor(&list, 7001 + i, groups[i])) {
            fprintf(stderr, "Memory allocation failed\n");
            free(list.data);
            return 1;
        }

        printf("Added %d (%s): size=%d, capacity=%d\n",
               list.data[i].id,
               list.data[i].group,
               list.size,
               list.capacity);
    }

    free(list.data);

    return 0;
}
