### commands ###
1.  `date`
2.  `echo hello`
3.  `echo "hello world"`
4.   `echo hello\ world`
5.   `mkdir my photos` will produce 2 folders "my" and "photos"
6.   environment variables are set whenever shell is started e.g home directory 
7.   `echo $PATH` list of paths/directory that is searched through for the file/program to match the command that is run
8.   `which echo` which specific path/directory/location that the program/file resides/run. See also https://unix.stackexchange.com/questions/85249/why-not-use-which-what-to-use-then
9.   Windows have separate root for every partition and file systems e.g C:\ D:\ but linux and MAC OS has only 1
10.  Relative path = relative to current directory vs Absolute paths = full path to current location. `pwd` = print working directory. `cd` = change directory. `pwd` and `cd /home`
11.   `.` for current directory and `..` for parent directory
12.   `cd ./home` is to cd into the 'home' directory under the current directory
13.   `ls` to see all the files in the directory vs `ls ..` to list all the files/folder one level up of itself
14.   `cd /` to root and `cd ~` to go home directory
15.   at regina@DESKTOP-SD2C393:~/smu/haskell$   `cd ../mit\ missing\ sem/lect1/`
16.   `cd -` to go to previous directory
17.   `ls --help` for help. `...` means 1 or more while `[ ]` means optional so `[FILE]` means optional number of files.
18.   -letter = flag vs -digit = option
19.   `ls -l` to get permissions of groups. 
      1.    owner - owner group - everyone else
      2.    for files:
            - `d` indicates its a direcory
            - `r` indicates read 
            - `w` indicates write , edit , save
            - `x` indicates execute
      3.    for directories:
            - `r` indicates read: can see contents of directory ie list
            - `w` indicates write: can create, remove file, rename. With only `w` on files but not its directory: can only empty the file but not delete the file.
            - `x` indicates for search ie can enter the directories 
20. `mv` to rename OR move a file; 2 arguments (CurrentFileName NewFileName)
21. `cp` to copy a file cp CurrFile ../CopyFile (to get a copy in directory above)
22. `rm` to remove a file
23. `rmdir` to remove an empty directory
24. `rm -r` path to remove everything from that directory onwards
25. `mkdir "My Photos"`
26. `man ls` to get manual page for `ls --help` ; Use `q` to quit here
27. Ctrl `l` to clear the prompt
28. `< file > file` to wire the input and output channel
    1.  `echo hello > hello.txt` to get hello output file "hello.txt"
    2.  `cat hello.txt` will print contents into the terminal
    3.  `cat < hello.txt` to open the file and takes its content to cat ie to print the contents into the terminal
    4.  `cat < hello.txt > hello2.txt` to cast/print hello.txt contents into hello2.txt 
    5.  `cat < hello.txt >> hello2.txt` to append content ie not overwrite 
29. `|` to get prog on left to be input for prog on right i.e chaining
    1.  `tail -n1` to print the last line
    2.  `ls -l/ | tail -n1` give same result as `ls -l / | tail -n1 > ls.txt` follow by `cat ls.txt`
    3.  `curl --head --silent google.com | grep -i content-length | cut --delimiter=' ' -f2`  
30. root user = superuser. sudo = do as su = do as super user
31. core systems
    1.  `cd /sys` 
    2.  then `ls` to see core systems
    3.  `cd class`
    4.  Change brightness inside brightness file
        1.  `cat max_brightness`
        2.  `sudo echo 500 > brightness` will get permission denied as brightness file needs sudo access
        3.  `sudo su` to be root user first
        4.  `#` to run as root. `# echo 500 > brightness`
        5.  exit to get out from super user 
    5. tee command...alternative to step 4 change brightness. tee sends output to a separate file for later viewing
        1. `echo 1060 | sudo tee brightness` to get output of echo into sudo tee bcos run tee as root and have tee write into the brightness file
32. open contents of file to see in bash. 
    1.  `cd smu/mit\ missing\ sem/Lecture1-Shell`
    2.  `xdg-open Lecture1.md`
