EZproxy
=======
A snippet of fragments useful for administering EZproxy

## Reject connections that originate from IPs outside of Ohio region.

Region codes should be specified using the two-character [ISO 3166-2 subcountry code for the US](https://geolite.maxmind.com/download/geoip/misc/region_codes.csv).

GeoLite data must also be configured on EZproxy (see the [Location](https://help.oclc.org/Library_Management/EZproxy/Configure_resources/Location) section of the EZproxy manual).

```bash
::Common
Set access = "deny";
If Region() eq "OH"; Set access = "allow"
If access eq "deny"; Deny irefused.htm
/Common
```

Regex for use in the main authentication block. Deny all access, unless the location contains OH, IA, OR MI in the string.

```bash
Set access = "deny";
If Region() =~ "/.*(OH|IA|MI).*/"; Set access = "allow"
If access eq "deny"; Deny irefused.htm
```

The above should be placed before any `IfRefused; Deny irefused.htm` or `IfExpired; Deny iexpired.htm` directives in the main authentication block.

It's also possible to restrict authentication to US-based IPs, using the `IfCountry` function. `IfCountry` used the two-letter [ISO 3166 Country Codes](https://dev.maxmind.com/geoip/legacy/codes/iso3166/).

```bash
Set access = "deny";
IfCountry US; Set access = "allow"
If access eq "deny"; Deny irefused.htm
```

__NOTE__: The above snippet of code needs to be placed __within__ the authentication block to be actively restriction authentication attempts that originate from a non-US country code.

## Supply Resource Credentials within an iFrame
There are times when you might need to Find/Replace username and password values within an `<iframe>` element The example below is a working draft of two auto-triggered functions to input credentials into the proper form fields.
```html
Find </iframe>
Replace </iframe><script>(function() { setTimeout(function() {window.frames[0].document.getElementById('username').value = "USERNAME";}, 1000); })();(function() { setTimeout(function() {window.frames[0].document.getElementById('password').value = "PASSWORD";}, 1000); })();</script>
```

## Regex Validation of SAML Attributes in the Auth: Namespace
SAML authorization check, within `shibuser.txt`, against the `userid` attribute released to EZproxy
> Authorize `userid`, if it begins with  1, 3, or 5. Otherwise, record `userid` and deny with `itype.htm`
```bash
If !(auth:userid =~ "/^(1|3|5).+$/"); Audit -expr auth:userid; Deny itype.htm; Stop
```

## Byte Order Mark (BOM)

When EZproxy configuration files are edited in Microsoft NotePad, EZproxy will complain - upon restart - about the single byte character `ï»¿` that begins the `config.txt` file. EZproxy expects UTF-8 encoding that does not start with a non-ASCII byte, like a BOM.

Using vim, does current file have a BOM?

```bash
:set bomb?
```

Remove BOM and write file back to disk:

```bash
:set nobomb
```

## Auto-trigger attribute replacement via EZproxy Find/Replace 
```javascript
(function() {
        var url = document.getElementById('ctl00_BodyContent_ucShare_txtTitleURL').value;
        var fixed_url = url.replace('fod.infobase.com', 'fod-infobase-com');
        document.getElementById('ctl00_BodyContent_ucShare_txtTitleURL').setAttribute('value', fixed_url);
})();
```

## Cron Jobs for Monitoring Activity
```bash
10 0 * * * find /usr/local -name messages.txt | xargs grep -E "Unrecognized|DANGER|hosts\s36[0-9][0-9]" | mail -E -s "EZproxy Warning Messages" -a "From: root \<root@{hostname}\>" recipient@derekzoladz.com

20 0 * * * find /usr/local/ezproxy/audit/$(date --date='yesterday' "+\%Y\%m\%d").txt -type f | xargs grep -E "exceeded" | mail -E -s "EZproxy Exceeding Usage Limit" -a "From: root \<root@{hostname}\>" recipient@derekzoladz.com

30 0 * * * find /usr/local/ezproxy/audit/$(date --date='yesterday' "+\%Y\%m\%d").txt -type f -print| xargs grep -E "Login.Intruder.IP" | mail -E -s "EZproxy Login.Intruder.IP" -a "From: root \<root@{hostname}\>" recipient@derekzoladz.com

40 0 * * * find /usr/local/ezproxy/audit/$(date --date='yesterday' "+\%Y\%m\%d").txt -type f -print | xargs grep -E "Session.ReconnectBlocked" | mail -E -s "EZproxy Session.ReconnectBlocked" -a "From: root \<root@{hostname}\>" recipient@derekzoladz.com

00 8 * * 1-5 find /usr/local/ezproxy/cookies -cmin +720 -type f | mail -E -s "EZproxy Sessions Over 12 Hours" -a "From: root \<root@{hostname}\>" recipient@derekzoladz.com
```

## Strip Parameters from DOIs
Although valid extensions of the Document Object Model, passing parameters in the doi can cause resolution issues within EZproxy sessions.
> {host}?locatt=label:secondary_bloomsburyCollections
```bash
SPUEditVar proxy_login=login?url=
SPUEdit @^(https:\/\/doi.org\/)(10.[0-9]*\/)([0-9]*)(\?locatt=label.*)$@${proxy_login}$1$2$3@ir
```

## Use Input onclick="processUsername()" to Submit Only Username
Useful in those cases when only the username is accepted on the target server and username@institution.edu fails. Removes the need for textual directions on the login UI.

```javascript
function processUsername() {
  var email = document.getElementsByName('user')[0].value;
  var username = email.split('@')[0];
  document.getElementsByName('user')[0].value = username;
  console.log('the username value submitted is: ' + document.getElementsByName('user')[0].value);
}
```

## Nested user.txt SAML Authorization | Resource Groups 

```shell
If auth:"urn:oid:2.5.4.10" eq "Student"; {
  If auth:"CurrentMajor" eq "Business"; Group +Business
  If auth:"CurrentMajor" eq "Law"; Group +Law
}
 
If auth:"urn:oid:2.5.4.10" eq "Faculty"; {
  If auth:"Department" eq "Law"; Group +Law
}
```

## Group-based Access to /Loggedin Files

1. In `user.txt`, create the authenticated group
```bash
::group=SecretFiles+Admin.Groups
user:password
```

2. Establish a `../loggedin/` directory for the named Group from `user.txt`
```bash
./docs/loggedin/SecretFiles
```

## Issue a Manual Call to the Sierra Patron API
Given a Sierra Patron Record containing `P BARCODE[pb]=fakeuser1234`
```bash
curl --interface eth0:48 https://{sierra-server-domain}:54620/PATRONAPI/fakeuser1234/dump
```

If successful, the call will return patron data; otherwise, a failure message will be returned with additional information to diagnose the issue. For example:
```bash
<BODY>ERRNUM=2<BR>
ERRMSG=Record ID not unique<BR>
</BODY>
```

## sed (stream editor)
**To perform an operation, use `sed -i`; otherwise, `sed` performs a dry-run.**  

You got 60+ logrotate configuration files in `/etc/logrotate.d` and you need to add the `delaycompress` directive, after the `compress` directive, to each of the files. The name of each configuration file that needs to be updated begins with `ezproxy_`. No problem!

```bash
sed '/compress/a\    delaycompress' ezproxy_*
```

You've got 60+ Let's Encrypt certificate `.conf` files that you need to increase
the delay for propogation of the DNS TXT file challenge. In the example below,
if a line in the configuration file contains `dns_google_propagation_seconds`,
find the value `60` and replace it with the value `90`.

```bash
sed '/dns_google_propagation_seconds/s/60/90/' *.conf
```

You need to add a temporary set of limited administrative credentials to all of
your `user.txt` files and you want to the new code after either `::limit=2` or
`::Limit=2`.

```bash
find . -iname user.txt -exec sed '/::[lL]imit=2/a ::group=+Admin.StatusUpdate+Admin.Restart \
username:password \
::group=-Admin.StatusUpdate-Admin.Restart' {} \;| less
``` 

Or, when a vendor transitions to HTTPS and you need to update 60+ EZproxy stanzas to specifically allow a new protocol.

#### Here are a few one-liners:

```
# Print All Matching Lines
sed -n '/^IncludeFile.*/p' config.txt

# Print the First Occurrence of Matching Line
sed -n '/^IncludeFile.*/ {p;q}' config.txt

# Print Last Occurrence of a Match
tac config.txt | sed -n '/^IncludeFile.*/ {p;q}'
```

## awk (columnar data processor)

Read through a directory of log files, find entries for a specific user, sort the results, print only the "interesting" data points

```bash
grep -rni "username" | sort | awk 'BEGIN {OFS="\t"}; {split($1,date,":"); print date[3], $2, $3, $4}'
```

You can also find IPs that hit the proxy server most frequently
```bash
awk '{ print $4}' $PATH_TO_LOGFILE | sort | uniq -c | sort -nr | head -n 20
```
