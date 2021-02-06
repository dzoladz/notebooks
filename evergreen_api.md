Documentation relating to the Evergreen ILS APIs are scattered, so I'm trying to bookmark useful links and commands.

- OSRF Gateway: [https://wiki.evergreen-ils.org/doku.php?id=osrfhttp:opensrf_gateway](https://wiki.evergreen-ils.org/doku.php?id=osrfhttp:opensrf_gateway)
- SuperCat: [https://docs.evergreen-ils.org/eg/docs/latest/development/data_supercat.html](https://docs.evergreen-ils.org/eg/docs/latest/development/data_supercat.html)
- unAPI: [https://docs.evergreen-ils.org/eg/docs/latest/development/data_unapi.html](https://docs.evergreen-ils.org/eg/docs/latest/development/data_unapi.html)
- opacAPI: [https://wiki.evergreen-ils.org/doku.php?id=documentation:technical:opacapi](https://wiki.evergreen-ils.org/doku.php?id=documentation:technical:opacapi)
- Developer Overview: [https://wiki.evergreen-ils.org/doku.php?id=eg_developer_overview](https://wiki.evergreen-ils.org/doku.php?id=eg_developer_overview)


# Commands

## Send Username and Password to auth.login
POST credentials. Returns a JSON blob with authtoken. Either `username` or `barcode` can be sent.

```shell
wget -O- -q --post-data 'service=open-ils.auth&method=open-ils.auth.login&param={"username":"some_username","password":"some_username
_password","type":"opac"}' https://cool-cat.org/osrf-gateway-v1
```