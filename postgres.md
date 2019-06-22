Postgres
============
Installation on MacOS using Homebrew services

## First Steps

1. Install PostgreSQL, preferably a pinned version
```bash
brew install postgresql@10
```

2. If you don't have one, get the `psql` client.

```bash
brew install libpq
```

> `libpq` won't install itself in the `/usr/local/bin` directory like other Homebrew applications. To make that happen, you need to run:

```bash
brew link --force libpq
```

3. Start Postgres services

```bash
brew services start postgresql
```

> Check `brew info postgresql`

4. Create a database, and...

```bash
createdb `first`
```

> Fix role "postgres" does not exist error

```bash
createuser -s postgres
```
## Initial Reading
[PostgreSQL Guide](http://postgresguide.com/)

## PSQL
- Run psql client as user postgres - `psql -U postgres`
- Connect to local postgres database as a specific user - `psql -h localhost -U <postgres_user> <database>``

#### Quick Start
- `\?` - List all available commands
- `\q` - Quit/Exit
- `\l` - List databases
- `\c <database>` - Connect to a database


#### Up and Running
- `\d <table>` - Show table definition, including triggers
- `\dy` - List events
- `\df` - List functions
- `\di` - List indexes
- `\dn` - List schemas
- `\dv` - List views
- `\e` - Open default text editor in psql shell
- `\copy (SELECT * FROM __table_name__) TO 'file_path_and_name.csv' WITH CSV` - Export a table as CSV

#### Settings
- `\timing` - Turn on query timing
- `\x` - Pretty-format query results

## COMMANDS

- Create database `createdb <database_name>`
- Drop database `dropdb <database_name>`
