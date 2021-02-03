1.  `|` piping to chain
2.  `less` is a local pager - to fit content into terminal to allow scroll up and down
3.  `> {file}` to channel output into the file 
4.  <Ctrl u> for move up
5.  <Ctrl d> for move down
6.  `sed` to make changes to contents of stream. It is a programming language over the given stream  ~ "replace"
    1. in the log stream: `sed 's/. *Disconnected from //'`  to remove everything before "Disconnected from" so `sed` takes 2 arguments ( i) the search string and ii) what to replace it with ) --- here , it is blank
      1. `.*` is a regex . `.` is any character. `*` is 0 or more char. `.*` means 0 or more characters so `.*` means 0 or more character following by the string "Disconnected from"
      2. `+*` is 1 or more character, ie match at least one
7.  `echo 'aba' | sed 's/[ab]//'`  sed will replace the string 'aba' with 0 or more of char in [ab]. Output is ba
    1. `echo 'bba' | sed 's/[ab]//'`  sed will replace the string 'aba' with 0 or more of char in [ab]. Output is ba. The output is same bcos sed will match the pattern once and replace the pattern once
8.  `echo 'bba' | sed 's/[ab]//g'` `g` is a modifier. Do this as many times as it keep matching 'a' or 'b' so output is blank.
9.  so `echo 'bcbzac' | sed 's/[ab]//g'` yields 'czc'
10. `echo 'abcaba' | sed -E 's/(ab)*//g'` yields 'ca' . 'a' or 'b' will be untouched. Only 'ab' will be replaced with nothing.
11. so `echo 'abcababc' | sed -E 's/(ab|bc)*//g'` yields 'cc'
12. `head -n5` is output the first 5 lines
13. `echo 'Disconnected from invalid user Disconnected from 84.211' | sed 's/.*Disconnected from //' yields 84.211 bcos regex like '.*' is greedy    and remove things in betweeen.
14. With a stream of "Disconnected from user Jon 24.61.9.143 port 51089" and "Disconnected from user invalid user admin 58.120.227.29 port 52978 [preauth]", "Disconnected from authenticating user root 138.68.10 port 5824 [preauth]....etc in prompt . 
        1. Using  `sed -E 's/^.*?Disconnected from (invalid\authenticating )?user (.*) [0-9.]+ port [0-9]+( \[preauth\]$/\2/' | wc -l` 
    1.  `-E` to avoid all the \
    2.  `*?` to make it a non-greedy match so it will stop at the first "Disconnected from" instead of matching as many as possible.
    3.  `(invalid|authenticating )?` for 0 or 1 of "invalid " or "authenticating "
    4.  `user .*` for some user name
    5.  `[0-9.]` for IP address part
    6.  `+` for many of those IP address
    7.  `port` as every line has that word
    8.  `[0-9]+` for many numbers
    9.  Anchors
        1.  `^` carrot for beginning of line
        2.  `$` for end of line
        3.  Goal: the expression must match the complete line 
    10. `\2` replace with the second captured group values. For above, it is the user group.
    11. IMPT: `( )` are captured group to group the expression part in `( )` for re-use later. The ( ) dont really do anything
    12. AVAILABLE: regex debugger online to provide breakdown/explanation for the expression
    13. `wc -l` to count the number of lines
    14. To get most frequent users into a line `sed -E 's/^.*?Disconnected from (invalid\authenticating )?user (.*) [0-9.]+ port [0-9]+( \[preauth\]$/\2/'| sort | uniq -c | sort -nk1,1 | tail -n10 | awk '{print $2}' | paste -sd, `
        1.  `| sort | uniq -c ` to sort output and print out only the unique and count the number of duplicates and summarize them
        2.  `| sort -nk1,1`    
            1.  `-n` is for numeric sort
            2.  `-k1,1` to select a whitespace separated column from the input to sort by. `1,1` for start and also stop at the first column
            3.  `tail -n10` to select last 10 lines as the highest are at the tail
        3. `awk` columnary data editor language to parse input in column. so here print the 2nd column
        4. `paste -sd,`  to take a bunch of lines into a single line with the delimiter ","
    15. To get all the list of users name that appear once and start with a "c" and end with an "e" and print the whole line : `sed -E 's/^.*?Disconnected from (invalid\authenticating )?user (.*) [0-9.]+ port [0-9]+( \[preauth\]$/\2/'| sort | uniq -c | awk '$1 == 1 && $2 ~ /^c.*e$/ {print $0}'
    16. Print the count of #15 `sed -E 's/^.*Disconnected from (invalid |authenticating )?user (.*) [0-9.]+ port [0-9]+ ( \[preauth\])?$/\2/'| sort| uniq -c |awk 'BEGIN { rows = 0} $1 == 1 && $2 ~ /^c. *e$/ {rows += 1} END {print rows }'`
        1.  On the zeroth line, set the row to 0
        2.  On every line that matches "$1 == 1 && $2 ~ /^c. *e$/", increment rows
        3.  After matching the last line , print row
