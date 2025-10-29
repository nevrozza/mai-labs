# **Навигация** (pwd, ls, cd, touch*, mkdir*)

```bash
:~$ pwd
```
>/home/ubuntu

```bash
:~$ mkdir mai-test
:~$ ls
```
>mai-test
```bash
:~$ touch mai-test/dummyFile
:~$ ls mai-test
```
>dummyFile
```bash
:~$ cd //
://$ ls
```
>bin  bin.usr-is-merged  boot  dev  etc  home  lib  (…и тд!)
```bash
://$ cd home/ubuntu
://home/ubuntu$ ls
```
>mai-test
```bash
://home/ubuntu$ cd ~
:~$ cd mai-test
:~/mai-test$ ls
```
>dummyFile

# **Работа с файлами, директориями, данными** (rm(dir), cat, mv, cp) + абсолютные и относительные пути
```bash
:~/mai-test$ nano dummyFile  # dummyFile content!!
:~/mai-test$ cat dummyFile
```
>dummyFile content!!
```bash
:~/mai-test$ mv dummyFile ..
:~/mai-test$ cd ..
:~$ ls
```
>dummyFile  mai-test
```bash
:~$ rmdir mai-test
:~$ ls
```
>dummyFile
```bash
:~$ cp dummyFile dummyFileCopy
:~$ ls
```
>dummyFile  dummyFileCopy
```bash
:~$ rm dummyFile
:~$ ls
```
>dummyFileCopy
```bash
:~$ cd /home/ubuntu/newFolder
:~/newFolder$ touch newFolderFile
:~/newFolder$ ls
```
>newFolderFile

# Ключи, кавычки
```bash
:~/newFolder$ cd ~
:~$ ls
```
>dummyFileCopy  newFolder
```bash
:~$ ls newFolder
```
>newFolderFile
```bash
:~$ rm -r newFolder
:~$ ls
```
>dummyFileCopy
```bash
:~$ mkdir 'folder with spaces'
:~$ ls
```
>dummyFileCopy  'folder with spaces'
```bash
:~$ cd 'folder with spaces'
:~/folder with spaces$ touch file\ with\ escaping
:~/folder with spaces$ ls
```
>'file with escaping'
```bash
:~/folder with spaces$ mkdir -p d1/d2/d3/d4
:~/folder with spaces$ cd d1/d2/d3/d4
:~/folder with spaces/d1/d2/d3/d4$
```

# **Успех/неуспех** ($?, &&, ||, !, if)
```bash
:~/folder with spaces/d1/d2/d3/d4$ mkdir 1/1/1
```
>mkdir: cannot create directory ‘1/1/1’: No such file or directory
```bash
:~/folder with spaces/d1/d2/d3/d4$ $?
```
>1: command not found
```bash
:~/folder with spaces/d1/d2/d3/d4$ mkdir -p 1/1/1
```
>0: command not found
```bash
:~/folder with spaces/d1/d2/d3/d4$ cd ~
:~$ rm -r 'folder with spaces'
```

```bash
:~$ if mkdir p1; then touch p1/meow; else echo 'cry'; fi
:~$ ls p1
```
>meow
```bash
:~$ if mkdir p1; then touch p1/meow; else echo 'cry'; fi
```
>mkdir: cannot create directory ‘p1’: File exists
>
>cry
```bash
:~$ if mkdir p1; then touch p1/meow; elif mkdir p2; then touch p2/meow2 ; else echo 'cry more.'; fi;
```
>mkdir: cannot create directory ‘p1’: File exists
```bash
:~$ ls p2
```
>meow2
```bash
:~$ if mkdir p1; then touch p1/meow; elif mkdir p2; then touch p2/meow2 ; else echo 'cry more.'; fi;
```
>mkdir: cannot create directory ‘p1’: File exists
>
>mkdir: cannot create directory ‘p2’: File exists
>
>cry more.

```bash
:~$ if ! mkdir p1; then echo 'Не удалось создать директорию p1'; fi
```
>mkdir: cannot create directory ‘p1’: File exists
>
>Не удалось создать директорию p1



```bash
:~$ if mkdir p1 || mkdir p3; then touch p3/newFile; fi
```
>mkdir: cannot create directory ‘p1’: File exists
```bash
:~$ ls p3
```
>newFile
```bash
:~$ if mkdir p1 || mkdir p3; then touch p3/newFile; fi
```
>mkdir: cannot create directory ‘p1’: File exists
>
>mkdir: cannot create directory ‘p3’: File exists

```bash
:~$ if mkdir p1 && mkdir p2; then touch p1/1; touch p2/2; else echo 'these folders are already created'; fi
```
>mkdir: cannot create directory ‘p1’: File exists
>
>these folders are already created

