---
title: "Bandit Levels 8-13: Notes"
date: 2025-08-14
categories: [Linux]
tags: []
---

## Notes
---
This post is my note for studying Bandit.
It doesn’t show full solutions since these levels are basic and many good explanations already exist on other blogs.
I will explain higher levels in detail when I find it important.

## Bandit Level 8 -> 9
---

### Goal
This level is to extract a unique line in a file.

### Commands

To sort content of the file
```bash
sort [options] [file]
```

#### Commonly Used Options

| Option | Description                         | Example                 |
| ------ | ----------------------------------- | ----------------------- |
| `-n`   | Sort numerically                    | `sort -n numbers.txt`   |
| `-r`   | Reverse order                       | `sort -r data.txt`      |
| `-k N` | Sort by Nth column/field            | `sort -k2 scores.txt`   |
| `-t X` | Use X as delimiter (default: space) | `sort -t, -k2 data.csv` |
| `-u`   | Unique sort (remove duplicates)     | `sort -u names.txt`     |

uniq prints unique lines from sorted input.
It only removes **consecutive duplicates** → so it’s usually combined with sort.

```bash
uniq [options] [input_file] [output_file]
```

#### Commonly Used Options

| Option | Description                              | Example            |
| ------ | ---------------------------------------- | ------------------ |
| `-c`   | Count occurrences of each line           | `uniq -c data.txt` |
| `-d`   | Print only duplicate lines               | `uniq -d data.txt` |
| `-u`   | Print only unique lines (non-duplicates) | `uniq -u data.txt` |
| `-i`   | Ignore case when comparing               | `uniq -i data.txt` |

Extract the whole words in data.txt, sort it, and count the occurences of each line
```bash
cat data.txt | sort | uniq -c
```

## Bandit Level 9 -> 10
---

### Goal
This level is to search a binary file.

### Commands

We already learned grep, and we can also search binary files with the command below.
```bash
grep -a
```

strings extracts only strings in a file
```bash
strings data.txt
```

## Bandit Level 10 -> 11
---

### Goal

This level is to learn handling files with base64 format.

### Commands

Print out the file encoded with base64
```bash
base64 file.txt
```

Decode and print out the base64 file
```bash
base64 -d encoded_file.txt
```

## Bandit Level 11 -> 12
---

### Goal
This level is quite interesting. You should rotate the letters in the file by 13 positions, which is called Caesar cipher.

### Commands

`tr` translates characters from one set to another.
[A-Za-z]: A to Z + a to z
[N-ZA-Mn-za-m]: N to Z + A to M + n to z + a to m
```bash
tr '[A-Za-z]' '[N-ZA-Mn-za-m]'
```

### Further Learnings
#### Caesar Cipher
Caesar Cipher is a classic cipher where each letter in the plaintext is shifted by a fixed number of positions down the alphabet.

#### Example
Example with a shift of +3
* Plain alphabet: A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
* Cipher alphabet: D E F G H I J K L M N O P Q R S T U V W X Y Z A B C
  
Plaintext: Hello
Ciphertext: KHOOR

## Bandit Level 12 -> 13
---

### Goal
This level is to learn handling several compressed or encoded data.

### Commands
At first, data.txt is in hexdump, and it should be converted to binary since most compression tools cannot interpret hexdump. 

hexdump -> binary
```bash
xxr -r data.txt > data.bin
```

Decompression for each format
```bash
gzip -d

bzip2 -d

tar -xvf
```
