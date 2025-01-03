set1

1.synchoronization problem

#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>
#include <semaphore.h>

#define BUFFER_SIZE 10

sem_t empty, full, mutex;
int buffer[BUFFER_SIZE];
int in = 0, out = 0;

void *producer(void *arg) {
    int item;
    while (1) {
        item = produce_item();
        sem_wait(&empty);
        sem_wait(&mutex);
        buffer[in] = item;
        in = (in + 1) % BUFFER_SIZE;
        sem_post(&mutex);
        sem_post(&full);
    }
}

void *consumer(void *arg) {
    int item;
    while (1) {
        sem_wait(&full);
        sem_wait(&mutex);
        item = buffer[out];
        out = (out + 1) % BUFFER_SIZE;
        sem_post(&mutex);
        sem_post(&empty);
        consume_item(item);
    }
}

int main() {
    pthread_t producer_thread, consumer_thread;
    sem_init(&empty, 0, BUFFER_SIZE);
    sem_init(&full, 0, 0);
    sem_init(&mutex, 0, 1);
    pthread_create(&producer_thread, NULL, producer, NULL);
    pthread_create(&consumer_thread, NULL, consumer, NULL);
    pthread_join(producer_thread, NULL);
    pthread_join(consumer_thread, NULL);
    return 0;
}
2. system call

#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main() {
    pid_t pid = fork();
    if (pid < 0) {
        printf("Error in forking\n");
        exit(1);
    } else if (pid == 0) {
        // child process
        sleep(5);
        printf("I am ready to murder my parent!\n");
        kill(getppid(), SIGINT);
        printf("I am an orphan now\n");
    } else {
        // parent process
        printf("I am the parent\n");
        wait(NULL);
    }
    return 0;
}
3. SCAN algoritham
#include <stdio.h>
#include <stdlib.h>

int main() {
    int RQ[100], i, j, n, TotalHeadMoment = 0, initial, size, move;
    printf("Enter the number of Requests\n");
    scanf("%d", &n);
    printf("Enter the Requests sequence\n");
    for (i = 0; i < n; i++)
        scanf("%d", &RQ[i]);
    printf("Enter initial head position\n");
    scanf("%d", &initial);
    printf("Enter total disk size\n");
    scanf("%d", &size);
    printf("Enter the head movement direction for high 1 and for low 0\n");
    scanf("%d", &move);
    for (i = 0; i < n; i++) {
        for (j = 0; j < n - i - 1; j++) {
            if (RQ[j] > RQ[j + 1]) {
                int temp;
                temp = RQ[j];
                RQ[j] = RQ[j + 1];
                RQ[j + 1] = temp;
            }
        }
    }
    int index;
    for (i = 0; i < n; i++) {
        if (initial < RQ[i]) {
            index = i;
            break;
        }
    }
    if (move == 1) {
        for (i = index; i < n; i++) {
            TotalHeadMoment = TotalHeadMoment + abs(RQ[i] - initial);
            initial = RQ[i];
        }
        TotalHeadMoment = TotalHeadMoment + abs(size - RQ[n - 1] - 1);
        TotalHeadMoment = TotalHeadMoment + abs(size - 1 - 0);
        initial = 0;
        for (i = 0; i < index; i++) {
            TotalHeadMoment = TotalHeadMoment + abs(RQ[i] - initial);
            initial = RQ[i];
        }
    } else {
        for (i = index - 1; i >= 0; i--) {
            TotalHeadMoment = TotalHeadMoment + abs(RQ[i] - initial);
            initial = RQ[i];
        }
        TotalHeadMoment = TotalHeadMoment + abs(RQ[0] - 0);
        TotalHeadMoment = TotalHeadMoment + abs(size - 1 - RQ[index - 1]);
        initial = size - 1;
        for (i = n - 1; i >= index; i--) {
            TotalHeadMoment = TotalHeadMoment + abs(RQ[i] - initial);
            initial = RQ[i];
        }
    }
    printf("Total head movement is %d", TotalHeadMoment);
    return 0;
}
4. index file allocation
#include <stdio.h>
#include <stdlib.h>

