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
10.