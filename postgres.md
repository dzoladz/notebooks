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

## PSQL
- Run psql client as user postgres `psql -U postgres`

#### Quick Start
- `\q` - Quit/Exit
- `\l` - List databases
- `\c <database>` - Connect to a database
- `\x` - Pretty-format query results


#### Up and Running
- `\d <table>` - Show table definition, including triggers
- `\dy` - List events
- `\df` - List functions
- `\di` - List indexes
- `\dn` - List schemas
- `\dv` - List views
- `\copy (SELECT * FROM __table_name__) TO 'file_path_and_name.csv' WITH CSV`: Export a table as CSV

## COMMANDS

- Create database `createdb <database_name>`
- Drop database `dropdb <database_name>`