int main() {
    int RQ[100], i, j, n, TotalHeadMoment = 0, initial, size, move;
    printf("Enter the number of Requests\n");
    scanf("%d", &n);
    printf("Enter the Requests sequence\n");
    for (i = 0; i < n; i++)
        scanf("%d", &RQ[i]);
    printf("Enter initial head position\n");
    scanf("%d", &initial);
    printf("Enter total disk size\n");
    scanf("%d", &size);
    printf("Enter the head movement direction for high 1 and for low 0\n");
    scanf("%d", &move);

    // Create index block
    int index_block[100];
    for (i = 0; i < 100; i++) {
        index_block[i] = -1;
    }

    // Add requests to the index block
    for (i = 0; i < n; i++) {
        int block = RQ[i] - 1; // Convert request number to block number
        if (index_block[block] == -1) {
            index_block[block] = i;
        } else {
            printf("Error: Block %d already in use\n", block);
            exit(1);
        }
    }

    // Sort the request sequence in ascending order
    for (i = 0; i < n - 1; i++) {
        for (j = 0; j < n - i - 1; j++) {
            if (RQ[j] > RQ[j + 1]) {
                int temp;
                temp = RQ[j];
                RQ[j] = RQ[j + 1];
                RQ[j + 1] = temp;
            }
        }
    }

    // Calculate total head movement
    if (move == 1) {
        for (i = 0; i < n; i++) {
            if (RQ[i] > initial) {
                TotalHeadMoment = TotalHeadMoment + abs(RQ[i] - initial);
                initial = RQ[i];
            }
        }
        TotalHeadMoment = TotalHeadMoment + abs(size - RQ[n - 1] - 1);
        TotalHeadMoment = TotalHeadMoment + abs(size - 1 - RQ[0]);
        initial = 0;
        for (i = 0; i < n; i++) {
            if (RQ[i] < initial) {
                TotalHeadMoment = TotalHeadMoment + abs(RQ[i] - initial);
                initial = RQ[i];
            }
        }
    } else {
        for (i = n - 1; i >= 0; i--) {
            if (RQ[i] > initial) {
                TotalHeadMoment = TotalHeadMoment + abs(RQ[i] - initial);
                initial = RQ[i];
            }
        }
        TotalHeadMoment = TotalHeadMoment + abs(RQ[0] - 0);
        TotalHeadMoment = TotalHeadMoment + abs(size - 1 - RQ[n - 1]);
        initial = size - 1;
        for (i = n - 1; i >= 0; i--) {
            if (RQ[i] < initial) {
                TotalHeadMoment = TotalHeadMoment + abs(RQ[i] - initial);
                initial = RQ[i];
            }
        }
    }

    printf("Total head movement is %d", TotalHeadMoment);
    return 0;
}
                                           SET 3
1. cpu shecluding algoritham
#include <stdio.h>

// Structure to store process details
struct Process {
    int pid; // Process ID
    int burst_time; // Burst time
    int arrival_time; // Arrival time
    int waiting_time; // Waiting time
    int turnaround_time; // Turnaround time
};

int main() {
    int n, i, j;
    float avg_waiting_time, avg_turnaround_time;
    printf("Enter the number of processes: ");
    scanf("%d", &n);

    // Array of processes
    struct Process processes[n];

    // Input process details
    for (i = 0; i < n; i++) {
        processes[i].pid = i + 1;
        printf("Enter the arrival time of process %d: ", processes[i].pid);
        scanf("%d", &processes[i].arrival_time);
        printf("Enter the burst time of process %d: ", processes[i].pid);
        scanf("%d", &processes[i].burst_time);
    }

    // Sort processes based on arrival time
    for (i = 0; i < n - 1; i++) {
        for (j = 0; j < n - i - 1; j++) {
            if (processes[j].arrival_time > processes[j + 1].arrival_time) {
                struct Process temp = processes[j];
                processes[j] = processes[j + 1];
                processes[j + 1] = temp;
            }
        }
    }

    // Calculate waiting time and turnaround time
    processes[0].waiting_time = 0;
    processes[0].turnaround_time = processes[0].burst_time;
    for (i = 1; i < n; i++) {
        processes[i].waiting_time = processes[i - 1].waiting_time + processes[i - 1].burst_time;
        processes[i].turnaround_time = processes[i].waiting_time + processes[i].burst_time;
    }

    // Calculate average waiting time and average turnaround time
    avg_waiting_time = 0;
    avg_turnaround_time = 0;
    for (i = 0; i < n; i++) {
        avg_waiting_time += processes[i].waiting_time;
        avg_turnaround_time += processes[i].turnaround_time;
    }
    avg_waiting_time /= n;
    avg_turnaround_time /= n;

    // Output results
    printf("Process\tArrival Time\tBurst Time\tWaiting Time\tTurnaround Time\n");
    for (i = 0; i < n; i++) {
        printf("%d\t%d\t\t%d\t\t%d\t\t%d\n", processes[i].pid, processes[i].arrival_time, processes[i].burst_time, processes[i].waiting_time, processes[i].turnaround_time);
    }
    printf("Average Waiting Time: %.2f\n", avg_waiting_time);
    printf("Average Turnaround Time: %.2f\n", avg_turnaround_time);

    return 0;
}

