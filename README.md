# Linux-IPC-Message-Queues
Linux IPC-Message Queues

# AIM:
To write a C program that receives a message from message queue and display them

# DESIGN STEPS:

### Step 1:

Navigate to any Linux environment installed on the system or installed inside a virtual environment like virtual box/vmware or online linux JSLinux (https://bellard.org/jslinux/vm.html?url=alpine-x86.cfg&mem=192) or docker.

### Step 2:

Write the C Program using Linux message queues API 

### Step 3:

Execute the C Program for the desired output. 

# PROGRAM:

## C program that receives a message from message queue and display them
#include <stdio.h> #include <stdlib.h> #include <sys/types.h> #include <sys/stat.h> #include <string.h> #include <fcntl.h> #include <unistd.h> #include <sys/wait.h>

void server(int rfd, int wfd); void client(int wfd, int rfd);

int main() { int p1[2], p2[2]; int pid, status;

pipe(p1);   // pipe1: client → server
pipe(p2);   // pipe2: server → client

pid = fork();

if (pid == 0)   // Child process → server
{
    close(p1[1]);
    close(p2[0]);
    server(p1[0], p2[1]);
    return 0;
}
else           // Parent process → client
{
    close(p1[0]);
    close(p2[1]);
    client(p1[1], p2[0]);
    wait(&status);
    return 0;
}
}

void client(int wfd, int rfd) { int n; char fname[2000]; char buff[2000];

printf("ENTER THE FILE NAME : ");
scanf("%s", fname);

printf("CLIENT SENDING THE REQUEST ...\nPLEASE WAIT\n");
sleep(10);

write(wfd, fname, 2000);

n = read(rfd, buff, 2000);
buff[n] = '\0';

printf("THE RESULTS OF CLIENTS ARE .....\n");
write(1, buff, n);
}

void server(int rfd, int wfd) { int n, fd; char fname[2000]; char buff[2000];

n = read(rfd, fname, 2000);
fname[n] = '\0';

fd = open(fname, O_RDONLY);
sleep(10);

if (fd < 0)
{
    write(wfd, "can't open", 9);
}
else
{
    n = read(fd, buff, 2000);
    write(wfd, buff, n);
}
}




## OUTPUT
<img width="606" height="503" alt="image" src="https://github.com/user-attachments/assets/921b5647-e0f2-42b7-9679-da4cb3aecd55" />
<img width="530" height="507" alt="image" src="https://github.com/user-attachments/assets/a3ab03d5-cf82-41ae-9f3d-064496a1e44d" />
<img width="542" height="502" alt="image" src="https://github.com/user-attachments/assets/233db9d4-e24e-4f42-88f5-26915965df2b" />





# RESULT:
The programs are executed successfully.
