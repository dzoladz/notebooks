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

## Supply Resource Credentials withn an iFrame
There are times when you might need to Find/Replace username and password values within an `<iframe>` element The example below is a working draft of two auto-triggered functions to input credentials into the proper form fields.
```bash
Find </iframe>
Replace </iframe><script>(function() { setTimeout(function() {window.frames[0].document.getElementById('username').value = "USERNAME";}, 1000); })();(function() { setTimeout(function() {window.frames[0].document.getElementById('password').value = "PASSWORD";}, 1000); })();</script>
```

## Regex Validation of SAML Attributes in the Auth: Namespace
SAML authorization check, within `shibuser.txt`, against the `userid` attribute released to EZproxy
> Authorize `userid`, if it begins with  1, 3, or 5. Otherwise, record `userid` and deny with `itype.htm`
```bash
If !(auth:userid =~ "/^(1|3|5).+$/"); Audit -expr auth:userid; Deny itype.htm; Stop
```
