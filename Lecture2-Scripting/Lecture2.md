### commands ###
1. Strings ' ' vs " "
   1. `foo=bar` NOT foo = bar
   2. `echo "$foo"` gives bar
   3. `echo '$foo'` gives $foo
   4. `echo "hello"` and `echo 'hello'` give the same output
   5. `echo "Value is $foo"` vs `echo 'Value is $foo'`
   
2. Reading from a file
   1.  `vim mcd.sh` to open the file
   2.  `source mcd.sh` to select the file to execute
   3.  `mcd test` to move into the test folder in current directory
   4.  `cd ..` to go back one directory up
   
3. Variable arguments 
   1.  `$0` - Name of the script
   2.  `$1` to `$9` - Arguments to the script. `$1` is the first argument and so on.
   3.  `$@` - All the arguments
   4.  `$#` - Number of arguments
   5.  `$?` - Return code/output of the previous command
       1.  `true` then `echo $?` gives 0
       2.  `false` then `echo $?` gives ` 
   6.  `$$` - Process identification number (PID) for the current script
   7.  `!!` - Entire last command, including arguments. A common pattern is to execute a command only for it to fail due to missing permissions; you can quickly re-execute the command with sudo by doing `sudo !!` minus `!!` then type the command again
   8.  `$_` - Last argument from the last command. If you are in an interactive shell, you can also quickly get this value by typing `Esc` followed by `.`

   
4. `&&` (and operator) and `||` (or operator)
   1. Commands can also be separated within the same line using a semicolon `;`
      1. `false || echo "Oops, fail"`  will give the second output if the first shortcircuit
        <!-- # Oops, fail -->

      2. `true || echo "Will not be printed"` . The first will execute so nothing printed.
        <!-- # -->

      3. `true && echo "Things went well"` The first and second will execute as the first is ok so second is printed.
        <!-- # Things went well -->

      4. `false && echo "Will not be printed"` . The first will fail so the second cannot execute and nothing is printed
        <!-- # -->

      5. `true ; echo "This will always run"` Concatenation so everything that runs will be executed. Note `true` though executed but prints nothing
        <!-- # This will always run -->

      6. `false ; echo "This will always run"` Concatenation so anything/everything that runs will be executed
        <!-- # This will always run -->
    
5.  1. Command Substitution: if you do `for file in $(ls)`, the shell will first call `ls` and then iterate over those values.
    2. Process Substitution: `<( CMD )` will execute `CMD` and place the output in a temporary file and substitute the `<()` with that file’s name. This is useful when commands expect values to be passed by file instead of by STDIN. For example, `diff <(ls foo) <(ls bar)` will show differences between files in dirs `foo` and `bar`.
    3. Command Substition example:
       1. `foo=$(pwd)`
       2. `echo $foo`
       3. `echo "We are in $(pwd)"` gives the same output as `echo "We are in  $foo"

6. #!/bin/bash
   1. `echo "Starting program at $(date)"` # Date will be substituted
   2. `echo "Running program $0 with $# arguments with pid $$"` pid = process id
   3. (didnt try) for file in "$@"; do
            grep foobar "$file" > /dev/null 2> /dev/null
            # When pattern is not found, grep has exit status 1
            # We redirect STDOUT and STDERR to a null register since we do not care about them
            if [[ $? -ne 0 ]]; then
                echo "File $file does not have any foobar, adding one"
                echo "# foobar" >> "$file"
            fi
      done
    4. `./example.sh mcd.sh script.py example.sh`
7. Shell *globbing*
   1. Wildcards
      1. `ls *.sh` to get all the files ending with .sh
      2. `ls project?` to get all the folders running from project0 to project9 BUT exclude project42
   2. Curly Braces {}. Whenever you have a common substring in a series of commands, you can use curly braces for bash to expand this automatically. This comes in very handy when moving or converting files.
      1. convert image.{png,jpg}
        # Will expand to
        convert image.png image.jpg

     2. cp /path/to/project/{foo,bar,baz}.sh /newpath
        # Will expand to
        cp /path/to/project/foo.sh /path/to/project/bar.sh /path/to/project/baz.sh /newpath

     3. # Globbing techniques can also be combined
        mv *{.py,.sh} folder
        # Will move all *.py and *.sh files

     4. mkdir foo bar
        # This creates files foo/a, foo/b, ... foo/h, bar/a, bar/b, ... bar/h
        touch {foo,bar}/{a..h}
        touch foo/x bar/y

     5. # Show differences between files in foo and bar
        diff <(ls foo) <(ls bar)
        # Outputs
        # < x
        # ---
        # > y
