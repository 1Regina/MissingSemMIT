    ### JOB CONTROL ###

1.  SIGINT` to interupt program; `SIGQUIT` to quit porgram; `SIGSTOP` to stop/pause the program; `SIGCONT` to continue the program; `SIGTERM` to terminate; `man signal` to get list manual of Signals
2.  If <Ctrl c> fail to work bcos there is a python file running that has disabled it, try `SIGQUIT` to terminate
3.  `SIGKILL` will terminate process no matter what.
4.  `&` at the end of command tells bash that the program is to be run in the background.
5.  `nohup sleep 2000 &` 
    1.  `nohup` no hangup -- to suspend job
6.  `jobs` to show the existing jobs
7.  `bg %1` to continue the first job. % is for job serial index. `bg` is for background.
8.  `kill STOP %1` to suspend first job . bad side effects such as leaving orphaned children processes.
9.  Example
10. $ sleep 1000
    ^Z
    [1]  + 18653 suspended  sleep 1000

    $ nohup sleep 2000 &
    [2] 18745
    appending output to nohup.out

    $ jobs
    [1]  + suspended  sleep 1000
    [2]  - running    nohup sleep 2000

    $ bg %1
    [1]  - 18653 continued  sleep 1000

    $ jobs
    [1]  - running    sleep 1000
    [2]  + running    nohup sleep 2000

    $ kill -STOP %1
    [1]  + 18653 suspended (signal)  sleep 1000

    $ jobs
    [1]  + suspended (signal)  sleep 1000
    [2]  - running    nohup sleep 2000

    $ kill -SIGHUP %1
    [1]  + 18653 hangup     sleep 1000

    $ jobs
    [2]  + running    nohup sleep 2000

    $ kill -SIGHUP %2

    $ jobs 
    [2]  + running    nohup sleep 2000
    # [2] still continue to run because signals are prevented from entering. But kill below again to send a kill signal `kill -KILL %2` will do the job

    $ kill %2
    [2]  + 18745 terminated  nohup sleep 2000

    $ jobs
11. `fg` for recover program back to foreground  
    
    ### TERIMAL MULTIPLEX ###
    tmux expects you to know its keybindings, and they all have the form <C-b> x where that means (1) press Ctrl+b, (2) release Ctrl+b, and then (3) press x. tmux has the following hierarchy of objects:

#### Sessions ####
- a session is an independent workspace with one or more windows
1.  `tmux` starts a new session.
2.  `tmux new -t NAME` starts it with that name.
3.  `tmux ls` lists the current sessions
4.  Within tmux typing <C-b> d detaches the current session
5.  `tmux a` attaches the last session. You can use -t flag to specify which ie `tmux a -t name`
6.  `tmux kill-session` to kill a session
7.  `tmux kill-session -t myname` to kill a specific session
8.  #### more on tmux cheatsheet https://gist.github.com/henrik/1967800 #### 
#### Windows ####
Note that the TA has config <C-a> for his local machine and <C-b> for his remote machine for tmux command. Here, these are based on his local machine. Typing twice will effect the remote.
- Equivalent to tabs in editors or browsers, they are visually separate parts of the same session
1.  <C-a> c Creates a new window. To close it you can just terminate the shells doing <C-d>
2.  <C-a> N Go to the N th window. Note they are numbered
3.  <C-a> p Goes to the previous window
4.  <C-a> n Goes to the next window
5.  <C-a> , Rename the current window
6.  <C-a> w List current windows
#### Panes #### 
- Like vim splits, panes let you have multiple shells in the same visual display.
1.  <C-a> " Split the current pane horizontally
2.  <C-a> % Split the current pane vertically
3.  <C-a> <direction> Move to the pane in the specified direction. Direction here means arrow keys.
4.  <C-a> z Toggle zoom for the current pane
5.  <C-a> [ Start scrollback. You can then press <space> to start a selection and <enter> to copy that selection.
6.  <C-a> <space> Cycle through pane arrangements.

### Alias ###
1.  alias alias_name="command_to_alias arg1 arg2"
    Note that there is no space around the equal sign =, because alias is a shell command that takes a single argument.
2.  Examples alias in https://missing.csail.mit.edu/2020/command-line/ but do not create!
3.  Aliases do not persist shell sessions by default. To make an alias persistent you need to include it in shell startup files, like .bashrc or .zshrc with `vim ~/.bashrc` 
   
### DotFiles ###
1.  dotfiles (because the file names begin with a ., e.g. ~/.vimrc, so that they are hidden in the directory listing ls by default)
2.  Some other examples of tools that can be configured through dotfiles are:
    1.    bash - ~/.bashrc, ~/.bash_profile
    2.    git - ~/.gitconfig
    3.    vim - ~/.vimrc and the ~/.vim folder
    4.    ssh - ~/.ssh/config
    5.    tmux - ~/.tmux.conf
3.  Popular resources references here : https://github.com/search?o=desc&q=dotfiles&s=stars&type=Repositories and https://github.com/mathiasbynens/dotfiles
4.   symlinked to organise the config file e.g. creating a dotfiles directory to contain the bashrc and vimrc so that when the .bashrc and .vimrc files in the home directory are accessed, it is writing from/using the ones in the dotfile direcotry. Command to edit is `ln -s path/.../bin symlink`
5.   Portability
     1.   Common pain with dotfiles is that the configurations might not work when working with several machines, e.g. if they have different operating systems or shells.  
     2. ``` 
    **if** [[ "$(uname)" == "Linux" ]]; **then** {do_something}; **fi**

    # Check before using shell-specific features
    **if** [[ "$SHELL" == "zsh" ]]; **then** {do_something}; **fi**

    # You can also make it machine-specific
    **if** [[ "$(hostname)" == "myServer" ]]; **then** {do_something}; **fi**
       ```
     3. If the configuration file supports it, make use of includes. For example, a ~/.gitconfig can have a setting:
       ```
       [include]
    path = ~/.gitconfig_local
       ```
     4. If you want different programs to share some configurations. For instance, if you want both bash and zsh to share the same set of aliases you can write them under .aliases and have the following block in both:
      ```
      # Test if ~/.aliases exists and source it
    **if** [ -f ~/.aliases ]; then
        source ~/.aliases
    **fi**