2.file and close file
#include <fcntl.h>
#include <stdio.h>
#include <unistd.h>

int main() {
    // Create a new file
    int fd = open("example.txt", O_CREAT | O_RDWR, S_IRUSR | S_IWUSR);
    if (fd < 0) {
        perror("Error creating and opening the file");
        return 1;
    }

    // Read data from the user
    char buffer[100];
    int n = read(0, buffer, sizeof(buffer));
    if (n < 0) {
        perror("Error reading from the user");
        close(fd);
        return 1;
    }

    // Write the same contents in the file
    int written = write(fd, buffer, n);
    if (written != n) {
        perror("Error writing to the file");
        close(fd);
        return 1;
    }

    // Close the file
    close(fd);

    return 0;
}
3.disk sheculding algoritham FCFS
#include <stdio.h>
#include <stdlib.h>

int main() {
    int n, i, j, k, seek = 0, max, diff;
    float avg;

    // Read the number of requests
    scanf("%d", &n);

    // Read the sequence of requests
    int requests[n];
    for (i = 0; i < n; i++) {
        scanf("%d", &requests[i]);
    }

    // Read the initial head position
    scanf("%d", &initial);

    // Calculate the total head movement for each request
    for (i = 0; i < n; i++) {
        diff = abs(requests[i] - initial);
        total_head_movement += diff;
    }

    // Update the head position after servicing each request
    for (i = 0; i < n; i++) {
        initial = requests[i];
    }

    // Calculate the total head movement after servicing all requests
    total_head_movement = total_head_movement;

    // Calculate the average seek time
    avg = total_head_movement / (float)n;

    // Print the results
    printf("Total head movement: %d\n", total_head_movement);
    printf("Average seek time: %f\n", avg);

    return 0;
}
4.first fit
#include <stdio.h>

void firstFit(int blockSize[], int m, int processSize[], int n) {
    int allocation[n];
    for (int i = 0; i < n; i++) {
        allocation[i] = -1;
    }

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (blockSize[j] >= processSize[i]) {
                allocation[i] = j;
                blockSize[j] -= processSize[i];
                break;
            }
        }
    }

    printf("\nProcess No.\tProcess Size\tBlock no.\n");
    for (int i = 0; i < n; i++) {
        printf("%d \t\t\t %d \t\t\t", i+1, processSize[i]);
        if (allocation[i] != -1) {
            printf("%d\n", allocation[i] + 1);
        } else {
            printf("Not Allocated\n");
        }
    }
}

int main() {
    int blockSize[] = {100, 500, 200, 300, 600};
    int processSize[] = {212, 417, 112, 426};
    int m = sizeof(blockSize) / sizeof(blockSize[0]);
    int n = sizeof(processSize) / sizeof(processSize[0]);

    firstFit(blockSize, m, processSize, n);

    return 0;
}
                                     SET 2
1.cpu sheduling alogritham

  ans:set 3 lo second first que

2.parent process odd and even
#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/wait.h>
#include <unistd.h>

int main() {
    pid_t pid = fork();

    if (pid < 0) {  // fork failed
        fprintf(stderr, "Fork failed\n");
        return 1;
    } else if (pid == 0) {  // child process
        for (int i = 2; i <= 10; i += 2) {
            printf("Child: %d\n", i);
        }
    } else {  // parent process
        for (int i = 1; i <= 10; i += 2) {
            printf("Parent: %d\n", i);
        }
        wait(NULL);  // wait for the child process to finish
    }

    return 0;
}
3.C SCAN 
#include <stdio.h>
#include <stdlib.h>

void cscan(int arr[], int head, int size, int disk_size) {
    int seek_count = 0;
    int distance, cur_track;
    int right[size], left[size];
    int r = 0, l = 0;
    int temp;

    // Appending end values which has to be visited before reversing the direction
    right[r++] = disk_size - 1;
    left[l++] = 0;

    for (int i = 0; i < size; i++) {
        if (arr[i] < head) {
            left[l++] = arr[i];
        }
        if (arr[i] > head) {
            right[r++] = arr[i];
        }
    }

    // Sorting the requests
    for (int i = 0; i < r - 1; i++) {
        for (int j = i + 1; j < r; j++) {
            if (right[i] > right[j]) {
                temp = right[i];
                right[i] = right[j];
                right[j] = temp;
            }
        }
    }

    for (int i = 0; i < l - 1; i++) {
        for (int j = i + 1; j < l; j++) {
            if (left[i] > left[j]) {
                temp = left[i];
                left[i] = left[j];
                left[j] = temp;
            }
        }
    }

    for (int i = 0; i < r; i++) {
        cur_track = right[i];
        seek_count += abs(cur_track - head);
        head = cur_track;
    }

    head = 0;
    seek_count += (disk_size - 1);

    for (int i = 0; i < l; i++) {
        cur_track = left[i];
        seek_count += abs(cur_track - head);
        head = cur_track;
    }

    printf("Total number of track movements in cylinders: %d\n", seek_count);
}

