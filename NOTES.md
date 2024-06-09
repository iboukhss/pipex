# Notes

Old stuff:
- [x] open()
- [x] close()
- [x] read()
- [x] write()
- [x] malloc()
- [x] free()

New stuff:
- [x] perror()
- [x] strerror()
- [x] access()
- [x] dup()
- [x] dup2()
- [x] execve()
- [x] exit()
- [ ] fork()
- [ ] pipe()
- [ ] unlink()
- [ ] wait()
- [ ] waitpid()

## perror

Usage:
```c
perror("Error opening file");
```

Displays `Error opening file: No such file or directory` for example.
See `man errno` for more information.

## strerror

Returns a pointer to a string containing the current error message.

Usage:
```c
printf("Error opening file: %s\n", strerror(errno));
```

Feels, less handly and more complicated to use than `perror()` at first glance.

## access

Checks if a file can be accessed depending on different access modes `amode`.
* `F_OK` checks if the file exists
* `R_OK`, `W_OK`, `X_OK` check if the the file exists and has read, write and execute permissions respectively.

Usage:
```c
if (access("myfile.txt", F_OK) != -1)
    printf("File exists.\n);
```

Note: `access()` returns `0` if no error occured or `-1` if an error happened and also sets errno accordingly.

## dup and dup2

`dup()` returns a copy of the fd passed as an argument.
`dup2()` makes fd2 a copy of fd1, where fd1 and fd2 are the first and second arguments respectively. If fd2 was already opened `dup2()` will close it before duplicating fd1.

Note: in case of an error, both functions return `-1` and set errno accordingly.

`dup()` usage:
```c
int fd1 = open("myfile.txt", O_RDONLY);
int fd2 = dup(fd1);

// fd1 and fd2 now point to the same file

close(fd1);
close(fd2);
```

`dup2()` usage:
```c
int fd1 = open("myfile.txt", O_RDONLY);
int fd2 = open("otherfile.txt", 0_RDONLY);
dup2(fd1, fd2);

// fd2 now points to the same location as fd1
// otherfile.txt is no longer opened

close(fd1);
close(fd2);
```

## execve

Executes a process with a specific set of arguments.

Usage:
```c
char *argv[] = {"/bin/ls", "-l ", NULL};
char *envp[] = {NULL};

execver(argv[0], argv, envp);

perror("execve");


## exit

Terminates the current program.

Usage:
```c
printf("About to exit..\n);
exit(0);
```

## fork

Long story.

## pipe

Usage:
```c
int pipefd[2];

pipe(pipefd);

// read end is pipefd[0]
// write end is pipefd[1]
```

## unlink

Remove a file?

Usage:
```c
unlink("/path/to/file");
```

## wait and waitpid

Related to `fork()` these functions help waiting for child processes in general.
