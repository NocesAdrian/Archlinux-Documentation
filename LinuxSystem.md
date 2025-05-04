## Basic CMD
```bash
NAME  
    Basic Linux Commands and Filters - Introduction to essential shell commands and text filters.

SYNOPSIS  
    command [OPTIONS] [ARGUMENTS]

DESCRIPTION  
    This manual introduces core Linux commands and filters used in shell scripting and day-to-day terminal work. These tools form the backbone of automation, file manipulation, and data parsing.

COMMANDS  

    rm - remove files or directories  
        SYNOPSIS:  
            rm [OPTION]... FILE...  
        DESCRIPTION:  
            Deletes files or directories permanently.  
        OPTIONS:  
            -r  Recursively remove directories  
            -f  Ignore nonexistent files and never prompt  
        EXAMPLES:  
            rm file.txt  
            rm -rf folder/

    mkdir - make directories  
        SYNOPSIS:  
            mkdir [OPTION]... DIRECTORY...  
        DESCRIPTION:  
            Creates directories.  
        OPTIONS:  
            -p  Create parent directories as needed  
        EXAMPLES:  
            mkdir new_folder  
            mkdir -p path/to/dir

    ls - list directory contents  
        SYNOPSIS:  
            ls [OPTION]... [FILE]...  
        DESCRIPTION:  
            Lists files and directories.  
        OPTIONS:  
            -l  Long listing format  
            -a  Include hidden files  
            -h  Human-readable sizes  
        EXAMPLES:  
            ls -la  
            ls /home/user

    sudo - execute a command as another user  
        SYNOPSIS:  
            sudo COMMAND  
        DESCRIPTION:  
            Runs a command with elevated privileges.  
        EXAMPLES:  
            sudo apt update  
            sudo rm -rf /opt/test

    cp - copy files and directories  
        SYNOPSIS:  
            cp [OPTION]... SOURCE DEST  
        DESCRIPTION:  
            Copies files or directories.  
        OPTIONS:  
            -r  Copy directories recursively  
        EXAMPLES:  
            cp file.txt /backup/  
            cp -r dir1 dir2

    mv - move (rename) files  
        SYNOPSIS:  
            mv [OPTION]... SOURCE DEST  
        DESCRIPTION:  
            Moves or renames files and directories.  
        EXAMPLES:  
            mv file.txt folder/  
            mv oldname.txt newname.txt

    touch - create empty files or update timestamps  
        SYNOPSIS:  
            touch FILE  
        DESCRIPTION:  
            Creates a new empty file or updates an existing file's timestamp.  
        EXAMPLES:  
            touch new.txt

TEXT FILTERS  

    grep - print lines matching a pattern  
        SYNOPSIS:  
            grep [OPTIONS] PATTERN [FILE]...  
        DESCRIPTION:  
            Searches for PATTERN in each FILE.  
        OPTIONS:  
            -i  Ignore case  
            -r  Recursively search directories  
            -n  Show line number  
        EXAMPLES:  
            grep "main" file.c  
            grep -rn "TODO" src/

    uniq - report or filter duplicate lines  
        SYNOPSIS:  
            uniq [OPTION]... [INPUT [OUTPUT]]  
        DESCRIPTION:  
            Removes or reports duplicate lines in sorted input.  
        OPTIONS:  
            -c  Count duplicates  
            -d  Only print duplicate lines  
        EXAMPLES:  
            sort file.txt | uniq  
            sort file.txt | uniq -c

    sort - sort lines of text files  
        SYNOPSIS:  
            sort [OPTION]... [FILE]...  
        DESCRIPTION:  
            Sorts lines in text files.  
        OPTIONS:  
            -r  Reverse order  
            -n  Numeric sort  
        EXAMPLES:  
            sort file.txt  
            sort -nr numbers.txt

    wc - count words, lines, and characters  
        SYNOPSIS:  
            wc [OPTION]... [FILE]...  
        DESCRIPTION:  
            Displays line, word, and byte counts.  
        OPTIONS:  
            -l  Line count  
            -w  Word count  
            -c  Byte count  
        EXAMPLES:  
            wc file.txt  
            wc -l *.c

    head - output the first part of files  
        SYNOPSIS:  
            head [OPTION]... [FILE]...  
        DESCRIPTION:  
            Prints the first 10 lines by default.  
        OPTIONS:  
            -n N  Show first N lines  
        EXAMPLES:  
            head -n 5 file.txt

    tail - output the last part of files  
        SYNOPSIS:  
            tail [OPTION]... [FILE]...  
        DESCRIPTION:  
            Prints the last 10 lines by default.  
        OPTIONS:  
            -f  Follow file updates (like logs)  
        EXAMPLES:  
            tail -f /var/log/syslog

    cut - remove sections from each line of files  
        SYNOPSIS:  
            cut OPTION... [FILE]...  
        DESCRIPTION:  
            Cuts parts of lines by delimiter or position.  
        OPTIONS:  
            -d  Delimiter  
            -f  Field number  
        EXAMPLES:  
            cut -d ':' -f1 /etc/passwd

    awk - pattern scanning and processing language  
        SYNOPSIS:  
            awk 'PROGRAM' FILE  
        DESCRIPTION:  
            Processes columns and rows, great for reports.  
        EXAMPLES:  
            awk '{print $1}' file.txt  
            awk -F ':' '{print $1, $3}' /etc/passwd

    xargs - build and execute command lines from input  
        SYNOPSIS:  
            xargs [OPTION]... COMMAND  
        DESCRIPTION:  
            Executes commands with input as arguments.  
        EXAMPLES:  
            echo file.txt | xargs rm  
            find . -name "*.tmp" | xargs rm
```