int main() {
    int arr[] = {82, 170, 43, 140, 24, 16, 190};
    int head = 50;
    int size = sizeof(arr) / sizeof(arr[0]);
    int disk_size = 200;

    cscan(arr, head, size, disk_size);

    return 0;
}
4. MFA marster file dirctionary
#include <stdio.h>
#include <string.h>

// Structure for User File Directory (UFD)
struct UFD {
    char username[20];
    char files[10][20];
    int file_count;
};

// Structure for Master File Directory (MFD)
struct MFD {
    struct UFD ufd[10];
    int user_count;
};

int main() {
    // Initialize the Master File Directory (MFD)
    struct MFD mfd;
    mfd.user_count = 0;

    // Add users to the MFD
    strcpy(mfd.ufd[0].username, "user1");
    mfd.ufd[0].file_count = 2;
    strcpy(mfd.ufd[0].files[0], "file1.txt");
    strcpy(mfd.ufd[0].files[1], "file2.txt");
    mfd.user_count++;

    strcpy(mfd.ufd[1].username, "user2");
    mfd.ufd[1].file_count = 1;
    strcpy(mfd.ufd[1].files[0], "document.txt");
    mfd.user_count++;

    // Simulate user job start or login
    char current_user[] = "user1";

    // Search the MFD for the current user's UFD
    int user_index = -1;
    for (int i = 0; i < mfd.user_count; i++) {
        if (strcmp(mfd.ufd[i].username, current_user) == 0) {
            user_index = i;
            break;
        }
    }

    if (user_index != -1) {
        // User's UFD found, search for a file in the UFD
        printf("User's UFD found. Files in the UFD:\n");
        for (int i = 0; i < mfd.ufd[user_index].file_count; i++) {
            printf("%s\n", mfd.ufd[user_index].files[i]);
        }
    } else {
        // User's UFD not found
        printf("User's UFD not found\n");
    }

    return 0;
}
                                                 SET 4
1.smaphore synachornzation
#include <stdio.h>
#include <pthread.h>
#include <semaphore.h>
#include <unistd.h>

int shared = 0; // Shared variable
sem_t s; // Semaphore variable

// Function for the first thread
void *fun1() {
    int x;
    sem_wait(&s); // Executes wait operation on s
    x = shared; // Thread 1 reads the value of the shared variable
    printf("Thread 1 reads the value as %d\n", x);
    x++; // Thread 1 increments its value
    printf("Local updation by Thread 1: %d\n", x);
    sleep(1); // Thread 1 is preempted by Thread 2
    shared = x; // Thread 1 updates the value of the shared variable
    printf("Value of shared variable updated by Thread 1 is: %d\n", shared);
    sem_post(&s);
}

// Function for the second thread
void *fun2() {
    int y;
    sem_wait(&s);
    y = shared; // Thread 2 reads the value of the shared variable
    printf("Thread 2 reads the value as %d\n", y);
    y--; // Thread 2 decrements its value
    printf("Local updation by Thread 2: %d\n", y);
    sleep(1); // Thread 2 is preempted by Thread 1
    shared = y; // Thread 2 updates the value of the shared variable
    printf("Value of shared variable updated by Thread 2 is: %d\n", shared);
    sem_post(&s);
}

int main() {
    pthread_t t1, t2;
    sem_init(&s, 0, 1); // Initialize semaphore variable
    pthread_create(&t1, NULL, fun1, NULL); // Create thread 1
    pthread_create(&t2, NULL, fun2, NULL); // Create thread 2
    pthread_join(t1, NULL); // Join thread 1
    pthread_join(t2, NULL); // Join thread 2
    sem_destroy(&s); // Destroy semaphore variable
    return 0;
}
2.cpu shedulimg algortiam
ans:set 3 first que

3.SCAN
ans:set 1. 3 questione
4.synachornzation 
ans:set 1 first que
                                      SET 5
1.disk shecduling algorithm
ans:set 3 third que
2. multi tread one even one odd
#include <stdio.h>
#include <pthread.h>
#include <semaphore.h>

int shared = 0; // Shared variable
sem_t s; // Semaphore variable

// Function for the first thread to print even numbers
void *print_even() {
    while (shared < 10) {
        sem_wait(&s); // Executes wait operation on s
        if (shared % 2 == 0) {
            printf("Thread 1 prints even number: %d\n", shared);
            shared++; // Thread 1 increments the value of shared variable
        }
        sem_post(&s);
    }
}

