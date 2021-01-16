# THM - Learn Linux
> https://tryhackme.com/room/zthlinux

### Basic Commands

- **Basic Commands**

    ls; cat; touch; ./

- **>**

    Output Redirection (writes to file)

- **>>**

    Output redirection (appends to file)

- **&&, and**

    Executes a 2nd command after the first one ran successfully

- **&**

    Background operator, the command will run in the background

- **$**

    Environment variable 

    Example: (prints current variable) 
    ```bash
        echo $USER 
    ```
    To set this kind of variable use 
    ```bash
        export varname=value
    ```

- **locate** 

    Locate a file (use **updatedb** before to make sure that it looks through every folder available even the newest ones)

- **passwd**

    Change password

- **man**

    Manual for commands

- **|**

    Pipe, takes the output of a command and uses it as input for a second command

- **;**

    It's like **&&** however it does  not require the first command to run successfully

### Basic Regular Expressions

With tr, sed, vi and grep ; name is either 'regexp', 'regex'

- **.**

    Replaces any character

- **^**

    Matches start of the string

- **$**

    Matches end of the string

- **\***

    Matches up zero or more times the preceding character

- **\**

    Represent special characters

- **()**

    Group regular expressions

- **?**

    Matches up exactly one character

- **{n}**

    Matches the preceding character appearing 'n' times exactly

- **{n,m}**

    Matches the preceding character appearing 'n' times 

- **{n,}**

    Matches the preceding character only if it appears 'n' times or more

You need to use -E with the last three regexps

- **\\+**

    Matches one or more occurence of the previous character
    ```bash
        cat sample | grep "a\+t"
    ```
    Filters lines where character 'a' precedes 't'

- **\\?**

    Matches zero or one occurence of the previous character

### Commands for Users

- **whoami**

    Command that states your current user 

- **sudo**

    Administrator you can also run as another user with **-u**

- **adduser**/**addgroup**

    Add users and groups
    ```bash
        usermod -a -G <groups separated by a comma> <user>
    ```
    Add user to group with the syntax above

- **id**

    Command that allows to view basic info about a user

- **nano**

    Terminal based text editor
    ```bash
        nano <file>
    ```
    ctrl x → exit

### Useful Commands

- **which**

    Command that shows you where an executable is in any of the PATH directories

- **apt**

    Install packages needs root cause we're making changes to the **/usr** directory
    ```bash
        apt install <package> 
    ```
- **ps**

    Shows list of user created processes

    **-ef** lists all of the system processes

    - **PID**

        Process id

- **kill**

    Kills processes
    ```bash
        kill <PID>
    ```
- **top**

    Shows what processes are taking up the most system resources, which allows you to manage the resource allocation on your system by killing unneeded processes

### File Operators

- **chmod**

    Sets the different permissions for a file

    1 → x

    2 → w

    4 → r

- **chown**

    Edits who owns the file for user and group (user: group)

- **mv**

    Moves smth or renames it
    ```bash
        mv <file> <destination>
    ```
- **cp**

    Copies file to a directory same syntax as **mv**

- **mkdir**

    Creates a directory
    ```bash
        mkdir <directoryname>
    ```
- **ln**
    1. Hardlinking

        Duplicates the file and links the duplicate to the original . What is done to the created link is done to  the original too.

    2. Symbolic linking(symlink)

        Reference to  the original copy. Symbolic Link has no data in it at all, it's just a reference.

    3. Syntax

        Symlink is  the same as hardlink but with an **-s** flag
        ```bash
            ln -s <file> <destination>
        ```
    **ls** shows that a file is a symbolic link with an arrow pointing to the original file

    The symlink has the same perms as the original file

- **find**

    Allows the user to find files but only shows files that the user has access to

- **grep**

    Allows the user to find data inside of data, helps narrow down the input to what we're looking for

    Files are optional if we are the piping
    ```bash
        grep <regular expressions> <files>
    ```
### Brace Expansions

The syntax for a brace expansion is either a sequence or a comma separated list of items inside curly braces '{}'. The starting and ending items in a sequence are separated by two periods ".."
```bash
    echoa{0..2}b
```
*a0b a1b a2b*

### Basic Shell Scripting

Store command in a file with a **.sh** extension if we want to run bash script but  if we add a shebang (#!/bin/bash)(which is the path to shell) at the top of the script and then to run we use **./<file\>**

### Important Directories

- **/etc/passwd**

    Stores user information often used to see all the users on a system

- **/etc/shadow**

    Has all the passwords of these users

- **/tmp**

    Every file  inside it gets deleted upon shutdown, it's used for temporary files

- **/etc/sudoers**

    Used to control the **sudo** permissions of every user on the system

- **/home**

    The directory where all your downloads, documents, etc. are like **C:\Users\\<user\>**

- **/root**

    The root user's home directory like **C:\Administrator**

- **/usr**

    Where all your software is installed

- **/bin** and **/sbin**

    Used for system critical files - ***DO NOT DELETE***

- **/var**

    The linux miscellaneous directory, a myriad of processes store data in **/var**

- **$PATH**

    Stores all the binaries you're able to run same as **$PATH** on Windows