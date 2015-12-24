# MongoDB
start mongodb
```bash
# w/o launchctl
$ mongod --config /usr/local/etc/mongod.conf

# w/ brew
$ brew services start mongodb

# w/ launchctl (doesn't work with tmux)
$ launchctl load ~/Library/LaunchAgents/homebrew.mxcl.mongodb.plist

# at login by launchd
$ ln -sfv /usr/local/opt/mongodb/*.plist ~/Library/LaunchAgents
```
