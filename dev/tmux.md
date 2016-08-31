# tmux
### sessions
```
tmux new -s work       start new with session name
tmux attach -t work    attach to a session
tmux ls                list sessions
prefix d               detach from current session
```

### windows
```
prefix c               create new tmux window
prefix 0-9             switch between windows
prefix ,               rename window
prefix q               show pane numbers
prefix ?               list shortcuts
:move-window -t 1      move current window to first position
```

### split-panes
```
prefix !               move pane to new window
prefix {               move pane to previous position
prefix }               move pane to next position
:join-pane -s :1       place window 1 into pane on current window
:join-pane -s 2 -t 1   move 2nd window as a pane to 1st window
```

### other
include in `tmux.conf` to split window at current directory:
```bash
bind-key v split-window -h -c "#{pane_current_path}"
bind-key s split-window -v -c "#{pane_current_path}"
```