```bash
:~$ rm -r p1 p2
:~$ if mkdir p1 && mkdir p2; then touch p1/1; touch p2/2; else echo 'these folders are already created'; fi
:~$ ls p1
```
>1
```bash
:~$ ls p2
```
>2


# Перенаправление
```bash
:~$ ls
```
>1  dummyFileCopy  p1  p2  p3
```bash
:~$ ls > list.txt
:~$ cat list.txt
```
>1
>
>dummyFileCopy
>
>list.txt
>
>p1
>
>p2
>
>p3
```bash
:~$ echo "str1" > list.txt
:~$ cat list.txt
```
>str1
```bash
:~$ echo "str2" >> list.txt
```
>str1
>
>str2

```bash
:~$ rm nonExistingFile 2> errors.log
:~$ cat errors.log
```
>rm: cannot remove 'nonExistingFile': No such file or directory




```bash
:~$ (if ! mkdir cant/create; then echo 'Не удалось создать папку, подробнее в логах'; fi;) > if_output.log 2> if_errors.log
:~$ cat <if_output.log
```
>Не удалось создать папку, подробнее в логах

```bash
:~$ cat <if_errors.log
```
>mkdir: cannot create directory ‘cant/create’: No such file or directory

```bash
~$ cat <<END
> cтрока 1
> строка 2
> строка 3
> END
```
>cтрока 1
>
>строка 2
>
>строка 3

```bash
:~$ cat <<< "just a string"
```
>just a string
```bash
:~$ (if ! mkdir cant/create; then echo 'Не удалось создать папку, подробнее ВЫШЕ'; fi;) > if_full_output.log 2>&1
:~$ cat <if_full_output.log 
```
>mkdir: cannot create directory ‘cant/create’: No such file or directory
>
>Не удалось создать папку, подробнее ВЫШЕ


# Конвейеры (bc, grep, head, tail, wc)
```bash
:~$ touch file_for_pipe
:~$ nano file_for_pipe
str1  # Ввод
str2
str3
num1
num2
num3
str4
```

```bash
:~$ cat file_for_pipe | wc
```
>7 7 35
```bash
:~$ cat file_for_pipe | grep "str"
```
>str1
>
>str2
>
>str3
>
>str4
```bash
:~$ cat file_for_pipe | grep "str" | wc
```
>4 4 20
```bash
:~$ cat file_for_pipe | grep "str" | wc > file.p
:~$ cat file.p
```
>4 4 20
```bash
:~$ cat file_for_pipe | grep "str" | wc -w
```
>4


```bash
:~$ touch grep_file
:~$ nano grep_file
1 * 2  # Ввод
91 * 123
str1
10 * 10
пятнадцать на пять
```

```bash
:~$ echo "15 * 20" | bc
```
>300
```bash
:~$ cat grep_file | grep -E "^[0-9]+ \* [0-9]+$" | bc
```
>2
>
>11193
>
>100
```bash
:~$ cat > test_file (1..14)
:~$ cat test_file | head
```
>1..10
```bash
:~$ cat test_file | head -5
```
>1..5
```bash
:~$ cat test_file | tail
```
>5..14
```bash
:~$ cat test_file | tail -5
```
>10..14


# Подстановка ($, vars, ${}, ${#A}, ${A:start:end}, $(…), $((выражение)) )

``` bash
:~$ A= 123
```
>[!Caution]
>>123: command not found
```bash
:~$ A =123
```
>[!Caution]
>>A: command not found
```bash
:~$ A=123
:~$ echo A
```
>[!Caution]
>>A
```bash
:~$ echo $A
```
>123

```bash
:~$ echo 'sad $A'
```
>[!Caution]
>>sad $A

```bash
:~$ echo "sad $A"
```
>sad 123

```bash
:~$ num=5
:~$ echo "номер: ${num}-ый"
```
>номер: 5-ый
```bash
:~$ num_word=пять
:~$ echo "${num_word:0:3}ый номер"
```
>пятый номер
```bash
:~$ word=#ЛЮБЛЮБАШ
:~$ echo "кол-во символов: ${#word}"
```
>кол-во символов: 9
```bash
:~$ dir_log=$(ls)
:~$ echo $dir_log
```
>1 cat dummyFileCopy echo errors.log file.p file_for_pipe grep_file if_errors.log if_full_output.log if_output.log list.txt p1 p2 p3 test test_file textik
```bash
:~$ jsVibe=2+3
:~$ echo $jsVibe
```
>[!Caution]
>>2+3
```bash
:~$ calc_output=$((2+3))
:~$ echo $calc_output
```
>5


