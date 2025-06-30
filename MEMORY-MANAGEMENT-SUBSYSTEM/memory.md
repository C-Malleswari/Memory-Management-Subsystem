# Source Code Listing

## `memory/file.txt`

```
It surrounds us with beauty, peace, and life itself. From the gentle rustling of leaves in the wind to the powerful roar of oceans and waterfalls, nature speaks in a language that touches the soul. It provides us with fresh air, clean water, food, and countless resources to survive. Beyond its utility, nature inspires creativity, offers healing, and teaches us balance. But in return, it asks for respect and care. Preserving nature is not just an environmental responsibility — it’s a duty to future generations and to the earth that sustains us all.

```

## `memory/m1.c`

```c

#include <stdio.h>
#include <stdlib.h>  // Required for malloc() and free()

int main() 
{
    int *ptr;
    int n, i;

    printf("Enter number of elements: ");
    scanf("%d", &n);

    ptr = (int *)malloc(n * sizeof(int));

    if (ptr == NULL) 
    {
        printf("Memory allocation failed!\n");
        return 1;
    }

    printf("Enter %d integers:\n", n);
    for (i = 0; i < n; i++) 
    {
        printf("Element %d: ", i + 1);
        scanf("%d", &ptr[i]);
    }
    printf("You entered:\n");
    for (i = 0; i < n; i++)
    {
        printf("%d ", ptr[i]);
    }
    free(ptr);

    return 0;
}
/*
#include <stdio.h>
#include <stdlib.h>

int main() 
{
    int *ptr = malloc(5 * sizeof(int));  // Allocate memory for 5 integers

    if (ptr == NULL) 
    {
        printf("Memory allocation failed\n");
        return 1;
    }

    // Use the memory (e.g., store values)
    for (int i = 0; i < 5; i++) 
    {
        ptr[i] = i + 1;
    }

    // Deallocate the memory
    free(ptr);  // Very important!

    return 0;
}
*/
```

## `memory/m2.c`
// internal fragmentation using structure
#include <stdio.h>

struct WithoutPadding 
{
    char a;     // 1 byte
    int b;      // 4 bytes
    char c;     // 1 byte
};

struct WithPadding 
{
    int b;      // 4 bytes
    char a;     // 1 byte
    char c;     // 1 byte
};

int main() 
{
    printf("Size of struct WithoutPadding: %lu bytes\n", sizeof(struct WithoutPadding));
    printf("Size of struct WithPadding: %lu bytes\n", sizeof(struct WithPadding));
    return 0;
}

```

##  `memory/m3.c`
```c
//external fragmentation
#include <stdio.h>
#include <stdlib.h>

int main() 
{
    char *block1, *block2, *block3, *large_block;

    // Step 1: Allocate 3 small blocks (1 KB each) 2^10 =1024 which is equal to 1KB.
    block1 = (char *)malloc(1024);  // 1 KB
    block2 = (char *)malloc(1024);  // 1 KB
    block3 = (char *)malloc(1024);  // 1 KB

    if (!block1 || !block2 || !block3) 
    {
        printf("Memory allocation failed.\n");
        return 1;
    }

    printf("Step 1: Allocated 3 blocks of 1 KB each.\n");

    // Step 2: Free the middle block (creates a hole in memory)
    free(block2);
    printf("Step 2: Freed the middle block (block2).\n");

    // Step 3: Try to allocate a big block (2 KB)
    large_block = (char *)malloc(2048);  // 2 KB

    if (large_block == NULL) 
    {
        printf("Step 3:  Could not allocate 2 KB block — possible external fragmentation.\n");
    }
    else 
    {
        printf("Step 3:  Successfully allocated 2 KB block.\n");
        free(large_block);
    }

    // Step 4: Free remaining blocks
    free(block1);
    free(block3);

    return 0;
}


```

## `memory/m4.c`

```c
//use sizeof() to get know actual bytes
#include <stdio.h>
#include <stdlib.h>

int main() 
{
    int x;
    char ch;
    float f;
    double d;

    printf("Size of int: %lu bytes\n", sizeof(x));
    printf("Size of char: %lu byte\n", sizeof(ch));
    printf("Size of float: %lu bytes\n", sizeof(f));
    printf("Size of double: %lu bytes\n", sizeof(d));

    // Allocate memory for 5 integers
    int *arr = (int *)malloc(5 * sizeof(int));

    if (arr == NULL)
   {
        printf("Memory allocation failed!\n");
        return 1;
    }

    printf("Size of one int: %lu bytes\n", sizeof(int));
    printf("Allocated memory: %lu bytes (for 5 integers)\n", 5 * sizeof(int));

    free(arr);  // Release memory

    return 0;
}


```

## `memory/m5.c`

```c
//to get offset value in virtual address
#include <stdio.h>

int main() 
{
    int virtual_address = 12088;//decimal
    int page_size = 4096;//bytes

    int offset = virtual_address % page_size;

    printf("Offset inside the page: %d bytes\n", offset);
    return 0;
}


```

## `memory/m6.c`

```c
// to get virtual address
// Virtual Address = (Page Number × Page Size) + Offset
/*
#include <stdio.h>

int main()
{
    int page_number = 2;
    int offset = 3896;
    int page_size = 4096;

    int virtual_address = (page_number * page_size) + offset;

    printf("Virtual Address: %d (Decimal)\n", virtual_address);
    printf("Virtual Address: 0x%X (Hexadecimal)\n", virtual_address);

    return 0;
}
*/
#include <stdio.h>

