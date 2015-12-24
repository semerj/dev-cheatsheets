## list drives/file systems
```bash
$ sudo blkid
$ sudo fdisk -l
$ df -h
```

## vpn
```bash
# server log
$ sudo less /var/log/openvpn.log

# client log
$ less /Library/Application\ Support/Tunnelblick/Logs/

# server.conf
# source: https://gist.github.com/laurenorsini/9925434
$ less /etc/openvpn/server.conf
```

## vnc
```bash
# screen share (open finder, cmd-k)
vnc://[LOCAL IP]:5901

# set up
$ vncserver -kill :1
$ vncserver :1 -geometry 1024x728 -depth 24

# logs (from pi)
$ less ~/.vnc/parsnip\:1.log
```

## shutdown or reboot
```bash
$ sudo halt
$ sudo shutdown -h now
$ sudo shutdown -h 5 #turn off in 5 minutes
$ sudo reboot
```

## format usb
```bash
$ sudo mkdir /media/external_disk
$ sudo mkfs.ext4 /dev/sda1 -L untitled
$ sudo mount -t ext4 /dev/sda1 /media/usb
```

## security
```bash
# monitor ip addresses accessing pi through ssh
$ grep -oE "[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+" /var/log/auth.log | sort | uniq -c | sort -nr

# install fail2ban
$ sudo apt-get install fail2ban

# start fail2ban
$ sudo service fail2ban start

# install iptables
# more info here: http://blog.self.li/post/63281257339/raspberry-pi-part-1-basic-setup-without-cables

# check current bans
$ sudo iptables -L
```