### Remote Machine ###
1.  `ssh foo@bar.mit.edu` to ssh as user foo in server bar.mit.edu or an IP (something like foobar@192.168.1.42)
2.  SSH key to prove to the server that the client owns the secret private key without revealing the key. private key (often ~/.ssh/id_rsa and more recently ~/.ssh/id_ed25519) is the password.
3.  Key generation of key pair : follow github instruction
4.  Authentication `ssh-copy-id -i .ssh/id_ed25519.pub foobar@remote`
5.  `logout` to logout
6.  can include a passphrase as double security for login
7.  To copy public key `ssh-copy-id -i .ssh/id_ed25519.pub foobar@remote`
8.  Copy files: 
    1.  `cat localfile | ssh remote_server tee serverfile`
    2.  `scp path/to/local_file remote_host:path/to/remote_fil`
    3.  `rsync path/to/local_file remote_host:path/to/remote_fil` is better than scp bcos it prevents duplication e.g when connection is restored after interruption and earlier copying was incomplete.
9.  Port forwarding: Local Port forwarding (most common): 
    1.  when a service in the remote machine listens in a port and you want to link a port in your local machine to forward to the remote port. For example, if we execute jupyter notebook in the remote server that listens to the port 8888. Thus, to forward that to the local port 9999, we would do ssh -L 9999:localhost:8888 foobar@remote_server and then navigate to locahost:9999 in our local machine.
10. SSH Configuration:
    ```
    Host vm
    User foobar
    HostName 172.16.174.141
    Port 2222
    IdentityFile ~/.ssh/id_ed25519
    LocalForward 9999 localhost:8888

    # Configs can also take wildcards
    Host *.mit.edu
        User foobaz
    ```
11. be thoughtful about sharing your SSH configuration bcos  ~/.ssh/config file is a dotfile.
12. sshfs can mount a folder on a remote server locally, and then you can use a local editor

### Shell ###
1. while shells like fish include many features like syntax highlight, theme, autocompletion etc., they may slow down the shell.
2. Aspects to consider for terminal setting
   1.  Font choice
   2.  Color Scheme
   3.  Keyboard shortcuts
   4.  Tab/Pane support
   5.  Scrollback configuration
   6.  Performance (some newer terminals like Alacritty or kitty offer GPU acceleration).