// Function for the second thread to print odd numbers
void *print_odd() {
    while (shared < 10) {
        sem_wait(&s);
        if (shared % 2 != 0) {
            printf("Thread 2 prints odd number: %d\n", shared);
            shared++; // Thread 2 increments the value of shared variable
        }
        sem_post(&s);
    }
}

int main() {
    pthread_t t1, t2;
    sem_init(&s, 0, 1); // Initialize semaphore variable
    pthread_create(&t1, NULL, print_even, NULL); // Create thread 1
    pthread_create(&t2, NULL, print_odd, NULL); // Create thread 2
    pthread_join(t1, NULL); // Join thread 1
    pthread_join(t2, NULL); // Join thread 2
    sem_destroy(&s); // Destroy semaphore variable
    return 0;
}
3.When a new process enters a system, it must declare the maximum number of instances of each resource type it needs. This number may exceed the total number of resources in the system. When the user requests a set of resources, the system must determine whether the allocation of each resource will leave the system in safe state. If true, the resources are allocated; otherwise the process must wait until some other process releases the resources. Write a C program to demonstrate the above scenario. 
 ans: #include <stdio.h>
#include <stdlib.h>

// Structure to represent a process
struct process {
    int id;
    int max[10];
    int allocation[10];
    int need[10];
};

// Function to initialize the process array
void init_process(struct process *processes, int num_processes) {
    for (int i = 0; i < num_processes; i++) {
        process[i].id = i;
        for (int j = 0; j < 10; j++) {
            process[i].max[j] = rand() % 50;
            process[i].allocation[j] = 0;
            process[i].need[j] = process[i].max[j];
        }
    }
}

// Function to check if the system is in a safe state
bool safe_state(struct process *processes, int num_processes, int available[]) {
    for (int i = 0; i < num_processes; i++) {
        for (int j = 0; j < 10; j++) {
            if (processes[i].need[j] > available[j]) {
                return false;
            }
        }
    }
    return true;
}

// Function to perform the Banker's Algorithm
void bankers_algorithm(struct process *processes, int num_processes, int available[]) {
    int safe_sequence[num_processes];
    int count = 0;

    while (count < num_processes) {
        bool flag = false;
        for (int i = 0; i < num_processes; i++) {
            if (processes[i].need[0] > 0 && available[0] >= processes[i].need[0]) {
                available[0] -= processes[i].need[0];
                processes[i].allocation[0] += processes[i].need[0];
                processes[i].need[0] = 0;

                flag = true;
                count++;
            }
        }

        if (!flag) {
            count--;
            for (int i = num_processes - 1; i >= 0; i--) {
                available[i] += processes[i].allocation[i];
                processes[i].allocation[i] = 0;
                processes[i].need[i] = processes[i].max[i] - processes[i].allocation[i];
            }
        }
    }

    for (int i = 0; i < num_processes; i++) {
        if (processes[i].need[0] > 0) {
            return false;
        }
    }

    return true;
}

int main() {
    int num_processes = 5;
    int available[10] = {10, 10, 10, 10, 10};
    struct process processes[num_processes];

    init_process(processes, num_processes);

    if (safe_state(processes, num_processes, available)) {
        printf("System is in a safe state\n");
    } else {
        printf("System is not in a safe state\n");
    }

    for (int i = 0; i < num_processes; i++) {
        printf("Process %d:\n", processes[i].id);
        for (int j = 0; j < 10; j++) {
            printf("%d ", processes[i].allocation[j]);
        }
        printf("\n");
    }

    return 0;
}
4. index file algoritham
ans: set1 fourth que
                                     SET 6
1. CPUsheduling 
ans: set 3. first que
2.FIFO
#include <stdio.h>

#define MAX_FRAMES 3
#define MAX_PAGES 20

int main() {
    int frames[MAX_FRAMES];
    int pages[MAX_PAGES];
    int page_faults = 0;
    int next_frame = 0;

    // Initialize frames to -1 (indicating empty)
    for (int i = 0; i < MAX_FRAMES; i++) {
        frames[i] = -1;
    }

    // Input the page references
    printf("Enter the page references (enter -1 to end):\n");
    int page;
    int num_pages = 0;
    while (1) {
        scanf("%d", &page);
        if (page == -1) {
            break;
        }
        pages[num_pages] = page;
        num_pages++;
    }

    // Simulate the FIFO page replacement algorithm
    for (int i = 0; i < num_pages; i++) {
        int page = pages[i];
        int found = 0;
        for (int j = 0; j < MAX_FRAMES; j++) {
            if (frames[j] == page) {
                found = 1;
                break;
            }
        }
        if (!found) {
            frames[next_frame] = page;
            next_frame = (next_frame + 1) % MAX_FRAMES;
            page_faults++;
        }
    }

    // Output the page faults
    printf("Page faults: %d\n", page_faults);

    return 0;
}
3.single level dirctionary
#include <stdio.h>
#include <string.h>

