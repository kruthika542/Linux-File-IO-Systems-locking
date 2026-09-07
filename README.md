# Linux-File-IO-Systems-locking
Ex07-Linux File-IO Systems-locking
# AIM:
To Write a C program that illustrates files copying and locking

# DESIGN STEPS:

### Step 1:

Navigate to any Linux environment installed on the system or installed inside a virtual environment like virtual box/vmware or online linux JSLinux (https://bellard.org/jslinux/vm.html?url=alpine-x86.cfg&mem=192) or docker.

### Step 2:

Write the C Program using Linux IO Systems locking

### Step 3:

Execute the C Program for the desired output. 

# PROGRAM:

## 1.To Write a C program that illustrates files copying 
```
char block[1024];
int in, out;
ssize_t nread;

// Open source file
in = open(argv[1], O_RDONLY);
if (in == -1) {
    perror("Error opening source file");
    exit(EXIT_FAILURE);
}

// Open destination file
out = open(argv[2], O_WRONLY | O_CREAT | O_TRUNC, S_IRUSR | S_IWUSR);
if (out == -1) {
    perror("Error opening destination file");
    close(in);
    exit(EXIT_FAILURE);
}

// Copy contents
while ((nread = read(in, block, sizeof(block))) > 0) {
    if (write(out, block, nread) != nread) {
        perror("Error writing to destination file");
        close(in);
        close(out);
        exit(EXIT_FAILURE);
    }
}

if (nread == -1) {
    perror("Error reading source file");
}

close(in);
close(out);
return EXIT_SUCCESS;
```
## OUTPUT
<img width="1047" height="423" alt="image" src="https://github.com/user-attachments/assets/21c5c200-405a-492e-960f-c80c4eb263b0" />


## 2.To Write a C program that illustrates files locking
```
char *file = argv[1];
int fd;

printf("Opening %s\n", file);

fd = open(file, O_WRONLY);
if (fd == -1) {
    perror("Error opening file");
    exit(EXIT_FAILURE);
}

// Acquire shared lock
if (flock(fd, LOCK_SH) == -1) {
    perror("Error acquiring shared lock");
    close(fd);
    exit(EXIT_FAILURE);
}
printf("Acquired shared lock using flock\n");
display_lslocks();

sleep(1); // Simulate waiting before upgrading

// Try to upgrade to exclusive lock (non-blocking)
if (flock(fd, LOCK_EX | LOCK_NB) == -1) {
    perror("Error upgrading to exclusive lock");
    flock(fd, LOCK_UN); // Release shared lock if upgrade fails
    close(fd);
    exit(EXIT_FAILURE);
}
printf("Acquired exclusive lock using flock\n");
display_lslocks();

sleep(1); // Simulate waiting before unlocking

// Release lock
if (flock(fd, LOCK_UN) == -1) {
    perror("Error unlocking");
    close(fd);
    exit(EXIT_FAILURE);
}
printf("Unlocked\n");
display_lslocks();

close(fd);
return 0;
```
## OUTPUT

<img width="782" height="693" alt="image" src="https://github.com/user-attachments/assets/81449d3b-0fd5-469c-991e-77992fa81c74" />

# RESULT:
The programs are executed successfully.