# Метасимволы (*, ?, [], {})
>[!Important]
>Make some generations in ‘~’
```bash
:~$ ls
```
>1.txt  2.txt  3.txt  4.txt  5.txt  6.txt  7.txt  8.txt  9.txt alotofsymbolstest{1}.txt file_a.bat  file_b.bat  file_c.bat  file_d.bat  file_e.bat
```bash
:~$ ls *.txt (10 args)
```
>1.txt  2.txt  3.txt  4.txt  5.txt  6.txt  7.txt  8.txt  9.txt alotofsymbolstest{1}.txt
```bash
:~$ ls file* (5 args)
```
>file_a.bat  file_b.bat  file_c.bat  file_d.bat  file_e.bat
```bash
:~$ ls *e* // все файлы, содержащие ‘e’
```
>alotofsymbolstest{1}.txt  file_a.bat  file_b.bat  file_c.bat  file_d.bat  file_e.bat

```bash
:~$ mkdir t{1..5}
:~$ touch t{1..5}/file.txt
:~$ ls */*.txt
```
>t1/file.txt  t2/file.txt  t3/file.txt  t4/file.txt  t5/file.txt

```bash
:~$ rm -r *
:~$ touch file_{a..e}
:~$ touch file_{a..e}{1..5}
:~$ ls
```
>file_a   file_a2  file_a4  file_b   file_b2  file_b4  file_c   file_c2  file_c4  file_d   file_d2 file_d4  file_e   file_e2  file_e4 file_a1  file_a3  file_a5  file_b1  
>file_b3  file_b5  file_c1 file_c3  file_c5  file_d1  file_d3  file_d5  file_e1  file_e3  file_e5
```bash
:~$ ls file_[abc]
```
>file_a  file_b file_c
```bash
:~$ touch file_{a..e}{1..5}1 // добавил файлы с 3-мя знаками после ‘file_’
:~$ ls file_[ab][13]
```
>file_a1  file_a3  file_b1  file_b3

```bash
:~$ ls file_[abc][345]1
```
>file_a31  file_a41  file_a51  file_b31  file_b41  file_b51  file_c31  file_c41  file_c51


```bash
:~$ ls file_a{1..7} (7 args)
```
>ls: cannot access 'file_\[a]6': No such file or directory
>
>ls: cannot access 'file_\[a]7': No such file or directory
>
> file_a1   file_a2   file_a3   file_a4   file_a5
```bash
:~$ ls file_a? (5 args)
```
>file_a1  file_a2  file_a3  file_a4  file_a5

```bash
:~$ touch test[12]
:~$ ls
```
>[!CAUTION]
>>test\[12]

```bash
:~$ touch test{1,2} (2 args – создаёт test1 и test2)
```

# Права доступа (chmod, auog, rwx, r=4, w=2, x=1)
```bash
:~$ touch file.txt script.sh
:~$ mkdir testDir
:~$ ls -l
```
>total 4
>
>-rw-rw-r-- 1 ubuntu ubuntu    0 Sep 28 17:23 file.txt
>
>-rw-rw-r-- 1 ubuntu ubuntu    0 Sep 28 17:23 script.sh
>
>drwxrwxr-x 2 ubuntu ubuntu 4096 Sep 28 17:23 testDir
```bash
:~$ sh script.sh
```
>text
```bash
:~$ chmod u-r script.sh
```
>sh: 0: cannot open script.sh: Permission denied
```bash
:~$ chmod a+r script.sh
:~$ sh script.sh
```
>text
```bash
:~$ chmod o-r file.txt
:~$ ls -l
```
>-rw-rw---- 1 ubuntu ubuntu    0 Sep 28 17:23 file.txt
```bash
:~$ chmod a-x testDir/
:~$ cd testDir/
```
>-bash: cd: testDir/: Permission denied
```bash
:~$ chmod a+x-r testDir/
:~$ cd testDir/
:~/testDir$ touch file
:~/testDir$ ls
```
>ls: cannot open directory '.': Permission denied
```bash
:~/testDir$ chmod a+x+r .
:~/testDir$ ls
```
>File
```bash
nano File  # some text
:~/testDir$ cat file
```
>some text
```bash
:~/testDir$ chmod u-r file
```
>cat: file: Permission denied
```bash
:~/testDir$ chmod 444 .
:~/testDir$ touch "can't create it"
```
>touch: cannot touch "can't create it": Permission denied
```bash
:~/testDir$ cd .
```
>-bash: cd: .: Permission denied
```bash
:~/testDir$ cd ..
:~$ rm -r testDir/
```
>rm: descend into write-protected directory 'testDir/'?
```bash
:~$ chmod 777 testDir
:~$ cd testDir
:~/testDir$ chmod a-r file
:~/testDir$ cat file
```
>cat: file: Permission denied
```bash
:~/testDir$ ls -l
```
>total 4
>
>-----w---- 1 ubuntu ubuntu 10 Sep 28 17:34 file
```bash
:~/testDir$ cd ..
:~$ chmod -R 777 testDir/
:~$ ls testDir/ -l
```
>total 4
>
>-rwxrwxrwx 1 ubuntu ubuntu 10 Sep 28 17:34 file

