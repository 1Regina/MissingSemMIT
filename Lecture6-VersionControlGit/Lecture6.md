### Version Control Git ### 
Ref: https://www.atlassian.com/git/tutorials/using-branches/git-checkout#:~:text=The%20git%20checkout%20command%20lets,new%20commits%20on%20that%20branch. 
https://www.atlassian.com/git/tutorials/merging-vs-rebasing 

1. Include authors of file changes, extra msg, timestamps. Good for tracking changes, resolve conflict bcos its snapshot model.
2. Allow binary search to see when code was intro that caused a test to break
3. In Git: blob = file ; tree = directory
4. ```
   <root> (tree)
    |
    +- foo (tree)
    |  |
    |  + bar.txt (blob, contents = "hello world")
    |
    +- baz.txt (blob, contents = "git is wonderful")
    ```
5. Each snapshot is produced by git commit.
6. Git allow different development process to take different branches and eventually merge back as 1 wholesome complete version.
7.  ```
    // a file is a bunch of bytes
    type blob = array<byte>

    // a directory contains named files and directories
    type tree = map<string, tree | blob>

    // a commit has parents, metadata, and the top-level tree
    type commit = struct {
        parent: array<commit>
        author: string
        message: string
        snapshot: tree
    }
    ```
8. An “object” is a blob, tree, or commit:
    ```
        type object = blob | tree | commit
    ```
9.  In Git data store, all objects are content-addressed by their SHA-1 hash (a series of alphanumerics ID).
    ```
        objects = map<string, object>

        def store(object):
            id = sha1(object)
            objects[id] = object

        def load(id):
            return objects[id]
    ```
    1. git cat-file -p 698281bc680d1995c5f4caaf3359721a5a58d48d), a tree containing pointer to its contents ( baz.txt (a blob) and foo (a tree)) looks like this:
    ```
    100644 blob 4448adbf7ecd394f42ae135bbeed9676e894af85    baz.txt
    040000 tree c68d233a33c5c06e0340e4c224f0afca87c8ce87    foo
    ```
    and look at the contents with git cat-file -p 4448adbf7ecd394f42ae135bbeed9676e894af85

10. In Git, that “where we currently are” is a special reference called “HEAD”.
11. ``` git checkout master; git reset --hard 5d83f9e``` “discard uncommitted changes and make the ‘master’ ref point to commit 5d83f9e”, there’s   probably a command to do it (e.g. in this case, git checkout master; git reset --hard 5d83f9e).
12. Staging area allows you to select which feature/ change to implement to the next snapshot bcos you can choose which modification to add.
13. ### Git commands ###
    #### Basics ####
* ```git help <command>``` : get help for a git command
* ```git init``` : creates a new git repo, with data stored in the .git directory
* ```git status``` : tells you what’s going on
* ```git add <filename>``` : adds files to staging area
* ```git commit``` : creates a new commit
    * Write [good commit messages](https://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html)!
    * Even more reasons to write [good commit messages](https://chris.beams.io/posts/git-commit/)!
* ```git log``` : shows a flattened log of history
* ```git log --all --graph --decorate``` : visualizes history as a DAG
* ```git diff <filename>``` : show changes you made relative to the staging area
* ```git diff <revision> <filename>``` : shows differences in a file between snapshots
* ```git checkout <revision>``` : updates HEAD and current branch
#### Branching and merging ####
* ```git branch``` : shows branches
* ```git branch <name>``` : creates a branch
* ```git checkout -b <name>``` : creates a branch and switches to it
    * same as ```git branch <name>; git checkout <name>```
* ```git merge <revision>``` : merges into current branch
* ```git mergetool``` : use a fancy tool to help resolve merge conflicts
* ```git rebase``` : rebase set of patches onto a new base
#### Remotes ####
* ```git remote``` : list remotes
* ```git remote add <name> <url>``` : add a remote
* ```git push <remote> <local branch>:<remote branch>``` : send objects to remote, and update remote reference
* ```git branch --set-upstream-to=<remote>/<remote branch>``` : set up correspondence between local and remote branch
* ```git fetch``` : retrieve objects/references from a remote
* ```git pull``` : same as git fetch; git merge
* ```git clone``` : download repository from remote
#### Undo ####
* ```git commit --amend``` : edit a commit’s contents/message
* ```git reset HEAD <file>``` : unstage a file
* ```git checkout -- <file>``` : discard changes
14. ### Advanced Git ###
* ```git config``` : Git is [highly customizable](https://git-scm.com/docs/git-config)
* ```git clone --depth=1``` : shallow clone, without entire version history
* ```git add -p``` : interactive staging
* ```git rebase -i``` : interactive rebasing
* ```git blame``` : show who last edited which line
* ```git stash``` : temporarily remove modifications to working directory
* ```git bisect``` : binary search history (e.g. for regressions)
* ```.gitignore``` : [specify](https://git-scm.com/docs/gitignore) intentionally untracked files to ignore
15. ### Miscellaneous ###
* **GUIs** : there are many [GUI clients](https://git-scm.com/downloads/guis) out there for Git. We personally don’t use them and use the command-line interface instead.
* **Shell integration** : it’s super handy to have a Git status as part of your shell prompt ([zsh](https://github.com/olivierverdier/zsh-git-prompt), [bash](https://github.com/magicmonty/bash-git-prompt)). Often included in frameworks like [Oh My Zsh](https://github.com/ohmyzsh/ohmyzsh).
* **Editor integration** : similarly to the above, handy integrations with many features. [fugitive.vim](https://github.com/tpope/vim-fugitive) is the standard one for Vim.
* **Workflows** : we taught you the data model, plus some basic commands; we didn’t tell you what practices to follow when working on big projects (and there are [many](https://nvie.com/posts/a-successful-git-branching-model/) [different](https://www.endoflineblog.com/gitflow-considered-harmful) [approaches](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)).
* **GitHub** : Git is not GitHub. GitHub has a specific way of contributing code to other projects, called [pull requests](https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests/about-pull-requests).
* **Other Git providers** : GitHub is not special: there are many Git repository hosts, like [GitLab](https://about.gitlab.com/) and [BitBucket](https://bitbucket.org/).
16. ### Resources ###
* [Pro Git](https://git-scm.com/book/en/v2) is **highly recommended** reading. Going through Chapters 1–5 should teach you most of what you need to use Git proficiently, now that you understand the data model. The later chapters have some interesting, advanced material.
* [Oh Shit, Git!?!](https://ohshitgit.com/) is a short guide on how to recover from some common Git mistakes.
* [Git for Computer Scientists](https://eagain.net/articles/git-for-computer-scientists/) is a short explanation of Git’s data model, with less pseudocode and more fancy diagrams than these lecture notes.
* [Git from the Bottom Up](https://jwiegley.github.io/git-from-the-bottom-up/) is a detailed explanation of Git’s implementation details beyond just the data model, for the curious.
* [How to explain git in simple words](https://xosh.org/explain-git-in-simple-words/)
* [Learn Git Branching](https://learngitbranching.js.org/) is a browser-based game that teaches you Git.