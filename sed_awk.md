## sed

You got 60+ logrotate configuration files in `/etc/logrotate.d` and you need to add the `delaycompress` directive, after the `compress` directive, to each of the files. The name of each configuration file that needs to be updated begins with `ezproxy_`. No problem!

```bash
sed -i '/compress/a\    delaycompress' ezproxy_*
```

## awk

Read through a directory of log files, find entries for a specific user, sort the results, print only the "interesting" data points

```bash
grep -rni "username" | sort | awk 'BEGIN {OFS="\t"}; {split($1,date,":"); print date[3], $2, $3, $4}'
```
