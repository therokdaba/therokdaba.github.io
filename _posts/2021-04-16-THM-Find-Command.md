# THM - The Find Command Cheatsheet

> https://tryhackme.com/room/thefindcommand

- **find \***

    Find all files

- **find \*1**

    Find all files that end in 1

- **-type**

    - **d**

        Find directories

    - **f**

        Find files

- **-name**

    Specify name or pattern to look for. Either type whole name or use wildcards to specify only parts of the game. If wildcards are used we need to use quotes.

- **-iname**

    Like name but case insensitive

- **-user**

    Specify the username of the owner of a file

- **-size**

    Specify size of the file

    (n equals to a number)

    - **-n**

        Matches value lesser than n

    - **+n**

        Matches values greater than n

    - **n**

        Matches exactly n

    We also need a suffix to specify size:

    - **c**

        Bytes

    - **k**

        KiB

    - **M**

        MiB

- **-perm**

    - Octal form (ex:644)

    - Symbolic form (ex:u=r)

    [Short Reference](https://www.oreilly.com/library/view/linux-pocket-guide/9780596806347/re44.html)

    Find will return files with those perms exactly

    - **-**

        Will return files with at least the permissions you specify

    - **/**

        Will return files that return any of the permission you have set

- **time related**

    - **min**

        Minutes

    - **time**

        Days

    Prefixes are:

    - **a**

        Last accessed

    - **m**

        Last modified

    - **c**

        Last time its status was changed

    For numerical values, same rules as size are applied

- **>**

    Redirection operator to save search to a file

    - **2> /dev/null**

    Supresses the output of any possible errors to make the output more readable

    Note: (0 -> standard input 1 -> standard output; 2 -> standard error; /dev/null -> special device that discards everything written to it)

- **-exec**

    Executes a new command, useful for privilege escalation

## Contact
If you have any questions or remarks don't hesitate to reach out on discord to *therokdaba#9872*.

Go back to the [homepage](https://therokdaba.github.io/) of this website.