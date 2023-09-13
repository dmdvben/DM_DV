# Linux Commands Overview

This document provides an overview of basic Linux commands. Each command is accompanied by a brief description and a small example.

## Table of Contents

1. [pwd](#pwd---print-working-directory)
2. [ls](#ls---list-directory-contents)
3. [cd](#cd---change-directory)
4. [mkdir](#mkdir---make-directory)
5. [touch](#touch---create-an-empty-file)
6. [rm](#rm---remove-files)
7. [rmdir](#rmdir---remove-directory)

### `pwd` - Print Working Directory
The `pwd` command displays the current working directory.
#### Example:
````bash
pwd
````
Output
````text
/home/user
````

### `ls` - List Directory Contents
The `ls` command lists the files and directories in the current directory.
#### Example:
````bash
ls
````
Output
````text
Desktop  Documents  Downloads
````

### `cd` - Change Directory
The `cd` command is used to change the current directory. You can navigate through directories using relative or absolute paths, as well as special characters to move up levels.
#### Examples:
1. **Change to a Specific Directory (Absolute Path)**
    ```bash
    cd /home/user/Documents
    ```
    This will navigate to the `Documents` directory located under `/home/user/`.

2. **Navigate to a Subfolder (Relative Path)**
    Assuming you are in `/home/user/Documents/`
    ```bash
    cd Subfolder
    ```
    This will navigate to `Subfolder` inside the `Documents` directory.
   
3. **Go One Level Up**
    ```bash
    cd ..
    ```
   This will move you one directory level up.
  
4. **Navigate to Home Directory**
    ```bash
    cd ~
    ```
    This will navigate you to your home directory, usually `/home/user/`.

### `mkdir` - Make Directory
The `mkdir` command is used to create a new directory.
#### Example:
````bash
mkdir new_folder
````

### `touch` - Create an Empty File
The `touch` command is used to create an empty file.
#### Example:
````bash
touch new_file.txt
````

### `rm` - Remove Files
The `rm` command is used to remove files.
#### Example:
````bash
rm new_file.txt
````

### `rmdir` - Remove Directory
The `rmdir` command is used to remove an empty directory.
#### Example:
````bash
rmdir new_folder
````



