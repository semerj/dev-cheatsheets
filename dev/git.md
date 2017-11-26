# git

## Shortcuts

### [zsh integration](https://github.com/robbyrussell/oh-my-zsh/blob/master/plugins/git/git.plugin.zsh)

```sh
gst            status
ga             add
gcmsg          commit -m
gd             diff
gm             merge
gsta           stash
glo            log --oneline --decorate
gb  / gba      branch / branch -a
gco / gcb      checkout / checkout -b
gl  / ggpull   pull / pull origin $(git_current_branch)
gp  / ggpush   push / push origin $(git_current_branch)
gf  / gfo      fetch / fetch origin
gr  / gra      remote / remote add
```

### [vim integration](https://github.com/tpope/vim-fugitive)

```sh
:Git           run arbitrary commands
:Gwrite        `git add`, when called from a work tree file,
               `git checkout`, when called from the index or a blob in history.
:Gread         `git checkout` -- filename, that operates on buffer rather than filename.
               can use `u` to undo it
:Gstatus       bring up the output of git status with
                 - : add/reset a file's changes
                 p : add/reset --patch
:Gcommit       commit files
:Gmove         `git mv`
:Gremove       `git rm`
:Glog          loads all previous revisions of a file into the quickfix list
:Gbrowse       open the current file on GitHub, with optional line range
```

#### configs

* make `vim` default editor

    ```sh
    $ git config --global core.editor "vim"
    ```

* always rebase on `git pull`

    ```sh
    $ git config --global pull.rebase true
    ```

## commands

### `git commit`

* add files and create message

    ```sh
    $ git commit -a -m <message>
    ```

* fix commit message of most recent commit

    ```sh
    $ git commit --amend
    ```

### `git branch`

* show all branches

    ```sh
    $ git branch -a
    ```

* delete local branch

    ```sh
    $ git branch -d <branch>
    ```

* delete remote branch

    ```sh
    $ git push origin --delete <branch>
    ```

* view remote branches

    ```sh
    $ git branch -r
    ```

* rename the current branch to `<branch>`

    ```sh
    $ git branch -m <branch>
    ```

### `git log`

* shows the full diff of each commit

    ```sh
    $ git log -p
    ```

* multiple lines per version for file

    ```sh
    $ git log --oneline -- <filename>
    ```

* search for commits with a commit message that matches

    ```sh
    $ git log --grep="<pattern>"
    ```

* `--graph` draws text based graph; `—-decorate` adds the names of branches

    ```sh
    $ git log --graph --decorate --oneline
    ```

### `git show`

* shows changes in files

    ```sh
    $ git show <commit>
    ```

* `--format` shows additional metadata

    ```sh
    $ git show --format=raw <commit>
    ```

### `git checkout`

Checking out a file affects the current state of your project.

* check out a previous version of file

    ```sh
    $ git checkout <commit> <file>
    ```

* if you decide you don’t want to keep the old version, you can check out the most recent version

    ```sh
    $ git checkout HEAD <file>
    ```

### `git diff`

* list differences between current and historical version

    ```sh
    $ git diff <hash> -- <file>
    ```

* compare the working directory with local repository

    ```sh
    $ git diff HEAD <file>
    ```

* compare the working directory with index

    ```sh
    $ git diff <file>
    ```

* compare the index with local repository

    ```sh
    $ git diff --cached <file>
    ```

### `git fetch`

* fetched content is represented as a remote branch, no effect on local work; safe way to review commits before integrating them with your local repository.

    ```sh
    $ git fetch <remote>
    $ git fetch <remote> <branch>
    ```

### `git merge`

* undo merge before completed, e.g. to undo merge conflict

    ```sh
    $ git merge --abort
    ```

* undo merge after you've completed the merge

    ```sh
    $ git reset --hard <commit>
    ```

### `git rebase`

* use `rebase` (instead of `merge`) to create a linear history

    ```sh
    # before                 # after

          C---D feature                    C'--D' feature
         /                                /
    A---B---E---F master     A---B---E---F master

    $ git rebase master topic
    ```

* interactively remove, split, and alter existing commits

    ```sh
    $ git rebase -i <base>
    ```

### `git pull`

* `git fetch` + `git merge`

    ```sh
    $ git pull <remote>
    ```

* prevent unnecessary merge commits

    ```sh
    $ git pull --rebase <remote>
    ```

### `git revert`

* generate a new commit that undoes all of the changes introduced in `<commit>`, then apply it to the current branch

    ```sh
    $ git revert <commit>
    ```

### `git reset`

Undo local changes in the staging area (can only work backwards from the current commit). More dangerous way to undo changes than `git revert` -- it's permanent.

* unstages all files without overwriting any changes

    ```sh
    $ git reset
    ```

* unstages a file without overwriting any changes

    ```sh
    $ git reset <file>
    ```

* move current branch tip backward to <commit>, reset the staging area to match, but leave the working directory alone

    ```sh
    $ git reset <commit>
    ```

### `git cherry-pick`

* bring commit to current branch

    ```sh
    $ git cherry-pick <commit>

    $ git cherry-pick C D
    # before                 # after

          C---D feature            C---D feature
         /                        /
    A---B---E---F master     A---B---E---F---C'---D' master
    ```

### `git stash`

* store uncommited changes

    ```sh
    $ git stash

    $ git stash list
      stash@{0}: WIP on master: 049d078 added the index file
      stash@{1}: WIP on master: c264051 Revert "added file_size"
      stash@{2}: WIP on master: 21d80a5 added number to log

    $ git show stash@{0}
    ```

* reapply the one you just stashed

    ```sh
    $ git stash apply
    ```

* apply one of the older stashes

    ```sh
    $ git stash apply stash@{1}
    ```

* remove stash from stack

    ```sh
    $ git stash drop stash@{0}
    ```

### `git reflog`

* after rewriting history, the `reflog` contains information about the old state of branches and allows you to go back to that state if necessary

    ```sh
    $ git reflog
    ```

### `git remote`

* add remote repo

    ```sh
    $ git remote school me@berkeley.edu:~/my_repo
    ```

* show remote repositories

    ```sh
    $ git remote -v
    $ git remote show origin
    ```

### Preserve git history while copying directory from repo A to repo B

* Get files ready for merge

  ```sh
  $ git clone <git@github.com:username/repo-A.git> && cd repo-A
  $ git remote rm origin
  $ git filter-branch --subdirectory-filter <repo-A/directory_1> -- --all
  $ mkdir <directory_1>
  $ mv * <directory_1>
  $ git add .
  $ git commit
  ```

* Merge files into new repository

  ```sh
  $ git clone <git@github.com:username/repo-B.git>
  $ cd <repo-B>
  $ git remote add repo-A-branch </directory_1>
  $ git pull repo-A-branch master --allow-unrelated-histories
  $ git remote rm repo-A-branch
  ```