int main() 
{
    int page_number, offset, page_size;

    // Get inputs from the user
    printf("Enter page size (in bytes): ");
    scanf("%d", &page_size);

    printf("Enter page number: ");
    scanf("%d", &page_number);

    printf("Enter offset (in bytes): ");
    scanf("%d", &offset);

    // Validation: offset should not exceed page size
    if (offset >= page_size)
    {
        printf(" Error: Offset must be less than the page size.\n");
        return 1;
    }

    // Calculate virtual address
    int virtual_address = (page_number * page_size) + offset;

    // Display result
    printf("\n  Virtual Address Calculation:\n");
    printf("Page Number: %d\n", page_number);
    printf("Offset     : %d bytes\n", offset);
    printf("Virtual Address: %d (Decimal)\n", virtual_address);
    printf("Virtual Address: 0x%X (Hexadecimal)\n", virtual_address);

    return 0;
}


```

## `memory/m7.c`

```c
// to calculate physical address
#include <stdio.h>

int main() 
{
    int page_size = 4096; // 4KB page size
    int page_number, offset;
    int frame_number, physical_address;

    // Step 1: A small predefined page table
    // page_table[page_number] gives frame_number
    int page_table[4] = {5, 3, 9, 1};  // Page 0→Frame 5, Page 1→Frame 3, etc.

    // Step 2: Take user input
    printf("Enter page number (0–3): ");
    scanf("%d", &page_number);

    printf("Enter offset (0–%d): ", page_size - 1);
    scanf("%d", &offset);

    // Step 3: Validate input
    if (page_number < 0 || page_number >= 4 || offset >= page_size) 
    {
        printf(" Invalid page number or offset.\n");
        return 1;
    }

    // Step 4: Look up frame number from page table
    frame_number = page_table[page_number];

    // Step 5: Calculate physical address
    physical_address = (frame_number * page_size) + offset;

    // Step 6: Show result
    printf("\n  Result:\n");
    printf("Page Number      : %d\n", page_number);
    printf("Offset           : %d\n", offset);
    printf("Frame Number     : %d (from page table)\n", frame_number);
    printf("Physical Address : %d (decimal)\n", physical_address);
    printf("Physical Address : 0x%X (hexadecimal)\n", physical_address);

    return 0;
}

```

## `memory/m8.c`

```c
//Implement a C program to allocate memory for an array dynamically using malloc().
#include <stdio.h>
#include <stdlib.h>  // Required for malloc and free

int main() 
{
    int *arr;
    int n, i;

    printf("Enter number of elements: ");
    scanf("%d", &n);

    arr = (int *)malloc(n * sizeof(int));
    if (arr == NULL) 
    {
        printf("Memory allocation failed!\n");
        return 1;
    }

    printf("Enter %d elements:\n", n);
    for (i = 0; i < n; i++)
    {
        scanf("%d", &arr[i]);
    }

    printf("the elements :\n");
    for (i = 0; i < n; i++)
    {
        printf("%d ", arr[i]);
    }
    printf("\n");
    free(arr);

    return 0;
}


```

## `memory/m9.c`

```c
//Implement a C program to allocate memory for an array dynamically using calloc().
#include <stdio.h>
#include <stdlib.h>  // For calloc() and free()

int main() 
{
    int *arr;
    int n, i;

    printf("Enter number of elements: ");
    scanf("%d", &n);

    arr = (int *)calloc(n, sizeof(int));

    if (arr == NULL) 
    {
        printf("Memory allocation failed!\n");
        return 1;
    }

    printf("Initial array values (from calloc):\n");
    for (i = 0; i < n; i++) {
        printf("%d ", arr[i]);  // All values should be 0
    }
    printf("\n");

    printf("Enter %d elements:\n", n);
    for (i = 0; i < n; i++) 
    {
        scanf("%d", &arr[i]);
    }

    printf("the elements are:\n");
    for (i = 0; i < n; i++)
    {
        printf("%d ", arr[i]);
    }
    printf("\n");
    free(arr);
}

```

## `memory/m10.c`

```c
//Implement a C program to allocate memory for an array dynamically using realloc()
#include <stdio.h>
#include <stdlib.h>

int main() 
{
    int *arr;
    int n, new_size, i;

    printf("Enter initial number of elements: ");
    scanf("%d", &n);

    arr = (int *)malloc(n * sizeof(int));

    if (arr == NULL) 
    {
        printf("Initial memory allocation failed!\n");
        return 1;
    }

    printf("Enter %d elements:\n", n);
    for (i = 0; i < n; i++) 
    {
        scanf("%d", &arr[i]);
    }

    printf("Original array:\n");
    for (i = 0; i < n; i++) 
    {
        printf("%d ", arr[i]);
    }
    printf("\n");

    printf("Enter new size of array: ");
    scanf("%d", &new_size);

    arr = (int *)realloc(arr, new_size * sizeof(int));

    if (arr == NULL) 
    {
        printf("Memory reallocation failed!\n");
        return 1;
    }
    
    if (new_size > n) 
    {
        printf("Enter %d more elements:\n", new_size - n);
        for (i = n; i < new_size; i++) 
	    {
            scanf("%d", &arr[i]);
        }
    }

    printf("the elements:\n");
    for (i = 0; i < new_size; i++) 
    {
        printf("%d ", arr[i]);
    }
    printf("\n");
    free(arr);

    return 0;
}


