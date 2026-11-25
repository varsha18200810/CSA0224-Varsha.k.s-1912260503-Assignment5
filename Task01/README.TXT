#include <stdio.h>

long long calls = 0;

long long power_linear(long long base, int exp) {
    calls++;

    if (exp == 0)
        return 1;

    return base * power_linear(base, exp - 1);
}

long long power_fast(long long base, int exp) {
    calls++;

    if (exp == 0)
        return 1;

    long long half = power_fast(base, exp / 2);  // Exactly once

    long long result = half * half;

    if (exp % 2 != 0)
        result *= base;

    return result;
}

int main(void) {
    calls = 0;

    long long a = power_linear(2, 30);

    printf("Linear: %lld in %lld calls\n", a, calls);

    calls = 0;

    long long b = power_fast(2, 30);

    printf("Fast: %lld in %lld calls\n", b, calls);

    return 0;
}
