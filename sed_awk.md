## sed

You got 60+ logrotate configuration files in `/etc/logrotate.d` and you need to add the `delaycompress` directive, after the `compress` directive, to each of the files. The name of each configuration file that needs to be updated begins with `ezproxy_`. No problem!

```bash
sed -i '/compress/a\    delaycompress' ezproxy_*
```

## awk