```

## `memory/m11.c`

```c
//Develop a program in C to allocate memory for a linked list node dynamically
#include <stdio.h>
#include <stdlib.h>

struct Node 
{
    int data;
    struct Node *next;
};

int main() 
{
    struct Node *head = NULL;

    head = (struct Node *)malloc(sizeof(struct Node));

    if (head == NULL) 
    {
        printf("Memory allocation failed!\n");
        return 1;
    }

    head->data = 10;
    head->next = NULL;

    printf("Data in the node: %d\n", head->data);

    free(head);

    return 0;
}


```

## `memory/m12.c`

```c
//Implement a C program to simulate memory allocation using the first-fit algorithm.
#include <stdio.h>

#define MAX_BLOCKS 10
#define MAX_PROCESSES 10

int main() 
{
    int blocks[MAX_BLOCKS], processes[MAX_PROCESSES];
    int blockCount, processCount;
    int assignedBlock[MAX_PROCESSES];  // Stores assigned block index for each process

    // Step 1: Input memory blocks
    printf("Enter total number of memory blocks: ");
    scanf("%d", &blockCount);

    printf("Enter size of each block:\n");
    for (int i = 0; i < blockCount; i++) 
    {
        printf("Block %d size: ", i + 1);
        scanf("%d", &blocks[i]);
    }

    // Step 2: Input processes
    printf("\nEnter total number of processes: ");
    scanf("%d", &processCount);

    printf("Enter memory required for each process:\n");
    for (int i = 0; i < processCount; i++) 
    {
        printf("Process %d size: ", i + 1);
        scanf("%d", &processes[i]);
        assignedBlock[i] = -1;  // Default: Not allocated
    }

    // Step 3: First-Fit Allocation Logic
    for (int i = 0; i < processCount; i++) 
    {
        for (int j = 0; j < blockCount; j++) 
        {
            if (blocks[j] >= processes[i]) 
            {
                assignedBlock[i] = j;           // Assign block
                blocks[j] -= processes[i];      // Reduce available size
                break;                          // Move to next process
            }
        }
    }

    // Step 4: Display results
    printf("\nProcess\tSize\tAllocated Block\n");
    for (int i = 0; i < processCount; i++) 
    {
        printf("P%d\t%d\t", i + 1, processes[i]);
        if (assignedBlock[i] != -1)
            printf("Block %d\n", assignedBlock[i] + 1);
        else
            printf("Not Allocated\n");
    }

    return 0;
}


```

## `memory/m13.c`

```c
//Implement a C program to simulate memory allocation using the best-fit algorithm.
#include <stdio.h>

#define MAX_BLOCKS 10
#define MAX_PROCESSES 10

int main() 
{
    int blocks[MAX_BLOCKS], processes[MAX_PROCESSES];
    int blockCount, processCount;
    int assignedBlock[MAX_PROCESSES]; // Stores the assigned block for each process

    // Step 1: Get memory block information
    printf("Enter number of memory blocks: ");
    scanf("%d", &blockCount);

    printf("Enter sizes of each block:\n");
    for (int i = 0; i < blockCount; i++) 
    {
        printf("Block %d: ", i + 1);
        scanf("%d", &blocks[i]);
    }

    // Step 2: Get process information
    printf("\nEnter number of processes: ");
    scanf("%d", &processCount);

    printf("Enter sizes of each process:\n");
    for (int i = 0; i < processCount; i++) 
    {
        printf("Process %d: ", i + 1);
        scanf("%d", &processes[i]);
        assignedBlock[i] = -1; // Default: not allocated
    }

    // Step 3: Best-Fit Allocation
    for (int i = 0; i < processCount; i++) 
    {
        int bestIndex = -1;

        for (int j = 0; j < blockCount; j++) 
        {
            if (blocks[j] >= processes[i]) 4
            {
                if (bestIndex == -1 || blocks[j] < blocks[bestIndex]) 
                {
                    bestIndex = j; // Find smallest fitting block
                }
            }
        }

        if (bestIndex != -1) 
        {
            // Allocate block and reduce its size
            assignedBlock[i] = bestIndex;
            blocks[bestIndex] -= processes[i];
        }
    }

    // Step 4: Print Allocation Result
    printf("\nProcess\tSize\tAllocated Block\n");
    for (int i = 0; i < processCount; i++) 
    {
        printf("P%d\t%d\t", i + 1, processes[i]);
        if (assignedBlock[i] != -1)
            printf("Block %d\n", assignedBlock[i] + 1);
        else
            printf("Not Allocated\n");
    }

    return 0;
}


```

## `memory/m14.c`

```c
//Develop a C program to simulate memory allocation using the worst-fit algorithm.
#include <stdio.h>

#define MAX_BLOCKS 10
#define MAX_PROCESSES 10

