e

![lab](/resources/pclogo-2.png)
## <div align="center">Lab 02</div>

1. Gain hands-on experience accessing and managing an AWS EC2 Linux instance using SSH.
2. Learn how to transfer files and directories securely using SCP, SFTP, and rsync.
3. Practice running remote commands and basic Linux operations on a cloud instance.
4. Develop and test simple C programs, including parallel programs using OpenMP and automated builds using make.

## Instructions

1. Get Screen shots of you carrying out these activities and share a word documents for each of the Parts seperately
2. Submit code samples if relavent


### Part 1: Connecting to AWS EC2
1. Prerequisites
An AWS account with an EC2 instance running.
A .pem key file (e.g., my-key.pem) downloaded when launching the instance.
Security Group configured to allow SSH (port 22) from your IP.
A terminal (Linux/Mac) or Git Bash (Windows).

2. Connecting via SSH
Set Key Permissions
`chmod 400 my-key.pem`
Connect to EC2
`ssh -i my-key.pem ec2-user@`
For Ubuntu AMIs, replace ec2-user with ubuntu.
Example:
`ssh -i my-key.pem ubuntu@3.122.45.67`

### Part 2: File Transfers
1. Copy Files to EC2 with SCP
`scp -i my-key.pem file.txt ec2-user@:~/`
2. Copy Directory to EC2
`scp -i my-key.pem -r myproject/ ec2-user@:~/myproject/`
3. Copy Files from EC2 to Local
`scp -i my-key.pem ec2-user@:~/output.txt ./downloaded\_output.txt`

### Part 3: Using SFTP for File Management
1. Start SFTP Session
`sftp -o IdentityFile=my-key.pem ec2-user@`
2. Inside SFTP Prompt
```
ls # List remote files
put file # Upload local file
get file # Download remote file
mkdir data # Create remote directory
exit # Quit SFTP
```
### Part 4: Checking Remote Directories without SSHing
1. Run a Remote Command via SSH
`ssh -i my-key.pem ec2-user@ "ls -lh /home/ec2-user/"`
2. Non-Interactive SFTP Command
`sftp -o IdentityFile=my-key.pem ec2-user@ <<< $'ls /home/ec2-user/'`

### Part 5: Automating Transfers
Use rsync for efficient synchronization:
`rsync -avz -e "ssh -i my-key.pem" ./localfolder/ ec2-user@:~/remotefolder/`

### Part 6: OpenMP Programs and timing parallel programs

OpenMP Program
This exercise demonstrates how to use OpenMP to parallelize a simple array computation using a for-loop.
✅ The program below:
Initializes two arrays A and B.
Computes C[i] = A[i] + B[i] using a parallel for-loop.
Uses both environment variable and code-based thread control.
Measures execution time using OpenMP’s omp\_get\_wtime().

🧾 parallel\_add.c
```
#include 
#include 
#include 

#define SIZE 10000

int main() {
 double *A = (double *)malloc(SIZE * sizeof(double));
 double *B = (double *)malloc(SIZE * sizeof(double));
 double *C = (double *)malloc(SIZE * sizeof(double));

 // Initialize arrays
 for (int i = 0; i < SIZE; i++) {
 A[i] = i * 0.5;
 B[i] = i * 2.0;
 }

 // Set number of threads explicitly
 int num_threads = 4;
 omp_set_num_threads(num_threads);
 printf("Running with %d threads.\n", num_threads);

 double start_time = omp_get_wtime();

 #pragma omp parallel for
 for (int i = 0; i < SIZE; i++) {
 C[i] = A[i] + B[i];
 }

 double end_time = omp_get_wtime();

 printf("Completed computation. Sample result: C[100] = %f\n", C[100]);
 printf("Time taken: %f seconds\n", end_time - start_time);

 free(A);
 free(B);
 free(C);

 return 0;
}
```
Compile and Run
`gcc -fopenmp parallel\_add.c -o parallel\_add`
`./parallel\_add`
Output might look like:
```
Running with 4 threads.
Completed computation. Sample result: C[100] = 250.000000
Time taken: 0.123456 seconds
```

### Part 7: Using make in Linux
The make utility is used to automate building projects, especially when compiling code (e.g., C/C++). It uses a Makefileto define rules.

1. Installing make
On Amazon Linux:
`sudo yum install make -y`
On Ubuntu:
`sudo apt install make -y`

2. A Simple Example
Suppose you have a main.c and utils.c. You can compile them manually:
`gcc main.c utils.c -o myapp`
But with a Makefile, you can automate:
Create a Makefile
```
# Makefile
CC = gcc
CFLAGS = -Wall

myapp: main.o utils.o
 $(CC) $(CFLAGS) -o myapp main.o utils.o

main.o: main.c
 $(CC) $(CFLAGS) -c main.c

utils.o: utils.c
 $(CC) $(CFLAGS) -c utils.c

clean:
 rm -f *.o myapp
```
3. Run make
`make`
This will compile all files and create myapp.

4. Clean Up
`make clean`
Removes object files and the executable.

5. Make with Parallel Jobs
Speed up compilation with:
`make -j4`
(4 = number of jobs/threads).






