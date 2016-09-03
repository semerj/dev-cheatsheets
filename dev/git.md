# git

# Zsh integration
[Oh My Zsh git plugin](https://github.com/robbyrussell/oh-my-zsh/blob/master/plugins/git/git.plugin.zsh)

```sh
gst      status
ga       add
gcmsg    commit -m
gd       diff
gm       merge
gf       fetch
gfo      fetch origin
gr       remote
gra      remote add
gb       branch
gba      branch -a
gco      checkout
gcb      checkout -b
glo      log --oneline --decorate
gl       pull
ggpull   pull origin $(git_current_branch)
gp       push
ggpush   push origin $(git_current_branch)
gsta     stash
```

# Vim integration
[Vim-fugitive plugin](https://github.com/tpope/vim-fugitive)

* `:Git`: for running any arbitrary command
* `:Gwrite` writes to both the work tree and index versions of a file, making it like git add when called from a work tree file and like `git checkout` when called from the index or a blob in history.
* `:Gread` is a variant of `git checkout -- filename` that operates on the buffer rather than the filename. This means you can use `u` to undo it and you never get any warnings about the file changing outside Vim
* `:Gstatus`: Bring up the output of git status with
  * `-`: Press to `add`/`reset` a file's changes
  * `p`: Press to `add`/`reset` `--patch`
* `:Gcommit`: commit file(s)
* `:Gmove`: does a `git mv`
* `:Gremove`: does a `git rm`
* `:Glog`: loads all previous revisions of a file into the quickfix list so you can iterate over them and watch the file evolve
* `:Gbrowse`: to open the current file on GitHub, with optional line range (try it in visual mode!)

# Getting Started

### Saving changes
* `git commit -a -m <message>`: Commit a snapshot of all changes in the working directory. This only includes modifications to tracked files (those that have been added with `git add` at some point in their history).

### Inspecting repo
* `git log`: Display the entire commit history

    ```sh
    # shows the full diff of each commit
    $ git log -p

    $ git log --oneline

    # multiple lines per version for file
    $ git log --oneline -- <filename>

    # search for commits with a commit message that matches
    $ git log --grep="<pattern>"

    # —graph flag that will draw a text based graph
    # —decorate adds the names of branches
    $ git log --graph --decorate --oneline
    ```

### Viewing old commits
* `git checkout`: for checking out files and commits

    Checking out a file affects the current state of your project
    ```sh
    # check out a previous version of file
    $ git checkout <commit> <file>

    # If you decide you don’t want to keep the old version, you can check out the most recent version with:
    $ git checkout HEAD <file>
    ```

    Edit files without worrying about losing the current state of the project. Nothing you do in here will be saved in your repository
    ```sh
    # update all files in the working directory to match the specified commit.
    # will put you in a detached HEAD state
    $ git checkout <commit>
    ```

### Viewing differences
* `git diff`: Show differences between your working directory and the index

    ```sh
    # list differences between current and historical version
    $ git diff <hash> -- <file>

    # compare the working directory with local repository
    $ git diff HEAD <file>

    # compare the working directory with index
    $ git diff <file>

    # compare the index with local repository
    $ git diff --cached <file>
    ```

### Undoing changes
* `git checkout`
* `git revert`:  Generate a new commit that undoes all of the changes introduced in `<commit>`, then apply it to the current branch. Can target an individual commit at an arbitrary point in the history.

    ```sh
    $ git revert <commit>
    ```

* `git reset`: Used to undo local changes in the staging area (can only work backwards from the current commit). More "dangerous" way to undo changes than `git revert` -- it's permanent.

    ```sh
    # unstages a file without overwriting any changes
    $ git reset <file>

    # unstages all files without overwriting any changes
    $ git reset

    # move current branch tip backward to <commit>, reset the staging area to match, but leave the working directory alone
    $ git reset <commit>
    ```

* `git clean -f`: Removes untracked files from your working directory. Can't undo.

### Rewriting history
* `git commit --amend`: Convenient way to fix up the most recent commit. Combine staged changes with previous commit and replace the previous commit
* `git rebase`: The process of moving a branch to a new base commit, used to maintain a linear project history. Assume the following history exists and the current branch is **topic**:

    ```sh
              A---B---C topic
             /
        D---E---F---G master
    ```

    From this point, the result of either of the following commands:

    ```sh
    $ git rebase master
    $ git rebase master topic
    ```

    would be:

    ```sh
                      A'--B'--C' topic
                     /
        D---E---F---G master
    ```

* `git rebase -i <base>`: Instead of blindly moving all commits to new base, interactive rebasing lets you alter individual commits in the process. You can clean up history by removing, splitting, and altering an existing series of commits.
* `git reflog`: After rewriting history, the reflog contains information about the old state of branches and allows you to go back to that state if necessary.

## Collaborating

### Syncing
* `git remote`

    ```sh
    # add remote repo
    $ git remote school me@berkeley.edu:~/my_repo
    $ git remote -v
    $ git remote show origin
    ```

* `git fetch`:  Fetched content is represented as a remote branch, it has absolutely no effect on your local development work. Safe way to review commits before integrating them with your local repository.

    ```sh
    $ git fetch <remote>
    $ git fetch <remote> <branch>
    ```

* `git merge`: Merge two branches. Resolve merge conflicts in branch rather than pulling commits into master.

    ```sh
    # undo merge before it's completed, e.g. there's a merge conflict you'd like to undo
    $ git merge --abort

    # undo merge after you've completed the merge
    $ git reset --hard <commit>

    # to fix merge conflicts:
    $ git checkout master
    $ git pull
    $ git checkout branch
    $ git merge master # resolve conflicts and create new commit
    $ git push
    ```

* `git pull`: Fetch the specified remote’s copy of the current branch and immediately merge it into the local copy. `git pull --rebase` is used to ensure a linear history by preventing unnecessary merge commits.

    ```sh
    $ git pull <remote>

    # same as above, but use rebase instead of merge to integrate
    $ git pull --rebase <remote>

    # consider adding config option to always rebase
    $ git config --global branch.autosetuprebase always
    ```

* `git push`: Publish your local changes to a central repository. `git push` is essentially the same as running `git merge master` from inside the remote repository. Example:

    ```sh
    $ git checkout master

    # make sure local master is up-to-date
    $ git fetch origin master

    # rebase your changes on top of them and
    # squash commits, fix up commit messages etc.
    $ git rebase -i origin/master

    $ git push origin master
    ```

### Branches
* `git branch`

    ```sh
    # show all branches
    $ git branch -a

    # delete local branch
    $ git branch -d <branch>

    # delete remote branch
    $ git push origin --delete <branch>

    # view remote branches
    $ git branch -r

    # rename the current branch to <branch>
    $ git branch -m <branch>
    ```


### Contributing
1. Fork repo and clone locally.
2. Connect local repo to the original `upstream` repository by adding it as a remote.

    ```sh
    $ git remote -v
    origin  https://github.com/your_username/your_fork.git (fetch)
    origin  https://github.com/your_username/your_fork.git (push)

    $ git remote add upstream https://github.com/og_owner/og_repo.git

    $ git remote -v
    origin    https://github.com/your_username/your_fork.git (fetch)
    origin    https://github.com/your_username/your_fork.git (push)
    upstream  https://github.com/og_owner/og_repo.git (fetch)
    upstream  https://github.com/og_owner/og_repo.git (push)
    ```

3. Pull in changes from `upstream` often so that you stay up to date so that when you submit your pull request, merge conflicts will be less likely.

    ```sh
    $ git pull upstream
    ```

4. Create a branch for your edits.


## Cherry-pick
Take specific commits and insert into a particular branch.
```sh
$ git checkout master

# bring commit to master
$ git cherry-pick <commit>
```

## Stash
Switch branches without making a commit, and still be able to get back to this point later
```sh
# view stored stashes
$ git stash list
  stash@{0}: WIP on master: 049d078 added the index file
  stash@{1}: WIP on master: c264051 Revert "added file_size"
  stash@{2}: WIP on master: 21d80a5 added number to log

# reapply the one you just stashed
$ git stash apply

# apply one of the older stashes
$ git stash apply stash@{1}

# remove stash from stack
$ git stash drop stash@{0}
```
