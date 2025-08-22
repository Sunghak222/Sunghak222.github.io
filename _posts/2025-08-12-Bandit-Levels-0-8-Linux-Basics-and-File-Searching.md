---
title: "Bandit Levels 0-8: Linux Basics and File Searching"
date: 2025-08-12
categories: [Linux]
tags: []
---

## Notes
---
This post is my note for studying Bandit.
It doesn’t show full solutions since these levels are basic and many good explanations already exist on other blogs.

## Bandit Level 0
---
This level is to learn how to log in to the server using SSH on port **2220**

```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220
```

You can exit the server with the command below:
```bash
exit
```

### SSH
---

**SSH** (Secure Shell) is a protocol that helps you to securely connect to the remote server.

## Bandit Level 0 -> 1
---
To list files
```bash
ls [option] [path]
```

| Option | Description                                                                                | Example  |
| ------ | ------------------------------------------------------------------------------------------ | -------- |
| `-l`   | Displays detailed information in long format (permissions, owner, size, modification time) | `ls -l`  |
| `-a`   | Includes hidden files (files starting with `.`)                                            | `ls -a`  |
| `-h`   | Shows file sizes in a human-readable format (KB, MB, GB)                                   | `ls -lh` |
| `-t`   | Sorts files by modification time, newest first                                             | `ls -lt` |
| `-S`   | Sorts files by size, largest first                                                         | `ls -lS` |
| `-r`   | Reverses the sorting order                                                                 | `ls -lr` |
| `-R`   | Lists files recursively, including subdirectories                                          | `ls -R`  |

To see file content
```bash
cat [filepath]
```

## Bandit Level 1 -> 2
---
The command below finds files with a user bandit7, group bandit6, and size 33 bytes.

```bash
find / -user bandit7 -group bandit6 -size 33c
```

| Option        | Description                                                                         | Example                               |
| ------------- | ----------------------------------------------------------------------------------- | ------------------------------------- |
| `-name`       | Search by file name (supports wildcards `*` and `?`)                                | `find . -name "file.txt"`             |
| `-type`       | Search by file type (`f`=file, `d`=directory, `l`=symlink, etc.)                    | `find . -type f`                      |
| `-size`       | Search by file size (`c`=bytes, `k`=KB, `M`=MB, `G`=GB)                             | `find / -size 33c`                    |
| `-user`       | Search files owned by a specific user                                               | `find /home -user bandit7`            |
| `-group`      | Search files owned by a specific group                                              | `find /var -group bandit6`            |
| `-perm`       | Search files by permission bits                                                     | `find . -perm 644`                    |
| `-mtime`      | Search by modification time in *days* (`-1`=less than 1 day, `+7`=more than 7 days) | `find /var -mtime -1`                 |
| `-atime`      | Search by last access time in *days*                                                | `find / -atime +30`                   |
| `-ctime`      | Search by last status change time in *days*                                         | `find / -ctime 0`                     |
| `-exec`       | Execute a command on found files (`{}` = placeholder, `\;` to end)                  | `find . -name "*.log" -exec rm {} \;` |
| `-print`      | Explicitly print results (default in most cases)                                    | `find /etc -name "*.conf" -print`     |
| `-maxdepth N` | Limit search to *N* levels of directories                                           | `find . -maxdepth 1 -type f`          |
| `-mindepth N` | Ignore matches until at least *N* levels deep                                       | `find . -mindepth 2 -name "*.txt"`    |

## Bandit Level 2 -> 3
---
This level teaches how to handle filenames containing spaces. 
In Linux, spaces are treated as separators, so you need to escape them with a backslash (\) when accessing such filenames.

```bash
cat spaces\ in\ this\ filename
cat ./--spaces\ in\ this\ filename-- 
```

./ means a current directory.

## Bandit Level 3 -> 4
---
This concept is already covered in bandit level 0 -> 1. The option -a shows hidden files, which filename begins with `.`, such as .ssh, .git, .gitignore, and many others.

```bash
ls -a
```

## Bandit Level 4 -> 5
---
The command file also supports wildcards `*`.

```bash
file ./*
```

## Bandit Level 5 -> 6
---
```bash
find . -size 1033c ! -executable -type f
```
This command finds files with size 1033 bytes and no executable.

`-type f`: not a directory.  
`-size 1033c`: 1033 bytes. `c` means byte.  

## Bandit Level 6 -> 7
---
```bash
find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
```

`/`: the whole files from root  

`stdout`: file descripter 1  
`stderr`: file descripter 2  
`/dev/null`: A trashcan  
`2>/dev/null`: Throw all the error messages away to trashcan  

## Bandit Level 7 -> 8
---
#### Usage

```bash
grep [options] "pattern" [file...]
```

#### Example
```bash
grep 'millionth' data.txt
```

#### Commonly Used Options

| Option   | Description                                  | Example                      |                 |
| -------- | -------------------------------------------- | ---------------------------- | --------------- |
| `-c`     | Prints the **number of matching lines**      | `grep -c 'foo' data.txt`     |                 |
| `-i`     | Ignore case when matching                    | `grep -i 'foo' data.txt`     |                 |
| `-v`     | Print **non-matching lines**                 | `grep -v 'foo' data.txt`     |                 |
| `-n`     | Show the **line number** of matches          | `grep -n 'foo' data.txt`     |                 |
| `-l`     | Show only filenames containing matches       | `grep -l 'foo' *.txt`        |                 |
| `-w`     | Match **whole words only**                   | `grep -w 'foo' data.txt`     |                 |
| `-x`     | Match the **entire line exactly**            | `grep -x 'hello' data.txt`   |                 |
| `-r`     | Search **recursively** in all subdirectories | `grep -r 'foo' ./logs`       |                 |
| `-m [N]` | Limit the number of matches shown            | `grep -m 3 'foo' data.txt`   |                 |
| `-E`     | Use **extended regular expressions**         | \`grep -E 'foo               | bar' data.txt\` |
| `-F`     | Search for fixed strings (faster)            | `grep -F 'foo*bar' data.txt` |                 |
