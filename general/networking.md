## ssh
```bash
# from local network
$ ssh [LOCAL IP] -l pi
$ ssh pi@[LOCAL IP] -l
$ ssh pi@raspberry.local
$ ssh username@ssh.ocf.berkeley.edu

# remote network access
$ ssh pi@[EXTERNAL IP] -p [PORT]

# after creating ssh key on client, copy public key to pi
$ ssh-copy-id -i ~/.ssh/id_rsa.pub pi@[INSERT IP]

# research ssh service after modifying /etc/ssh/sshd_config
$ sudo service ssh reload

# view key on pi
$ less ~/.ssh/authorized_keys
```

## Copying files

### sftp
```bash
$ sftp pi@[INSERT IP]
$ lpwd # local working directory
$ pwd  # remote working directory
```

### copy files/folders

| from  | to     | type      | scp (secure copy)                                       | sftp (secure ftp) |
|:------|:-------|:----------|---------------------------------------------------------|-------------------|
| local | pi     | file      | `scp -P [PORT] file.txt pi@[INSERT IP]:~`               | `put file.txt`    |
| local | pi     | folder    | `scp -r -P [PORT] ~/folder/ pi@[INSERT IP]:/home/pi`    | `put -r folder`   |
| pi    | local  | file      | `scp pi@[INSERT IP]:file.txt .`                         | `get file.txt`    |
| pi    | local  | folder    | `scp -r pi@[INSERT IP]:folder/ .`                       | `get -r folder`   |

### rsync
```bash
# get pi's ip
$ hostname -I

# copy files from remote to local
$ rsync -avzhe ssh pi@[INSERT IP]:~/remote_dir ~/local_dir

# copy files from local to remote
$ rsync -avzhe ssh ~/local_dir pi@[INSERT IP]:~/remote_dir
```

## IP, ports, MAC address

### ip
```bash
# local ip
$ ifconfig
$ ip addr show
$ hostname -I

# external ip
$ curl ifconfig.me

# show who is logged on and what they are doing
$ w
```

### nmap
```bash
# scan for open ports
$ nmap localhost
```

### lsof
```bash
# list which ports are open
$ sudo lsof -i -P | grep -i "listen"

# list open IPv4 connections
$ lsof -Pnl +M -i4
```

### netstat
```bash
# list which ports are open
$ netstat -atp | grep -i "listen"
```

### networksetup
```bash
# list all network hardware MAC addresses
networksetup -listallhardwareports
```

### arp-scan
```bash
# scan wifi network for all connected devices with MAC addresses
$ sudo arp-scan -l

# can also do the following
$ ifconfig | grep broadcast
# ping broadcast address
$ ping 10.0.1.3
$ arp -a
```


## Job control command list
```bash
# keep program running after logging out of server
$ nohup command-name &

# run command in the background
$ my_command &

# stop foreground process
$ Ctrl-z

# list all jobs
$ jobs -l

# list background processes
$ bg

# bring a background process to the foreground
$ fg %1

# kill a process
$ kill %1
```