# Работа с процессами (jobs, fg, bg, disown, ctrl+c, ctrl+z, ps, kill, kill -9)
```bash
:~$ ping ya.ru&
echo "keyboard is free"
```
>keyboard is free
```bash
jobs
```
| | |
| :---:              | :---:                  |
| \[1]+  Running      |           ping ya.ru & |
```bash
:~$ fg
CTRL+C
```
>--- ya.ru ping statistics ---
>
>57 packets transmitted, 0 received, 100% packet loss (окак), time 59857ms

```bash
:~$ jobs
:~$ ps
```
| PID TTY      |    TIME CMD |
| :---:              | :---:                  |
| 89375 pts/0    | 00:00:00 bash |
| 166538 pts/0    | 00:00:00 ps |
```bash
:~$ ping ya.ru&
bg
```
>-bash: bg: job 1 already in background

```bash
(terminate)
:~$ ping ya.ru
CTRL+Z
```
| | |
| :---:              | :---:                  |
| \[1]+  Stopped        |         ping ya.ru |

```bash
:~$ ping ya.ru&
jobs
```
| | |
| :---:              | :---:                  |
| \[1]+  Stopped         |        ping ya.ru |
| \[2]-  Running         |        ping ya.ru & |
 
```bash
:~$ fg 1
CTRL+C
:~$ jobs
```
| | |
| :---:              | :---:                  |
| \[2]+  Running         |        ping ya.ru & |

```bash
:~$ fg 2
^Z
```
| | |
| :---:              | :---:                  |
| \[2]+  Stopped           |      ping ya.ru |
```bash
:~$ bg 2
:~$ jobs
```
| | |
| :---:              | :---:                  |
| \[2]+  Running         |        ping ya.ru & |

```bash
:~$ ps
```
|    PID TTY       |   TIME CMD |
| :---:              | :---:                  |
|  89375 pts/0  |  00:00:00 bash |
| 173218 pts/0  |  00:00:00 ping |
| 175025 pts/0  |  00:00:00 ps |

```bash
:~$ ps -e
```
>…  89375 pts/0    00:00:00 bash…

```bash
:~$ jobs
```
| | |
| :---:              | :---:                  |
| \[2]+  Running         |        ping ya.ru & |
```bash
:~$ kill 2
```
>[!CAUTION]
>>-bash: kill: (2) - Operation not permitted

>[!TIP]
>Ниже будет пример

```bash
:~$ ps
```

|    PID TTY     |     TIME CMD |
| :---:              | :---:                  |
|  89375 pts/0  |   00:00:00 bash |
| 173218 pts/0  |  00:00:00 ping |
| 177145 pts/0  |  00:00:00 ps |

```bash
:~$ kill 173218
:~$ jobs
```


| | |
| :---:              | :---:                  |
| \[2]+  Terminated        |      ping ya.ru |

```bash
:~$ jobs  # Нет процессов
:~$ ping ya.ru&
ps
```

|    PID TTY      |    TIME CMD |
| :---:              | :---:                  |
|  89375 pts/0  |  00:00:00 bash |
| 179790 pts/0  |  00:00:00 ping |
| 179854 pts/0  |  00:00:00 ps |


```bash
:~$ kill -9 179790
:~$ jobs
```

| | |
| :---:              | :---:                  |
| \[1]+  Killed          |        ping ya.ru |

>[!TIP]
>Пример

```bash
:~$ ping ya.ru&
jobs
```

| | |
| :---:              | :---:                  |
| \[1]+  Running         |        ping ya.ru & |

```bash
kill %1
:~$ jobs
```

| | |
| :---:              | :---:                  |
| \[1]+  Terminated     |         ping ya.ru |

```bash
:~$ ping ya.ru&
jobs
```
| | |
| :---:              | :---:                  |
| \[1]+  Running         |        ping ya.ru & |
```bash
:~$ disown
:~$ jobs  # Нет процессов
:~$ ps
```


|    PID TTY   |    TIME CMD |
| :---:              | :---:                  |
|  89375 pts/0  |  00:00:00 bash |
| 186030 pts/0  |  00:00:00 ping |
| 187326 pts/0  |  00:00:00 ps |
 
```bash
:~$ fg 186030
```

> -bash: fg: 186030: no such job

```bash
:~$ kill 186030
:~$ ps
```


|    PID TTY      |    TIME CMD |
| :---:              | :---:                  |
|  89375 pts/0    | 00:00:00 bash |
| 189894 pts/0   | 00:00:00 ps |
 

```bash
:~$ kill -9 89375 
```
> У меня просто перезапускается терминал =)















