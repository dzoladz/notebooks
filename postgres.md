Postgres
============
Installation on MacOS using Homebrew services

1. Install PostgreSQL, preferably a pinned version
```bash
brew install postgresql@10
```

2. If you don't have one, get the `psql` client.

```bash
brew install libpq
```

`libpq` won't install itself in the `/usr/local/bin` directory like other Homebrew applications. To make that happen, you need to run:

```bash
brew link --force libpq
```

3. Start Postgres services

```bash
```

4. Create a database, and...

```bash
createdb `first`
```

Fix role "postgres" does not exist error

```bash
createuser -s postgres
```
