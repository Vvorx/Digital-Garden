---
title: "OverTheWire bandit: levels 1-5"
date: 2025-09-25T10:04:57+01:00
summary: My approach to levels 1-5 on OverTheWire bandit
description:
toc:
readTime:
autonumber:
tags: []
showTags:
hideBackToTop:
---

For all of my approaches one thing I didn't mention is, if I didn't know any of the commands I just looked through the `man` pages, I would have a separate terminal or a virtual terminal open to let you have the `man` pages pulled up on the side. 

## Bandit Level 1

#### Task

The password is stored in a file called `-` located in the home directory 

#### Approach

I used  `cat ./-`  to print out the text in the file `-` is used as an argument so the full location of the file has to be specified in order for `cat` to find it

/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
## Bandit Level 2

#### Task

The password is stored in a file called `--spaces in this filename--`

#### Approach

Similar to the first level, I used `cat ./--spaces\ in\ this\ filename--` to print out the text, the location has to be specified again due to the dashes and backslash has to be used to let the terminal know about the spaces

in general it's bad practice to use spaces in filenames or directories as spaces are interpreted as **delimiters** (things that separate commands and or arguments). Instead, it's advised to use **underscores** or **dashes**

/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

## Bandit Level 3

#### Task

The password for the next level is stored in a hidden file in the **inhere** directory.

#### Approach

I checked for all files in the **inhere** directory via `ls -a inhere` which gave me the output

````bash
bandit3@bandit:~$ ls -a inhere
.  ..  ...Hiding-From-You
````

I then ran `cat inhere/...Hiding-From-You` to print out the password

/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

## Bandit Level 4

#### Task

The password for the next level is stored in the only human-readable file in the **inhere** directory. 

#### Approach

To check which files are human-readable in the **inhere** directory I ran `file inhere/*` which output all the file types in the directory


````bash
bandit4@bandit:~$ file inhere/*
inhere/-file00: Non-ISO extended-ASCII text, with no line terminators, with overstriking
inhere/-file01: data
inhere/-file02: data
inhere/-file03: data
inhere/-file04: data
inhere/-file05: data
inhere/-file06: data
inhere/-file07: ASCII text
inhere/-file08: data
inhere/-file09: data
````

Then I simply ran `cat inhere/-file07` to get the password

/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
## Bandit Level 5

#### Task

The password for the next level is stored in a file somewhere under the **inhere** directory and has all of the following properties:

- human-readable
- 1033 bytes in size
- not executable

#### Approach

Since there's a lot of directories to look through I run `find inhere/ -size 1033c ! -executable`  with `-size 1033c` specifying a file with 1033 bytes and `! -executable` specifying a non executable file

````bash
bandit5@bandit:~$ find inhere/ -size 1033c ! -executable
inhere/maybehere07/.file2
````

then as always I get the password using `cat`

`cat inhere/maybehere07/.file2`









