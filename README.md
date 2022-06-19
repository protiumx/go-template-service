# template-go-service
Service oriented golang template with relational database.
The goal of this template is to provide an initial setup for DB migrations and code generation,
using Docker, Postgres, and a task runner.

## Dependencies
- [Docker](https://docker.com)
- [sqlc](https://github.com/kyleconroy/sqlc): Generate go code from `sql` queries
- [go-migrate](https://github.com/golang-migrate/migrate): Manage DB migrations
- [Task](https://taskfile.dev): Task Runner
- [dbml2sql](https://www.dbml.org/cli/): Generate sql from dbml
- [pgx](https://github.com/jackc/pgx): Postgres driver

## Init
This template does not include the go module file. To create a new one and add dependencies run:
```sh
go mod init <package>
go mod tidy
go get ./...
```

## Folder structure
- `cmd/`: app entry points
- `db/`:
  - `migrations/`: SQL migrations files
  - `queries/`: SQL query files used by `sqlc`
- `pkg/`: app sources

## Docker compose
Docker compose can be used to setup a db instance. Use
```sh
docker-compose up -d
```
**Note:** migrations will run once the postgres container is ready using [docker-compose depend-on](https://docs.docker.com/compose/startup-order/)

## Database
The [schema.dbml](./db/schema.dbml) file is the db schema source of truth.
It is used to generate the `schema.sql` and initial migration.

DB driver is `pgx` but it can be changed in the [sqlc configuration](./sqlc.yaml).

## Tasks
List all available tasks
```sh
task -l
```
**Note:** a `Makefile` is also provided for comparison with `Taskfile`.
### Testing
Run tests with
```sh
task test

# or specific folders
task test -- pkg/db
```
For integration testing consider using docker-compose or `task db-up` to get a test db instance.

## CI
A github actions [workflow](./.github/workflows/ci.yml) is configured to run: `go fmt, vet, test`.
The workflow also runs [gosec](https://github.com/securego/gosec).
