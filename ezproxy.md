EZproxy
=======

```bash
::Common
Set access = "deny";
If Region() eq "OH"; Set access = "allow"
If access eq "deny"; Deny irefused.htm
/Common
```