struct file {
    char name[20];
    char content[100];
};

struct directory {
    struct file files[50];
    int file_count;
};

int main() {
    struct directory root;
    root.file_count = 0;

    int choice;
    do {
        printf("\n1. Create a file\n");
        printf("2. Display all files\n");
        printf("3. Search for a file\n");
        printf("4. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1: {
                if (root.file_count < 50) {
                    printf("Enter the name of the file: ");
                    scanf("%s", root.files[root.file_count].name);
                    printf("Enter the content of the file: ");
                    scanf("%s", root.files[root.file_count].content);
                    root.file_count++;
                } else {
                    printf("Directory is full\n");
                }
                break;
            }
            case 2: {
                printf("Files in the directory:\n");
                for (int i = 0; i < root.file_count; i++) {
                    printf("%s\n", root.files[i].name);
                }
                break;
            }
            case 3: {
                char search_name[20];
                printf("Enter the name of the file to search for: ");
                scanf("%s", search_name);
                int found = 0;
                for (int i = 0; i < root.file_count; i++) {
                    if (strcmp(root.files[i].name, search_name) == 0) {
                        printf("File found. Content: %s\n", root.files[i].content);
                        found = 1;
                        break;
                    }
                }
                if (!found) {
                    printf("File not found\n");
                }
                break;
            }
            case 4: {
                printf("Exiting...\n");
                break;
            }
            default: {
                printf("Invalid choice\n");
            }
        }
    } while (choice != 4);

    return 0;
}
4.n natural number incresing orer with two tread
#include <stdio.h>
#include <pthread.h>

#define N 10

void *print_even(void *arg) {
    for (int i = 2; i <= N; i += 2) {
        printf("%d\n", i);
    }
    pthread_exit(NULL);
}

void *print_odd(void *arg) {
    for (int i = 1; i <= N; i += 2) {
        printf("%d\n", i);
    }
    pthread_exit(NULL);
}

int main() {
    pthread_t t1, t2;
    pthread_create(&t1, NULL, print_even, NULL);
    pthread_create(&t2, NULL, print_odd, NULL);
    pthread_join(t1, NULL);
    pthread_join(t2, NULL);
    return 0;
}
                                     SET 7
1.recently used replacment
#include <stdio.h>
#include <stdlib.h>

#define PAGE_SIZE 4096
#define N 10

int page_map[N];
int page_reference[N];
int page_count = 0;

void simulate_page_references() {
    for (int i = 0; i < N; i++) {
        page_reference[i] = i;
    }
    for (int i = 0; i < N; i++) {
        int page = page_reference[i];
        int index = page % N;
        if (page_map[index] == -1) {
            page_map[index] = page;
            page_count++;
        } else {
            page_map[index] = -1;
            page_count--;
        }
    }
}

void lru_page_replacement() {
    for (int i = 0; i < N; i++) {
        if (page_map[i] != -1) {
            printf("Page %d\n", page_map[i]);
        }
    }
    printf("Page faults: %d\n", page_count);
}

int main() {
    simulate_page_references();
    lru_page_replacement();
    return 0;
}
2.synachornzation 
ans: set1. first que
3.To Write a C program to simulate the Best -fit contiguous memory allocation techniques. One of the simplest methods for memory allocation is to divide memory into several fixed-sized partitions. Each partition may contain exactly one process. In this multiple-partition method, when a partition is free, a process is selected from the input queue and is loaded into the free partition. When the process terminates, the partition becomes available for another process. The operating system keeps a table indicating which parts of memory are available and which are occupied. Finally, when a process arrives and needs memory, a memory section large enough for this process is provided. When it is time to load or swap a process into main memory, and if there is more than one free block of memory of sufficient size, then the operating system must decide which free block to allocate. Best-fit strategy chooses the block that is closest in size to the request.
ans:#include <stdio.h>

#define MAX_BLOCKS 10
#define MAX_PROCESSES 10

