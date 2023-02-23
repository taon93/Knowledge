Unix is the family of operating systems, operating system is an abstraction between higher level software and hardware of the computer.

## File systems: 
Operating systems usually standardize its file systems: Windows uses NTFS file system format. IOS uses APFS, Ext4 used by Linux
1. Difference between Windows and Unix filesystems: 
   2. Windows have drives: each drive represents one partition (part of the hard drive formatted in 
   a particular file system format) or other kind of access memory like: external drives, CD_ROMS etc. Each drive is on the same level with respect to each other.
   3. Linux/Unix has one main `/root` directory - which stores other directories, those directories may be responsible for different things in an operating system
      (in linux and unix everything is a file or a directory)
   
### File system structure: 
`/root` - main directory of the Unix os. `cd /` will take you to the `root` dir. 
    `/bin` - binary executables — essential programs that are accessible for every User of the os. 
    `/dev` - Devices - devices that os can see. Maps hardware devices to file. 
    `/etc` - System wide configuration files.
    `/home` - stores all directories of the users of this os, is mapped to `~` dir, and can be accessed via `cd ` or `cd ~`
        `/username` - directory inside `home` with files and directories assigned to `username` user.
    `/lib` - libraries - code libraries, 3rd party libraries (usually C/C++ centric). They have `.so` extension, which stands for a standard library. 
    `/sbin` - binary executables but restricted only to use it by administrator of the os. 
    `/usr` - binary executables that are "extras" — programs nice to have but not essential to the os. 
    `/var` - temporary files that are used by the system or other programs — like for example logs. 
----
`.` - current directory — you may think of it like `this` operator in programming, it is usually used to run programs that are not in the `PATH`
they are not in any secified directory with executables that system is aware of. To run program from current directory, you use: `./<program-name>`
(without `/` there would be ambiguity - shell would not know if we want to run program or we ara specifying program that is hidden).
If you want to run program, that is in the parent directory use: `../<program-name>`. 
To run program from any place of the file system, use its absolute path: `/home/maciek/<program-name>` (without any dots)
> you may use `cd -` to get to previous directory. 
----
## Command line shortcuts: Everything 
Divide commands in one line by `;`

### REDIRECTION
`>` replaces
`>>` appends
> This operators have options for file descriptors: STDIN, STDOUT and STDERR:
> - 0 is STDIN
> - 1 is STDOUT
> - 2 is STDERR
> If you want to redirect error messages to the file (replacing internals of the file) use: `<command + options + arguments> 2> <file>`

If you just want to get rid of the output you are redirecting, redirect it to `/dev/null`    
You may join redirects ie: redirect STDERR to one file and STDOUT to another: `<command +options +arguments> 2> <file1> > <file2>`
### with **CTRL**
- `l` - clear whole terminal window (like `clear` command)
- `a` - go to a beginning of the line
- `b` - backward one character
- `f` - forward one character
- `e` - move to the end of the line
- `w` - remove last word before the cursor
- `h` - erase one character (like backspace)
- `u` - erase whole line — before the cursor
- `k` - erase whole line — after the cursor
- `z` - cancel current operation
### with **ALT**
- `f` - move by one word forward
- `b` - move by one word backward


