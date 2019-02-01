Tasks
=====
A collection commands for tasks that I've needed to perform multiple times.

## Delete Old Kernels, Classic Version

```bash
uname -r
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