void best_fit(int block_size[], int num_blocks, int process_size[], int num_processes) {
    int allocation[MAX_PROCESSES];
    for (int i = 0; i < num_processes; i++) {
        allocation[i] = -1;
    }
    for (int i = 0; i < num_processes; i++) {
        int best_index = -1;
        for (int j = 0; j < num_blocks; j++) {
            if (block_size[j] >= process_size[i]) {
                if (best_index == -1 || block_size[j] < block_size[best_index]) {
                    best_index = j;
                }
            }
        }
        if (best_index != -1) {
            allocation[i] = best_index;
            block_size[best_index] -= process_size[i];
        }
    }
    printf("Process No.\tProcess Size\tBlock no.\n");
    for (int i = 0; i < num_processes; i++) {
        printf("%d \t\t\t %d \t\t\t", i+1, process_size[i]);
        if (allocation[i] != -1) {
            printf("%d\n", allocation[i] + 1);
        } else {
            printf("Not Allocated\n");
        }
    }
}

int main() {
    int block_size[MAX_BLOCKS];
    int process_size[MAX_PROCESSES];
    int num_blocks, num_processes;

    printf("Enter the number of memory blocks: ");
    scanf("%d", &num_blocks);
    printf("Enter the size of each memory block:\n");
    for (int i = 0; i < num_blocks; i++) {
        scanf("%d", &block_size[i]);
    }

    printf("Enter the number of processes: ");
    scanf("%d", &num_processes);
    printf("Enter the size of each process:\n");
    for (int i = 0; i < num_processes; i++) {
        scanf("%d", &process_size[i]);
    }

    best_fit(block_size, num_blocks, process_size, num_processes);

    return 0;
}
4. light weight process
set 4.first que
                                           SET 8
1. set 6 .fourth que
3.set 7.fist que
4.cpu sheduling 
ans: set 2.first que
2. declareation of maximum number
#include <stdio.h>

#define MAX_BLOCKS 10
#define MAX_PROCESSES 10

void best_fit(int block_size[], int num_blocks, int process_size[], int num_processes) {
    int allocation[MAX_PROCESSES];
    for (int i = 0; i < num_processes; i++) {
        allocation[i] = -1;
    }
    for (int i = 0; i < num_processes; i++) {
        int best_index = -1;
        for (int j = 0; j < num_blocks; j++) {
            if (block_size[j] >= process_size[i]) {
                if (best_index == -1 || block_size[j] < block_size[best_index]) {
                    best_index = j;
                }
            }
        }
        if (best_index != -1) {
            allocation[i] = best_index;
            block_size[best_index] -= process_size[i];
        }
    }
    printf("Process No.\tProcess Size\tBlock no.\n");
    for (int i = 0; i < num_processes; i++) {
        printf("%d \t\t\t %d \t\t\t", i+1, process_size[i]);
        if (allocation[i] != -1) {
            printf("%d\n", allocation[i] + 1);
        } else {
            printf("Not Allocated\n");
        }
    }
}

int main() {
    int block_size[MAX_BLOCKS];
    int process_size[MAX_PROCESSES];
    int num_blocks, num_processes;

    printf("Enter the number of memory blocks: ");
    scanf("%d", &num_blocks);
    printf("Enter the size of each memory block:\n");
    for (int i = 0; i < num_blocks; i++) {
        scanf("%d", &block_size[i]);
    }

    printf("Enter the number of processes: ");
    scanf("%d", &num_processes);
    printf("Enter the size of each process:\n");
    for (int i = 0; i < num_processes; i++) {
        scanf("%d", &process_size[i]);
    }

    best_fit(block_size, num_blocks, process_size, num_processes);

    return 0;
}
                                   SET 9
2
 set 7.lo first answer
1,3. same que 
     dead lock
ans: #include <stdio.h>
#include <pthread.h>
#include <unistd.h>

pthread_mutex_t first_mutex;  // Mutex lock for the first resource
pthread_mutex_t second_mutex;  // Mutex lock for the second resource

// Function for the first thread
void *function1() {
    pthread_mutex_lock(&first_mutex);  // Acquire the first resource
    printf("Thread ONE acquired first_mutex\n");
    sleep(1);
    pthread_mutex_lock(&second_mutex);  // Try to acquire the second resource
    printf("Thread ONE acquired second_mutex\n");
    pthread_mutex_unlock(&second_mutex);  // Release the second resource
    pthread_mutex_unlock(&first_mutex);  // Release the first resource
    return NULL;
}

// Function for the second thread
void *function2() {
    pthread_mutex_lock(&second_mutex);  // Acquire the second resource
    printf("Thread TWO acquired second_mutex\n");
    sleep(1);
    pthread_mutex_lock(&first_mutex);  // Try to acquire the first resource
    printf("Thread TWO acquired first_mutex\n");
    pthread_mutex_unlock(&first_mutex);  // Release the first resource
    pthread_mutex_unlock(&second_mutex);  // Release the second resource
    return NULL;
}

