# zsh

There are five [startup files](http://zsh.sourceforge.net/Intro/intro_3.html) that zsh will read commands from:

* `.zlogin` should contain commands that should be executed only in login shells.
* `.zshrc` is sourced in interactive shells. It should contain commands to set up aliases, functions, options, key bindings, etc.
* `.zprofile` is similar to `.zlogin`, except that it is sourced before `.zshrc`.

## file permissions
triad
* user, group, other

notation
* 4: read, 2: write, 1: execute

```bash
# read/write/execute for user, read/write for group and other
$ chmod 755 a_file
$ ls -l a_file
-rwxr-xr-x 1 coop coop 1601 Mar 9 15:04 a_file
```
make file executable for all user types
```bash
$ chmod +x file.sh
```
make file not executable
```bash
$ chmod -x file.sh
```

## multiple commands
run three commands even if preceding one fails
```bash
$ make ; make install ; make clean
```
abort subsequent commands if one fails
```bash
$ make && make install && make clean
```
proceed until something succeeds and then stop executing any further steps
```bash
$ cat file1 || cat file2 || cat file3
```

## history
load history
```bash
$ history | tail
# or
$ fc -l
# or
$ tail $HISTFILE
```
don't save command to history (space in front of command)
```bash
$  secretcommand
```

### environment
show all environment variables
```bash
$ printenv
$ export
```
