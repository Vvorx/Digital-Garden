---
title: "OverTheWire bandit: levels 6-10"
date: 2025-09-25T21:40:38+01:00
summary: My approach to levels 6-10 on OverTheWire bandit
description:
toc:
readTime:
autonumber:
tags: []
showTags:
hideBackToTop:
---
## Bandit Level 6

#### Task

The password for the next level is stored **somewhere on the server** and has all of the following properties:

- owned by user bandit7
- owned by group bandit6
- 33 bytes in size
#### Approach

I used the `find` command with the following options:

- `-type f`, specifies that we're looking for a file
- `-user bandit7`, finds files that are owned by the user "bandit7"
- `-group bandit6`, finds files that's are owned by the group "bandit6"
- `-size 33c`, looks for a file that's 33 bytes

````bash
bandit6@bandit:~$ find / -type f -user bandit7 -group bandit6 -size 33c
find: ‘/sys/kernel/tracing/osnoise’: Permission denied
find: ‘/sys/kernel/tracing/hwlat_detector’: Permission denied
find: ‘/sys/kernel/tracing/instances’: Permission denied
find: ‘/sys/kernel/tracing/trace_stat’: Permission denied
find: ‘/sys/kernel/tracing/per_cpu’: Permission denied
find: ‘/sys/kernel/tracing/options’: Permission denied
find: ‘/sys/kernel/tracing/rv’: Permission denied
find: ‘/sys/kernel/debug’: Permission denied
find: ‘/sys/fs/pstore’: Permission denied
find: ‘/sys/fs/bpf’: Permission denied
find: ‘/root’: Permission denied
find: ‘/boot/lost+found’: Permission denied
find: ‘/boot/efi’: Permission denied
find: ‘/run/udisks2’: Permission denied
........
````

You'll notice that we got a lot of permission denied messages

We can add `2>/dev/null` to help us hide all of those

````bash
bandit6@bandit:~$ find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null
/var/lib/dpkg/info/bandit7.password
````

/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

## Bandit Level 7

#### Task

The password for the next level is stored in the file **data.txt** next to the word **millionth**

#### Approach

I used `grep` to search lines that follow a specific pattern, we can pipe ``
`cat` to `grep` as input to look through the text file.

````bash
cat data.txt | grep millionth
````

simple as that :)

/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

## Bandit Level 8

#### Task

The password for the next level is stored in the file **data.txt** and is the only line of text that occurs only once

#### Approach

`uniq` is the perfect command for this task, it filters through input based on identical lines, The flag `-u` filters for unique lines which will become very useful when searching for the password in this text file.

`sort` can be used to sort the lines in the text file, which will allow `uniq` to find the line that only occurs once 

```bash
bandit8@bandit:~$ sort data.txt | uniq -u
UsvVyFSfZZWbidoityourself
```

/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

## Bandit Level 9

#### Task

The password for the next level is stored in the file **data.txt** in one of the few human-readable strings, preceded by several "=" characters.

#### Approach

The `strings` command finds human-readable strings in files, To be specific it prints sequences of printable characters.

First I used the `strings` command in "data.txt". 

Next, I filter the output by looking at lines that feature more than one "=" by piping it into `grep`. 

````bash
bandit9@bandit:~$ strings data.txt | grep ===
========== the*2i"4
========== password
Z)========== is
````

/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

## Bandit Level 10

#### Task

The password for the next level is stored in the file **data.txt**, which contains base64 encoded data
#### Approach

The `base64` command allows files as input, so we just need to use the command on the file.

````bash
bandit10@bandit:~$ cat data.txt
VGhlIHBhc3N3b3JkIGlzIElGdWt3S0dzRlc4TU9xM0lSRnFyeEUxaHhUTkViVVBSCg==
bandit10@bandit:~$ base64 -d data.txt
The password is 
````