int main() 
{
    int blocks[MAX_BLOCKS], processes[MAX_PROCESSES];
    int blockCount, processCount;
    int assignedBlock[MAX_PROCESSES];  // Stores assigned block index for each process

    // Step 1: Get memory block info
    printf("Enter number of memory blocks: ");
    scanf("%d", &blockCount);

    printf("Enter sizes of each block:\n");
    for (int i = 0; i < blockCount; i++) 
    {
        printf("Block %d: ", i + 1);
        scanf("%d", &blocks[i]);
    }

    // Step 2: Get process info
    printf("\nEnter number of processes: ");
    scanf("%d", &processCount);

    printf("Enter sizes of each process:\n");
    for (int i = 0; i < processCount; i++) 
    {
        printf("Process %d: ", i + 1);
        scanf("%d", &processes[i]);
        assignedBlock[i] = -1; // Initially not allocated
    }

    // Step 3: Worst-Fit Allocation
    for (int i = 0; i < processCount; i++) 
    {
        int worstIndex = -1;

        for (int j = 0; j < blockCount; j++) 
        {
            if (blocks[j] >= processes[i]) 
            {
                if (worstIndex == -1 || blocks[j] > blocks[worstIndex]) 
                {
                    worstIndex = j; // Select the largest suitable block
                }
            }
        }

        if (worstIndex != -1) 
        {
            assignedBlock[i] = worstIndex;
            blocks[worstIndex] -= processes[i];
        }
    }

    // Step 4: Display result
    printf("\nProcess\tSize\tAllocated Block\n");
    for (int i = 0; i < processCount; i++) 
    {
        printf("P%d\t%d\t", i + 1, processes[i]);
        if (assignedBlock[i] != -1)
            printf("Block %d\n", assignedBlock[i] + 1); // 1-based index
        else
            printf("Not Allocated\n");
    }

    return 0;
}


```

## `memory/m15.c`

```c
//Implement a C program to simulate memory allocation using the next-fit algorithm.
#include <stdio.h>

#define MAX 10

int main() 
{
    int blockSize[MAX], processSize[MAX], allocation[MAX];
    int blockCount, processCount;
    int lastAllocatedIndex = 0; // Keeps track of where we stopped last time

    // Input block count and sizes
    printf("Enter number of memory blocks: ");
    scanf("%d", &blockCount);
    printf("Enter sizes of %d memory blocks:\n", blockCount);
    for (int i = 0; i < blockCount; i++) 
    {
        printf("Block %d: ", i + 1);
        scanf("%d", &blockSize[i]);
    }

    // Input process count and sizes
    printf("Enter number of processes: ");
    scanf("%d", &processCount);
    printf("Enter sizes of %d processes:\n", processCount);
    for (int i = 0; i < processCount; i++) 
    {
        printf("Process %d: ", i + 1);
        scanf("%d", &processSize[i]);
        allocation[i] = -1; // Default: not allocated
    }

    // Next-Fit Allocation Logic
    for (int i = 0; i < processCount; i++) 
    {
        int allocated = 0;
        int start = lastAllocatedIndex;
        int j = start;

        do 
        {
            if (blockSize[j] >= processSize[i]) 
            {
                allocation[i] = j;
                blockSize[j] -= processSize[i];
                lastAllocatedIndex = j; // Start next search from here
                allocated = 1;
                break;
            }

            j = (j + 1) % blockCount; // Wrap around if needed
        } while (j != start); // Stop if we come full circle

        if (!allocated) 
        {
            allocation[i] = -1; // Could not find a suitable block
        }
    }

    // Output
    printf("\n--- Next-Fit Allocation ---\n");
    printf("Process\tSize\tBlock\n");
    for (int i = 0; i < processCount; i++) 
    {
        printf("P%d\t%d\t", i + 1, processSize[i]);
        if (allocation[i] != -1)
            printf("Block %d\n", allocation[i] + 1);
        else
            printf("Not Allocated\n");
    }

    return 0;
}


```

## `memory/m16.c`

```c
//Write a C program to implement a simple memory allocator using the buddy system

#include <stdio.h>
#include <math.h>

#define MAX_LEVEL 4  // Max 2^4 = 16 KB total memory
#define MEMORY_SIZE (1 << MAX_LEVEL)  // Total memory size = 16

int memory_tree[1 << (MAX_LEVEL + 1)]; // binary tree for buddy system

// Initialize the memory tree
void init_memory() 
{
    for (int i = 0; i < (1 << (MAX_LEVEL + 1)); i++)
        memory_tree[i] = 0;  // 0 = free, 1 = allocated
}

// Recursive function to allocate memory
int allocate(int index, int level, int required_level) 
{
    if (memory_tree[index] == 1)
        return -1;  // already allocated

    if (level == required_level) 
    {
        memory_tree[index] = 1;
        return index;
    }

    int left = index * 2;
    int right = left + 1;

    int res = allocate(left, level + 1, required_level);
    if (res != -1) 
    {
        memory_tree[index] = 1;
        return res;
    }

    res = allocate(right, level + 1, required_level);
    if (res != -1) 
    {
        memory_tree[index] = 1;
        return res;
    }

    return -1;
}

// Recursive function to free memory
void free_block(int index) 
{
    memory_tree[index] = 0;

    while (index > 1) {
        int buddy = (index % 2 == 0) ? index + 1 : index - 1;
        if (memory_tree[buddy] == 1)
            break;

        index /= 2;
        memory_tree[index] = 0;
    }
}