int main() {
    pthread_mutex_init(&first_mutex, NULL);  // Initialize the first mutex
    pthread_mutex_init(&second_mutex, NULL);  // Initialize the second mutex

    pthread_t one, two;
    pthread_create(&one, NULL, function1, NULL);  // Create the first thread
    pthread_create(&two, NULL, function2, NULL);  // Create the second thread

    pthread_join(one, NULL);
    pthread_join(two, NULL);

    pthread_mutex_destroy(&first_mutex);  // Destroy the first mutex
    pthread_mutex_destroy(&second_mutex);  // Destroy the second mutex

    return 0;
}
4. close file
ans: set 3. lo second que
                                     SET 10

1.wrost fit contiguous
#include <stdio.h>

void worstFit(int blockSize[], int m, int processSize[], int n) {
    int allocation[n];
    for (int i = 0; i < n; i++) {
        allocation[i] = -1;
    }
    for (int i = 0; i < n; i++) {
        int bestIdx = -1;
        int diff;
        for (int j = 0; j < m; j++) {
            if (blockSize[j] >= processSize[i]) {
                if (bestIdx == -1) {
                    diff = blockSize[j] - processSize[i];
                    bestIdx = j;
                } else {
                    int inter = blockSize[j] - processSize[i];
                    if (diff < inter) {
                        diff = inter;
                        bestIdx = j;
                    }
                }
            }
        }
        if (bestIdx != -1) {
            blockSize[bestIdx] -= processSize[i];
            allocation[i] = bestIdx;
        }
    }
    printf("Process No.\tProcess Size\tBlock no.\n");
    for (int i = 0; i < n; i++) {
        printf("%d \t\t\t %d \t\t\t", i+1, processSize[i]);
        if (allocation[i] != -1) {
            printf("%d\n", allocation[i] + 1);
        } else {
            printf("Not Allocated\n");
        }
    }
}

int main() {
    int blockSize[] = {100, 500, 200, 300, 600};
    int processSize[] = {212, 417, 112, 426};
    int m = sizeof(blockSize) / sizeof(blockSize[0]);
    int n = sizeof(processSize) / sizeof(processSize[0]);
    worstFit(blockSize, m, processSize, n);
    return 0;
}
2..The most common form of file structure is the sequential file in this type of file, a fixed format is used for records. All records (of the system) have the same length, consisting of the same number of fixed length fields in a particular order because the length and position of each field are known, only the values of fields need to be stored, the field name and length for each field are attributes of the file structure. Develop a code for sequential file allocation, where the file allocation table consists of a single entry for each file. It shows the filenames, starting block of the file and size of the file. 
#include <stdio.h>

struct File {
    char fileName[20];
    int startBlock;
    int fileSize;
};

int main() {
    struct File fileTable[50];
    int numFiles, i;

    printf("Enter the number of files: ");
    scanf("%d", &numFiles);

    for (i = 0; i < numFiles; i++) {
        printf("Enter the name of file #%d: ", i + 1);
        scanf("%s", fileTable[i].fileName);
        printf("Enter the starting block of file #%d: ", i + 1);
        scanf("%d", &fileTable[i].startBlock);
        printf("Enter the size of file #%d: ", i + 1);
        scanf("%d", &fileTable[i].fileSize);
    }

    printf("File Allocation Table\n");
    printf("-------------------------------------------------\n");
    printf("FileName\tStartBlock\tFileSize\n");
    for (i = 0; i < numFiles; i++) {
        printf("%s\t\t%d\t\t%d\n", fileTable[i].fileName, fileTable[i].startBlock, fileTable[i].fileSize);
    }
    return 0;
}
3.replacement
#include <stdio.h>
#include <stdlib.h>

int pageFault(int a[], int frame[], int n, int no) {
    int i, j, avail, count = 0, k;
    for (i = 0; i < no; i++) {
        frame[i] = -1;
    }
    j = 0;
    for (i = 0; i < n; i++) {
        avail = 0;
        for (k = 0; k < no; k++)
            if (frame[k] == a[i])
                avail = 1;
        if (avail == 0) {
            frame[j] = a[i];
            j = (j + 1) % no;
            count++;
        }
    }
    return count;
}

int main() {
    int n, i, *a, *frame, no, fault;
    printf("\nENTER THE NUMBER OF PAGES:\n");
    scanf("%d", &n);
    a = (int *)malloc(n * sizeof(int));
    printf("ENTER THE PAGE NUMBER :\n");
    for (i = 0; i < n; i++)
        scanf("%d", &a[i]);
    printf("ENTER THE NUMBER OF FRAMES :");
    scanf("%d", &no);
    frame = (int *)malloc(no * sizeof(int));
    fault = pageFault(a, frame, n, no);
    printf("Page Fault Is %d", fault);
    return 0;
}
4. set6. 1 que
