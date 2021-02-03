1.  Mode changes
    1. change modes by pressing <ESC> (the escape key) to switch from any mode back to Normal mode. 
2.  Keys - from Normal mode, enter 
    1. Insert mode with `i`, 
    2. Replace mode with `R`, 
    3. Visual mode with `v`, 
    4. Visual Line mode with `V`, 
    5. Visual Block mode with `<C-v>` (Ctrl-V, sometimes also written ^V), and 
    6. Command-line mode with `:` to open shell to do `:quit`.
3.  Enter by `vim` or open file in vim by `vim filename.md` e.g `vim editors.md`
4.  Exit by `Ctrl z`
5.  So to edit and save
    1.  Go normal mode with `i` and type type type
    2.  Press <Esc> to go to Normal mode
    3.  `:` to go into Command mode
    4.  `:w` to save ie write
    5.  <Enter>
    6.  `:q` to close current window aka quit and <Enter>
    7.  To check, do `vim filename`
6.  `:help` command e.g `:help :w` is for w in Command mode vs `:help w` where w for Normal Mode `w` which is count words forward
7.  Multiple window
    1.  Open 2 windows views on a single file `:sp` 
    2.  `:q` to quit window. 
    3.  `:q` a few times to get back to shell or `:qa`
    4.  <Ctrl W> to switch between windows
8.  Normal Mode. Vim interface is a programming language!
9.  Directions 
    1.  `j` for down
    2.  `k` for up
    3.  `h` for left
    4.  `l` for right
    5.  `w` to move fwd by one word
    6.  `b` to move backward by one word
    7.  `e` to move to end of the word
    8.  `0` to move to bgn of line
    9.  `$` to move to end of line
    10. `^` to move to first non-empty character on line
    11. <Ctrl U> for up a page
    12. <Ctrl D> for down a page
    13. `G` for all the way down
    14. `gg` for all the way up
    15. `L` for lowest line on screen
    16. `M` for middle line on screen
    17. `H` for highest line on screen
10. Find/ search and edit
    1.  `fo` find the first word starting with o
    2.  `Fo` find backwards the first word starting with o
    3.  `To` jump to next letter just before o
    4.  `to` jump backwards to letter just after o
    5.  `o` from normal mode will jump to insert mode and open a new line below the cursor. Esc to return to Normal
    6.  `O` from normal mode into insert mode and open a new line above the cursor. Esc to return to Normal
    7.  `d` with `w` to delete a word
    8.  `u` for undo in Normal mode. ie changes in Insert Mode -> Normal -> `u` will undo all the changes in Insert Mode. But if merely `x` to delete a character; `u` will undo that deletion and put back the character.
    9.  <Ctrl R> for redo.
    10. `d` with `e` to delete the end of word if cursor is in middle
    11. `c` with `e` to change the end of word if cursor is in middle and goes into Insert mode to continue typing. Esc to return to Normal
    12. `dd` delete a line
    13. `cc` delete a line but goes into Insert mode so can replace it with new typing
    14. `x` delete a character
    15. `r` with `character` replace the specific letter with the `character`
    16. `~` to change upper to lowercase and vice versa after selection  
    17. `/` text to search text interactively to first instance of text
    18. `n` to go to the next match
    19. `.` repeats the previous editing command
11. COPY AND PASTE:
    1.  `yy` then `p` will copy and paste the line
    2.  `yw` then `p` will copy and paste the word
    3.  blocks by 
        1.  `v` to go into Visual mode 
        2.  then `j` / `l` / `k` / `h` for direction or `w` for word 
        3.  then `y` to copy (which would then go into Normal mode)
        4.  then `j` / `l` / `k` / `h` for direction to get to the desired location
        5.  then `p` to paste
        6. Visual lines by 
           1. `V`
           2. then `j` / `k` to select lines up or down
           3. OR by selected area block by with directional keys `j` / `l` / `k` / `h`
12. Counts:
    1.  `4j` to use count and move down 4 times/lines
    2.  `v` Visual Mode then `3e` to select 3 ends of words
    3.  `v` Visual Mode then `7dw` to delete 7 words
    4.  `8j` in Normal Mode to move down 8 lines

13.  Modifiers:
    5.  Method 1: 
         1.  `b` move back
         2.  `2dw` to delete 2 words
         3.  `i`  to insert then type in desired text
    6.  Method 2:
         1.  `c2w` to change 2 words in Insert Mode then type in desired text
    7.  Method 3 Modifier way when in [ ] or ( ):
         1.  `ci[` to change inside bracket and go into Insert Mode
         2.  `%` to jump back and fro between matching ( ) or [ ]
         3.  `di(` to delete contents inside the ( ) in Normal Mode . ( ) is retained
         4.  `da(` delete around/including in ( ) in Normal Mode and ( ) is also deleted
         5.  summary `cw` = `dwi` but saved effort on a `i`
14. Other notes:
    1.  `:e {name of file}` open file for editing
    2.  `:ls` show open buffers
    3.  `:help {topic}` open help