8. #! shebang. The kernel knows to execute this script with a python interpreter instead of a shell command because we included a shebang line at the top of the script. It is good practice to write shebang lines using the env command that will resolve to wherever the command lives in the system, increasing the portability of your scripts. To resolve the location, env will make use of the PATH environment variable we introduced in the first lecture. For this example the shebang line would look like #!/usr/bin/env python.
     1. python3 script.py a b c
     2. Some differences between shell functions and scripts that you should keep in mind are:
        1. Functions have to be in the same language as the shell, while scripts can be written in any language. This is why including a shebang for scripts is important.
        2. Functions are loaded once when their definition is read. Scripts are loaded every time they are executed. This makes functions slightly faster to load, but whenever you change them you will have to reload their definition.
        3. Functions are executed in the current shell environment whereas scripts execute in their own process. Thus, functions can modify environment variables, e.g. change your current directory, whereas scripts can’t. Scripts will be passed by value environment variables that have been exported using export
        4. As with any programming language, functions are a powerful construct to achieve modularity, code reuse, and clarity of shell code. Often shell scripts will include their own function definitions.
9. help with `:help` and `?`
     1.  `tldr` installed has good helpful presentation 
     2.  `tldr ffmpeg` help on ffmepg
     3.  `tldr tar` for combining
10. Finding/Searching codes and/do some action. NB: use `-iname` in place of `-name` if you want the pattern matching to be case insensitive. `fd` = `find`. 
     1. # Find all directories named src
        `find . -name src -type d`
     2.  # Find all python files that have a folder named test in their path
        `find . -path '*/test/*.py' -type f`
     3. # Find all files modified in the last day
        `find . -mtime -1`
     4. # Find all zip files with size in range 500k to 10M
        `find . -size +500k -size -10M -name '*.tar.gz'`
     5. # Find all .py
        `fd ".*py"`
     6. # Delete all files with .tmp extension
        `find . -name '*.tmp' -exec rm {} \;`
     7. # Find all PNG files and convert them to JPG
        `find . -name '*.png' -exec convert {} {}.jpg \;`
     8. # Find text in file
        `grep foobar mcd.sh`
     9. # Find text recursively in the entire directory
        `grep -R foobar`
    10. # `-v` for inverting the match, i.e. print all lines that do not match the pattern. 
        1. `grep -R -v foobar`
        2. rg -u --files-without-match "^#\!" -t sh   (search in .sh files)  
    11. compiling some sort of index or database for quickly searching. That is what `locate` is for. `locate` uses a database that is updated using `updatedb` e.g.   
        1. `locate` missing-semester
    12. ripgrep (rg) is fast and intuitive
        1.  # Find text in all file of certain type and list them
        `rg "import requests" -t py ~/scratch`
        2.  # Find text in all file of certain type and list them with 5 lines of context before and after
        `rg "import requests" -t py -C 5 ~/scratch`
        3.  # Find all python files where I used the requests library
        `rg -t py 'import requests'`
        4.  # Find all files (including hidden files) without a shebang line
        `rg -u --files-without-match "^#!"`
        5.  # Find all matches of foo and print the following 5 lines
        `rg foo -A 5`
        6.  # Print statistics of matches (# of matched lines and files )
        `rg --stats PATTERN`
        7.  # Find text in all file of certain type and list them with 5 lines of context before and after and the stats of search
        `rg "import requests" -t py -C 5 -- stats ~/scratch`
    13. Find shell history for previous commands
        1.  `history`
        2.  `history 1` to print from beginning of time
        3.  `history | grep convert` to print commands with `convert`
    14. Interactive search
        1.  `cat example.sh | fzf`
        2.  `desired string to search for`
    
11. Directory search/display
    1.  `tree` to display directory and files hierachy
    2.  `broot` display summarized hierachy and interactive search
    3.  `nnn` display summary hierachy which u can enter to see the contents in each directory