// Convert requested size to level
int size_to_level(int size) 
{
    int level = 0;
    int block = MEMORY_SIZE;

    while (block > size && level < MAX_LEVEL) 
    {
        block /= 2;
        level++;
    }

    return level;
}

// Print current memory blocks status
void print_status() 
{
    printf("\nMemory Tree (0 = Free, 1 = Allocated):\n");
    for (int i = 1; i < (1 << (MAX_LEVEL + 1)); i++) 
    {
        printf("%d ", memory_tree[i]);
        if ((i & (i + 1)) == 0) printf("\n"); // new level
    }
}

int main() 
{
    int choice, size, index;
    init_memory();

    while (1) 
    {
        printf("\n\n--- Buddy System Menu ---\n");
        printf("1. Allocate memory\n");
        printf("2. Free memory block\n");
        printf("3. Print memory status\n");
        printf("4. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) 
        {
        case 1:
            printf("Enter size to allocate: ");
            scanf("%d", &size);
            if (size > MEMORY_SIZE) 
            {
                printf("Cannot allocate more than %d bytes\n", MEMORY_SIZE);
                break;
            }
            {
                int level = size_to_level(size);
                int result = allocate(1, 0, level);
                if (result != -1)
                    printf("Allocated at node index: %d (Level %d)\n", result, level);
                else
                    printf("Allocation failed\n");
            }
            break;

        case 2:
            printf("Enter node index to free: ");
            scanf("%d", &index);
            if (index < 1 || index >= (1 << (MAX_LEVEL + 1))) 
            {
                printf("Invalid node index\n");
                break;
            }
            free_block(index);
            printf("Block at index %d freed\n", index);
            break;

        case 3:
            print_status();
            break;

        case 4:
            return 0;

        default:
            printf("Invalid choice!\n");
        }
    }
}



```

## `memory/m17.c`

```c
//Develop a C program to implement a memory allocator using a custom memory management algorithm.
#include <stdio.h>
#include <stdint.h>

#define MEMORY_SIZE 1024  // Total simulated memory size
#define HEADER_SIZE sizeof(BlockHeader)

typedef struct {
    size_t size;  // size of block (not including header)
    int free;     // 1 if block is free, 0 if allocated
} BlockHeader;

char memory[MEMORY_SIZE];  // Simulated heap

void initialize_memory() {
    BlockHeader *initial = (BlockHeader *)memory;
    initial->size = MEMORY_SIZE - HEADER_SIZE;
    initial->free = 1;
}

void *my_malloc(size_t size) {
    char *ptr = memory;

    while (ptr < memory + MEMORY_SIZE) {
        BlockHeader *header = (BlockHeader *)ptr;

        if (header->free && header->size >= size) {
            if (header->size > size + HEADER_SIZE) {
                // Split block
                BlockHeader *next = (BlockHeader *)(ptr + HEADER_SIZE + size);
                next->size = header->size - size - HEADER_SIZE;
                next->free = 1;

                header->size = size;
            }

            header->free = 0;
            return ptr + HEADER_SIZE; // Return pointer to usable memory
        }

        ptr += HEADER_SIZE + header->size;
    }

    return NULL;  // Out of memory
}

void my_free(void *ptr) {
    if (!ptr) return;

    BlockHeader *header = (BlockHeader *)((char *)ptr - HEADER_SIZE);
    header->free = 1;

    // Coalesce with next block if free
    char *next_ptr = (char *)ptr + header->size;
    if (next_ptr < memory + MEMORY_SIZE) {
        BlockHeader *next = (BlockHeader *)next_ptr;
        if (next->free) {
            header->size += HEADER_SIZE + next->size;
        }
    }
}

void print_memory_status() {
    char *ptr = memory;
    int block_num = 0;

    printf("\n--- Memory Blocks ---\n");
    printf("Block\tAddress\t\tSize\tStatus\n");

    while (ptr < memory + MEMORY_SIZE) {
        BlockHeader *header = (BlockHeader *)ptr;
        printf("%d\t%p\t%zu\t%s\n", block_num++, ptr, header->size,
               header->free ? "Free" : "Used");

        ptr += HEADER_SIZE + header->size;
    }
}

int main() {
    initialize_memory();

    printf("Custom Memory Allocator Initialized\n");
    print_memory_status();

    void *p1 = my_malloc(100);
    void *p2 = my_malloc(200);
    printf("\nAllocated 100 and 200 bytes\n");
    print_memory_status();

    my_free(p1);
    printf("\nFreed 100 bytes block\n");
    print_memory_status();

    void *p3 = my_malloc(50);
    printf("\nAllocated 50 bytes\n");
    print_memory_status();

    return 0;
}


```

## `memory/m18.c`

```c
//Develop a C program to implement a memory allocator using a custom memory management algorithm
#include <stdio.h>
#include <stdint.h>

#define MEMORY_SIZE 1024  // Total simulated memory size
#define HEADER_SIZE sizeof(BlockHeader)

typedef struct {
    size_t size;  // size of block (not including header)
    int free;     // 1 if block is free, 0 if allocated
} BlockHeader;

char memory[MEMORY_SIZE];  // Simulated heap

void initialize_memory() {
    BlockHeader *initial = (BlockHeader *)memory;
    initial->size = MEMORY_SIZE - HEADER_SIZE;
    initial->free = 1;
}

void *my_malloc(size_t size) 
{
    char *ptr = memory;

    while (ptr < memory + MEMORY_SIZE) {
        BlockHeader *header = (BlockHeader *)ptr;

        if (header->free && header->size >= size) 
        {
            if (header->size > size + HEADER_SIZE) 
            {
                // Split block
                BlockHeader *next = (BlockHeader *)(ptr + HEADER_SIZE + size);
                next->size = header->size - size - HEADER_SIZE;
                next->free = 1;

                header->size = size;
            }

            header->free = 0;
            return ptr + HEADER_SIZE; // Return pointer to usable memory
        }

        ptr += HEADER_SIZE + header->size;
    }

    return NULL;  // Out of memory
}

void my_free(void *ptr) 
{
    if (!ptr) return;

    BlockHeader *header = (BlockHeader *)((char *)ptr - HEADER_SIZE);
    header->free = 1;

    // Coalesce with next block if free
    char *next_ptr = (char *)ptr + header->size;
    if (next_ptr < memory + MEMORY_SIZE) 
    {
        BlockHeader *next = (BlockHeader *)next_ptr;
        if (next->free) 
        {
            header->size += HEADER_SIZE + next->size;
        }
    }
}

void print_memory_status() 
{
    char *ptr = memory;
    int block_num = 0;

    printf("\n--- Memory Blocks ---\n");
    printf("Block\tAddress\t\tSize\tStatus\n");

    while (ptr < memory + MEMORY_SIZE) 
    {
        BlockHeader *header = (BlockHeader *)ptr;
        printf("%d\t%p\t%zu\t%s\n", block_num++, ptr, header->size,
               header->free ? "Free" : "Used");

        ptr += HEADER_SIZE + header->size;
    }
}

int main() 
{
    initialize_memory();

    printf("Custom Memory Allocator Initialized\n");
    print_memory_status();

    void *p1 = my_malloc(100);
    void *p2 = my_malloc(200);
    printf("\nAllocated 100 and 200 bytes\n");
    print_memory_status();

    my_free(p1);
    printf("\nFreed 100 bytes block\n");
    print_memory_status();

    void *p3 = my_malloc(50);
    printf("\nAllocated 50 bytes\n");
    print_memory_status();

    return 0;
}



```

## `memory/m19.c`

```c
//Write a C program to demonstrate memory mapping using mmap().
#include <stdio.h>
#include <stdlib.h>
#include <fcntl.h>      // open
#include <unistd.h>     // close
#include <sys/mman.h>   // mmap, munmap
#include <string.h>     // strcpy

int main() 
{
    const char *filename = "mapped_file.txt";
    const char *text = "Hello from mmap!";
    int fd;
    char *mapped;
    size_t length = 4096; // 4 KB

    // Step 1: Create or open the file
    fd = open(filename, O_RDWR | O_CREAT, 0666);
    if (fd == -1) 
    {
        perror("open");
        return 1;
    }

    // Step 2: Extend file size
    if (ftruncate(fd, length) == -1) 
    {
        perror("ftruncate");
        close(fd);
        return 1;
    }

    // Step 3: Map file to memory
    mapped = mmap(NULL, length, PROT_READ | PROT_WRITE, MAP_SHARED, fd, 0);
    if (mapped == MAP_FAILED) 
    {
        perror("mmap");
        close(fd);
        return 1;
    }

    // Step 4: Write to mapped memory
    strcpy(mapped, text);
    printf("Written to memory-mapped file: %s\n", mapped);

    // Step 5: Unmap and close file
    munmap(mapped, length);
    close(fd);

    return 0;
}
 


```

## `memory/m2.c`

```c
// internal fragmentation using structure
#include <stdio.h>

struct WithoutPadding 
{
    char a;     // 1 byte
    int b;      // 4 bytes
    char c;     // 1 byte
};

struct WithPadding 
{
    int b;      // 4 bytes
    char a;     // 1 byte
    char c;     // 1 byte
};

int main() 
{
    printf("Size of struct WithoutPadding: %lu bytes\n", sizeof(struct WithoutPadding));
    printf("Size of struct WithPadding: %lu bytes\n", sizeof(struct WithPadding));
    return 0;
}


```

## `memory/m20.c`

```c
//Write a C program to demonstrate memory mapping using mmap() in existing file.
#include <stdio.h>
#include <stdlib.h>
#include <fcntl.h>       // open()
#include <unistd.h>      // close()
#include <sys/mman.h>    // mmap(), munmap()
#include <sys/stat.h>    // fstat()

int main() 
{
    const char *filename = "file.txt";
    int fd;
    char *data;
    struct stat fileInfo;

    // Step 1: Open the file
    fd = open(filename, O_RDONLY);
    if (fd == -1) 
    {
        perror("open");
        return 1;
    }

    // Step 2: Get file size
    if (fstat(fd, &fileInfo) == -1) 
    {
        perror("fstat");
        close(fd);
        return 1;
    }

    // Step 3: Memory-map the file
    data = mmap(NULL, fileInfo.st_size, PROT_READ, MAP_PRIVATE, fd, 0);
    if (data == MAP_FAILED) 
    {
        perror("mmap");
        close(fd);
        return 1;
    }

    // Step 4: Print the contents
    write(STDOUT_FILENO, data, fileInfo.st_size);

    // Step 5: Cleanup
    munmap(data, fileInfo.st_size);
    close(fd);

    return 0;
}



```

## `memory/m21.c`

```c
//Implement a C program to read from and write to a memory-mapped file
#include <stdio.h>
#include <stdlib.h>
#include <fcntl.h>      // open()
#include <unistd.h>     // close(), ftruncate()
#include <string.h>     // strcpy()
#include <sys/mman.h>   // mmap(), munmap()
#include <sys/stat.h>   // For file permissions

#define FILESIZE 4096

int main() 
{
    const char *filename = "mymappedfile.txt";
    int fd;
    char *mapped;

    // Step 1: Open or create the file
    fd = open(filename, O_RDWR | O_CREAT, 0666);
    if (fd == -1) 
    {
        perror("open");
        return 1;
    }

    // Step 2: Resize the file
    if (ftruncate(fd, FILESIZE) == -1) 
    {
        perror("ftruncate");
        close(fd);
        return 1;
    }

    // Step 3: Map file to memory
    mapped = mmap(NULL, FILESIZE, PROT_READ | PROT_WRITE, MAP_SHARED, fd, 0);
    if (mapped == MAP_FAILED) {
        perror("mmap");
        close(fd);
        return 1;
    }

    // Step 4: Write to memory
    const char *message = "Hello C. Malleswari! This is written using mmap().";
    strcpy(mapped, message);
    printf("Written to memory-mapped file: %s\n", message);

    // Step 5: Read from memory
    printf("Read from memory-mapped file: %s\n", mapped);

    // Step 6: Cleanup
    munmap(mapped, FILESIZE);
    close(fd);

    return 0;
}


```

## `memory/m22.c`

```c
//Develop a C program to demonstrate shared memory usage using shmget() and shmat().
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/ipc.h>
#include <sys/shm.h>
#include <unistd.h>

#define SHM_SIZE 1024  // 1KB shared memory

int main() 
{
    key_t key;
    int shmid;
    char *shm_ptr;

    // Step 1: Generate a unique key
    key = ftok("shmfile", 65);  // You can use any existing file path
    if (key == -1) {
        perror("ftok");
        exit(1);
    }

    // Step 2: Create shared memory segment
    shmid = shmget(key, SHM_SIZE, 0666 | IPC_CREAT);
    if (shmid == -1) {
        perror("shmget");
        exit(1);
    }

    // Step 3: Attach to the shared memory
    shm_ptr = (char*) shmat(shmid, NULL, 0);
    if (shm_ptr == (char*)(-1)) 
    {
        perror("shmat");
        exit(1);
    }

    // Step 4: Write data into shared memory
    strcpy(shm_ptr, "Hello C. Malleswari, this is shared memory!");
    printf("Data written to shared memory: %s\n", shm_ptr);

    // Step 5: Read back the data
    printf("Data read from shared memory: %s\n", shm_ptr);

    // Step 6: Detach from shared memory
    shmdt(shm_ptr);

    // Step 7 (optional): Destroy the shared memory
    shmctl(shmid, IPC_RMID, NULL);

    return 0;
}


```

## `memory/m23.c`

```c
//Write a C program to create a shared memory segment and synchronize access using semaphores.
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/ipc.h>
#include <sys/shm.h>
#include <sys/sem.h>
#include <unistd.h>

#define SHM_KEY 1234
#define SEM_KEY 5678
#define SHM_SIZE 1024

// Semaphore operation wrappers
void sem_wait(int semid) 
{
    struct sembuf sb = {0, -1, 0}; // P operation
    semop(semid, &sb, 1);
}

void sem_signal(int semid) 
{
    struct sembuf sb = {0, +1, 0}; // V operation
    semop(semid, &sb, 1);
}

int main() 
{
    int shmid, semid;
    char *shm;

    // Step 1: Create shared memory
    shmid = shmget(SHM_KEY, SHM_SIZE, IPC_CREAT | 0666);
    if (shmid == -1) 
    {
        perror("shmget");
        exit(1);
    }

    // Step 2: Attach shared memory
    shm = (char *)shmat(shmid, NULL, 0);
    if (shm == (char *)(-1)) 
    {
        perror("shmat");
        exit(1);
    }

    // Step 3: Create semaphore (1 semaphore)
    semid = semget(SEM_KEY, 1, IPC_CREAT | 0666);
    if (semid == -1) 
    {
        perror("semget");
        exit(1);
    }

    // Step 4: Initialize semaphore to 1
    semctl(semid, 0, SETVAL, 1);

    // Step 5: Lock semaphore (P operation)
    sem_wait(semid);
    printf("Writing to shared memory...\n");

    // Step 6: Write to shared memory
    strcpy(shm, "Hello C. Malleswari! This is synchronized shared memory.");

    sleep(2); // Simulate some delay

    // Step 7: Unlock semaphore (V operation)
    sem_signal(semid);

    printf("Write complete. Data in shared memory: %s\n", shm);

    // Step 8: Detach from shared memory
    shmdt(shm);

    // (Optional) Cleanup
    // shmctl(shmid, IPC_RMID, NULL);
    // semctl(semid, 0, IPC_RMID);

    return 0;
}



```

## `memory/m24.c`

```c
//Implement a C program to simulate page replacement algorithms like FIFO, LRU, and optimal

#include <stdio.h>
#include <stdlib.h>

#define MAX 50

// Function to check if page is in memory
int isInFrame(int page, int frame[], int n) 
{
    for (int i = 0; i < n; i++)
        if (frame[i] == page)
            return 1;
    return 0;
}

// FIFO
int fifo(int pages[], int n, int frames) 
{
    int frame[frames], index = 0, pageFaults = 0;

    for (int i = 0; i < frames; i++) frame[i] = -1;

    for (int i = 0; i < n; i++) 
    {
        if (!isInFrame(pages[i], frame, frames)) 
        {
            frame[index] = pages[i];
            index = (index + 1) % frames;
            pageFaults++;
        }
    }
    return pageFaults;
}

// LRU
int lru(int pages[], int n, int frames) 
{
    int frame[frames], age[frames], time = 0, pageFaults = 0;

    for (int i = 0; i < frames; i++) 
    {
        frame[i] = -1;
        age[i] = 0;
    }

    for (int i = 0; i < n; i++) 
    {
        time++;
        int found = 0;
        for (int j = 0; j < frames; j++) 
        {
            if (frame[j] == pages[i]) 
            {
                age[j] = time;
                found = 1;
                break;
            }
        }

        if (!found) 
        {
            int minTime = age[0], replace = 0;
            for (int j = 1; j < frames; j++) 
            {
                if (age[j] < minTime) 
                {
                    minTime = age[j];
                    replace = j;
                }
            }
            frame[replace] = pages[i];
            age[replace] = time;
            pageFaults++;
        }
    }
    return pageFaults;
}

// Optimal
int optimal(int pages[], int n, int frames) 
{
    int frame[frames], pageFaults = 0;

    for (int i = 0; i < frames; i++) frame[i] = -1;

    for (int i = 0; i < n; i++) 
    {
        if (!isInFrame(pages[i], frame, frames)) 
        {
            int index = -1, farthest = i;

            for (int j = 0; j < frames; j++) 
            {
                int k;
                for (k = i + 1; k < n; k++) 
                {
                    if (frame[j] == pages[k]) break;
                }

                if (k > farthest) 
                {
                    farthest = k;
                    index = j;
                }

                if (k == n) 
                {
                    index = j;
                    break;
                }
            }

            if (index == -1) index = 0;

            frame[index] = pages[i];
            pageFaults++;
        }
    }
    return pageFaults;
}

int main() 
{
    int pages[MAX], n, frames, choice;

    printf("Enter number of page references: ");
    scanf("%d", &n);
    printf("Enter the page reference string:\n");
    for (int i = 0; i < n; i++) 
    {
        scanf("%d", &pages[i]);
    }

    printf("Enter number of frames: ");
    scanf("%d", &frames);

    while (1) 
    {
        printf("\nMenu:\n");
        printf("1. FIFO\n");
        printf("2. LRU\n");
        printf("3. Optimal\n");
        printf("4. Exit\n");
        printf("Choose algorithm: ");
        scanf("%d", &choice);

        switch (choice) 
        {
            case 1:
                printf("FIFO Page Faults = %d\n", fifo(pages, n, frames));
                break;
            case 2:
                printf("LRU Page Faults = %d\n", lru(pages, n, frames));
                break;
            case 3:
                printf("Optimal Page Faults = %d\n", optimal(pages, n, frames));
                break;
            case 4:
                printf("Exiting...\n");
                exit(0);
            default:
                printf("Invalid option!\n");
        }
    }

    return 0;
}


```

## `memory/program.c`

```c
#include <stdio.h>


void rotate_array(int arr[],int n)
{
	int i,j;
	for(i=0,j=0;i<n;i++)
	{
		if(arr[i] %2 == 0)
		{
			int temp =arr[i];
			arr[i]=arr[j];
			arr[j]=temp;
			j++;
		}
	}
}

int main() 
{
    int arr[100], n, i;

    printf("Enter number of elements: ");
    scanf("%d", &n);

    printf("Enter %d elements (even and odd mixed):\n", n);
    for (i = 0; i < n; i++)
    {
     	    scanf("%d", &arr[i]);
    }
    rotate_array(arr,n);
    printf("Array after separating even to left, odd to right:\n");
    for (i = 0; i < n; i++)
    {
	    printf("%d ", arr[i]);

    }
    return 0;
}





```

## `memory/program1.c`

```c
#include <stdio.h>

void rotate_array(int arr[], int size) 
{
    int left = 0, right = size - 1;

    while (left < right) 
    {
        while (arr[left] % 2 == 0 && left < right)
            left++;

        while (arr[right] % 2 == 1 && left < right)
            right--;

       if (left < right) 
	    {
            int temp = arr[left];
            arr[left] = arr[right];
            arr[right] = temp;
        }
    }
}

int main() 
{
    int n, i;

    printf("Enter the number of elements: ");
    scanf("%d", &n);
    int arr[n];
    printf("Enter %d elements:\n", n);
    for (i = 0; i < n; i++)
    {
        scanf("%d", &arr[i]);
    }
    rotate_array(arr, n);

    printf("Array after separating even and odd:\n");
    for (i = 0; i < n; i++)
    {
        printf("%d ", arr[i]);
    }
    return 0;
}


```