## Commonly used programs:
1. `cp` : copy, usage `cp <inputDirectory>/<inputFile> <outputDirectory>/<outputFile>` copies file from input to output. InputDirectory, OutputDirectory and OutputFile 
are optional. — you don't need to specify inputAndOutput directory if you want them to be current directory if not specified OutputFile do have same name like InputFile (then it must be in another directory)
By default `cp` will not copy directory, to make deep copy use switch `-r`. If you want to copy multiple files and/or directories, you must use: `cp <inputFile1> <inputFile2> <inputDir1>/<inputFile3> <inputDir2> -r <outputDir>`
2. `mkdir` `-p` switch: makes whole structure of directories: by default `mkdir` will make only one layer deep directory, if you want to make deeper structure use this switch. 
3. `chmod`: `a` - all, `u` - user, `g` - group, `o`- others: `chmod u+x jerry.txt` - you can join permissions with `,`: `chmod u+x,g-r jerry.txt`. To set exact permissions you may use `=`: `chmod u=rwx,g=rw,o=x jerry1.txt` will set this file permissions to: 
`wrxwr---x`. You may use chmod recursively with `-r` switch 
4. `ln` - creates a link to the file: `-s` symbolic link: it is preferred: to use soft links. Hard links have more restrictions. `ln -s <inputDir>/<dir or file to be linked> <outputDir>/<outputLink>` 
5. `man` - manual, page down: `space` , page up: `b`. `enter` - go one line at the time back, `/` search
6. `apropos` - search unix command by the keyword, you may filter by sections (more on sections on `man man` man page), you usually want to include only section 1 commands: `apropos -s 1 <keyword>` you may join keywords with `-a`
`apropos -s 1 <keyword1> -a <keyword2>`. Synonym for this command is `man -k -s 1 <keyword1> -a <keyword2>`
7. `wahtis` - command for getting quick information about command: `whatis <keyword>`
8. `whereis` - where is installed given command
9. `which` - where is location of the program that will be run if you use searched command. 
10. `export` - exports variable as an environment variable, `-n` - removes variable from environment variables
11. `head` & `tail` - head shows firs couple of lines of the file, tail shows last few. `-f` in `tail` lets you observe changes to the end of the file in real time: Useful for Logs or errors. 
12. `wc` - counts number of new lines, words and bytes
13. `sort` - sorts input (by default in alphabetical order: 10 will come before 1 and before 2). You may use switches to change that
14. `grep` - finds given pattern in the input: `grep <pattern< <input>`, `-i` - case insensitive, `-p` - **IMPORTANT** - treats pattern as a Pearl regex flavour pattern. `-v` invert search, '-c' - count number of matches.
15. `seq` - generates sequence: `seq <startingNumber> <endingNumber>` 
16. `xargs` - takes input, divides it by whitespaces and invokes argument procedure on each of the elements, usually used with pipelines

## Environment variables: 
1. `$SHELL` - what shell is currently used (location)
2. `$PATH` - All locations searched vor environment variables, and order of search, if found, rest of the directories won't be searched.
## File permissions: 
The system assigns default permissions to the files created by users. The creator of the file is an owner; there are 3 groups of users that may have permissions 
related to the file/directory - owners, assigned groups and others (everyone else). 

## File globes: 
They let you specify a group of files that share some common things in the name: they are used like regex. 

## Shell scripting: 
1. $arg vs "$arg" - in $arg every word is treated as a separate string (like an array of strings), in "$arg" whole phrase is treated as one string. 
2. `./<script>` vs `source <script>` - When `./` is used program is run in separate child shell, and output is sent to the parent (shell that invoked the script), when a script is "sourced" — it is executed in current shell, so it may be affected by it.  

## Job and Process control
- Jobs are managed by the shell, processes are managed by the OS. Jobs hava job IDs and processes have process ID. Jobs may be in the foreground or in the background.
- `CTRL + z` - stops current program without killing it - moves it to the background. 
- `jobs` - lists currently running jobs (postponed by `CTRL+z`) - gives every program number, to use with a combination with `fg`, `+` in the prompt of the list indicates last used program. 
- `fg` - foreground: brings program from the background to the foreground: without arguments last used program will be chosen. As argument, you may use number (got from the `list`)  
- `CTRL + c` - stops current program entirely
- `watch` - rerun given program every time interval (2s by default): `watch date`, `-n` switch lets you specify time interval: `watch date -n 1` (in seconds)
- To run program in the background append `&` to the program name: `watch date&`. 
- `ps` - what processes run currently on **this shell**
- `kill` - `-l` - lists kill options (signal names), to kill the process use: `kill <signalName> <processID>` default `<signalName>` is `SIGKILL`
### Redirection and Piping