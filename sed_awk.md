## sed (stream editor)

You got 60+ logrotate configuration files in `/etc/logrotate.d` and you need to add the `delaycompress` directive, after the `compress` directive, to each of the files. The name of each configuration file that needs to be updated begins with `ezproxy_`. No problem!

```bash
sed -i '/compress/a\    delaycompress' ezproxy_*
```

You need to add a temporary set of limited administrative credentials to all of your `user.txt` files and you want to the new code after either `::limit=2` or `::Limit=2`. NOTE: As written below, `sed` will be in dry-run mode. To perform the operation, use `sed -i`.

```bash
find . -iname user.txt -exec sed '/::[lL]imit=2/a ::group=+Admin.StatusUpdate+Admin.Restart \
username:password \
::group=-Admin.StatusUpdate-Admin.Restart' {} \;| less
``` 

## awk

Read through a directory of log files, find entries for a specific user, sort the results, print only the "interesting" data points

```bash
grep -rni "username" | sort | awk 'BEGIN {OFS="\t"}; {split($1,date,":"); print date[3], $2, $3, $4}'
```

