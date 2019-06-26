Tasks
=====
A collection commands for tasks that I've needed to perform multiple times.

## Byte order mark (BOM)

When EZproxy configuration files are edited in Microsoft NotePad, EZproxy will complain - upon restart - about the single byte character `ï»¿` that begins the `config.txt` file. EZproxy expects UTF-8 encoding that does not start with a non-ASCII byte, like a BOM.

Using vim, does current file have a BOM?

```bash
:set bomb?
```

Remove BOM and write file back to disk:

```bash
:set nobomb
```

## Print the Number of Lines in a File

Because I can never remember the command I need

```bash
wc -l <file>
```

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

## Convert .mov -> .mp4 With FFMPEG

```bash
ffmpeg -i Untitled.mov -vcodec h264 -acodec mp2 Untitled.mp4
```

## Find the Size of a Directory

```bash
du -hs /path/to/directory
```

## Locate all files with a specific name in the entire directory and search through those files for a line of text using regex, sort

```bash
find . -iname config.txt -exec grep "^[A-Za-z ]*\.ohionet\.org" {} \; -print | grep Name | sort
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
