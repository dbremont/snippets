# Multithreading Bug (Non-Volatile Variable)

```c
#include <stdio.h>
#include <pthread.h>

int flag = 0;  // Non-volatile variable, can be optimized by the compiler

void* thread1(void* arg) {
    // Do some work...
    flag = 1;  // Signal thread2
    return NULL;
}

void* thread2(void* arg) {
    // Wait for flag to become 1
    while (flag == 0) {
        // Compiler might optimize this loop, assuming flag is not changing
        // in memory, causing thread2 to wait indefinitely
    }
    printf("Flag was set by thread1\n");
    return NULL;
}

int main() {
    pthread_t t1, t2;
    
    // Start two threads
    pthread_create(&t1, NULL, thread1, NULL);
    pthread_create(&t2, NULL, thread2, NULL);
    
    pthread_join(t1, NULL);
    pthread_join(t2, NULL);
    
    return 0;
}
```