15. bc is a calculator `echo "1+2" | bc -l` yields 3
16. Sum of user name which has been used not only once `sed -E 's/^.*Disconnected from (invalid |authenticating )?user (.*) [0-9.]+ port [0-9]+ ( \[preauth\])?$/\2/'| sort| uniq -c |awk '$1 !=1 {print $1}' | paste -sd+| bc -l`
    1.  `sed -E 's/^.*Disconnected from (invalid |authenticating )?user (.*) [0-9.]+ port [0-9]+ ( \[preauth\])?$/\2/'| sort| uniq -c |awk '$1 !=1 {print $1}' ` to list all the users not used only once. so output is a list of numbers
    2.  `paste -sd+` to paste the output into a continuous formula of x + y + z + etc
    3.  `bc -l` sum/calculate the above
17. Print statistics of min, max, quartile of distribution:
    `sed -E 's/^.*Disconnected from (invalid |authenticating )?user (.*) [0-9.]+ port [0-9]+ ( \[preauth\])?$/\2/'| sort| uniq -c |awk '$1 !=1 {print $1}' | R --slave -e 'x <- scan (file="stdin", quiet = TRUE); summary (x)'`
18. Plot histogram of top 5 user since Jan 1:
    `sed -E 's/^.*Disconnected from (invalid |authenticating )?user (.*) [0-9.]+ port [0-9]+ ( \[preauth\])?$/\2/'| sort| uniq -c | sort -nk1,1 | nail -n5 | gnuplot -p -e 'set boxwidth 0.5; plot "-" using 1:xtic(2) with boxes'`
19. Command Line Data Wrangling
    1.  `xargs` takes lines of input and turn them into arguments
    2.  `rustup toolchain list` rust lets u install multiple version of compilers showing the list of version installed
    3.  `rustup toolchain list | grep nightly | grep -v 'nighty-x86' | grep 2019` list the installation of compilers done nightly with x 86 in 2019
    4.  to uninstall then do `rustup toolchain uninstall specific-name-of-version-individually-step-by-step` Or more efficiently with `rustup toolchain list | grep nightly | grep -v 'nighty-x86' | grep 2019`| sed 's/-x86.*//' | xargs rustup toolchain uninstall` to take the list of inputs and turns them into arguments so it will do "rustup toolchain uninstall" each version one by one.
    5.  `ffmpeg -loglevel panic -i /dev/video0 -frames 1 -f image2 -| convert - -colorspace gray - | gzip | ssh tsp 'gzip -d |tee copy.png' | feh -`
        1.  `ffmpeg` encode and decode images and some video
        2.  `-loglevel panic` bcos many 
        3.  `/dev/video0` webcam device name
        4.  `-frames 1` take the first frame ie picture
        5.  `-f image2` take image rather than a single frame video file
        6.  `-` in this case (image to) standard output
        7.  `convert - -colorspace gray ` to read from standard input and turn image into grey colorspace and write the resulting image into a standard output 
        8.  `gzip` to pipe and compress the image file
        9.  `ssh tsp 'gzip -d |tee copy.png'`  send to remote server to decode the image and store a copy [remember that tee does read input, print stdout into a file ]
        10. `feh -` display on a image displayer
        11. check server has the image copy with `scp tsp:copy.png .`
        12. check decompress image on local `feh copy.png`