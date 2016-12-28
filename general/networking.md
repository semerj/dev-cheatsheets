# IP, ports, MAC address

## ip

get local ip
```sh
$ ifconfig
$ ip addr show
$ hostname -I
```

get external ip
```sh
$ curl ifconfig.me
```

show logged on user
```sh
$ w
```

## `nmap`

scan for open ports
```sh
$ nmap localhost
```

## `lsof`

list which ports are open
```sh
$ sudo lsof -i -P | grep -i "listen"
```

list open IPv4 connections
```sh
$ lsof -Pnl +M -i4
```

## `netstat`

list which ports are open
```sh
$ netstat -atp | grep -i "listen"
```

## `networksetup`

list all network hardware MAC addresses
```sh
networksetup -listallhardwareports
```

## `arp-scan`

scan wifi network for all connected devices with MAC addresses
```sh
$ sudo arp-scan -l
```

can also do the following
```sh
$ ifconfig | grep broadcast
# ping broadcast address
$ ping 10.0.1.3
$ arp -a
```
