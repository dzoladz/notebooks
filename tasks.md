Tasks
=====
A collection commands for tasks that I've needed to perform multiple times.

## Delete Old Kernels, Classic Version

```bash
uname -r # the kernel in use
```

```bash
dpkg -l | grep linux-image | awk '{print$2}'
```

```bash
apt remove --purge linux-image-3.1.0-131-generic
```

```bash
update-grub
```

## Find the Size of a Directory

```bash
du -hs /path/to/directory
```

## Tuning with Sysctl

File Location on Ubuntu

```bash
nano -w /etc/sysctl.conf
```

Count instances of SYN received

```bash
netstat -ant|grep SYN_RECV|wc -l
```

Check States
```bash
netstat -nta | egrep "State|443"
netstat -nta | egrep "State|80"
```

Check Current Value for Parameter

```bash
sysctl -n net.ipv4.tcp_syncookies
sysctl -n net.ipv4.tcp_max_syn_backlog
sysctl -n net.ipv4.tcp_synack_retries
sysctl -n net.core.somaxconn
```

Reload configuration

```bash
sysctl -p
```
