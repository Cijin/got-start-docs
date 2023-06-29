# go-start

A go template to get you started on your projects swiftly. `go-start` was created
out my own need and frustration when starting a new project. Blank canvases are
great but get in the way of getting things done, when you have to spend days on project setup.

Made for bootstrappers.

## Important

Update the database secrets inside the `Makefile` first.

Managing secrets have been deliberately ommited in `v1.0` and it's left up to
you to decide. Hide the variables in the `Makefile` if you intend to make the source
of your project public.

My recommendation would be to use something similar to [vault](https://developer.hashicorp.com/vault/downloads).

## Getting Started

The easiest way to get familiar with the project is have a look at the `Makefile`.
It consists of all the commands you would need. From starting the database to
mocking api's for tests.

Follow these steps to get started:

1. Init the project using `make init`. This will update the `go.mod` to your project name, and update all local file imports
2. The following applications need to be installed:
   a. [Docker Cli](https://docs.docker.com/desktop/)
   b. [Golang Migrate CLI](https://github.com/golang-migrate/migrate/blob/master/cmd/migrate/README.md)
   c. [Sqlc](https://docs.sqlc.dev/en/stable/overview/install.html)
   d. [Gomock](https://github.com/golang/mock)
   e. [gRPC Prerequisites](https://grpc.io/docs/languages/go/quickstart/)
3. Update the database tables if requried
4. Run database migrations: `make migrateup`
5. Create & Start Database: `make createdb` & `make startdb`
6. Start Server: `make server`

## Migrations

1. Run `make createmigration <migration_name>` to create a new migration
2. Update both the `_up` & `_down` migrations
3. Once updated run `make migrateup` to run all migraions or `make migrateup1`
   to run only the newly created migration
4. To tear down any migrations use the `migratedown` or `migratedown1` commands
5. If a migration were to fail, after upating the failing migration. You'll need
   to mark `schema_migration` table as clean by setting `dirty:false` before
   re-running the migration

## Adding Database Methods

1. Go-start uses `sqlc` to generate the database methods
2. Define your database methods inside `db/query/`
3. Then run `make sqlc` to generate the types, & methods to access the database
   methods in your API
4. Make sure you run `make sqlc` everytime you update a method inside `db/query/`
5. You can read more about sqlc [here](https://docs.sqlc.dev/en/stable/tutorials/getting-started-postgresql.html)

## Adding a new API

1. Inside the proto folder create a new file with the `.proto` extension
2. Create the required definitions for the entity, request, & response
3. Register the new service inside `services.proto`
4. Run `make proto`. This will generate the neccessary types and functions
   inside `pb/`
5. Now inside `api/` you can import and use the new service, request, & response
   definitions to define how the API should behave

## Adding tests for an API

1. If there are database changes, start by running `make mock`
2. The `mock` command will generate the necessary database mocks
3. Add a new test file for your API and any database calls will invoke the
   mocked db

## Authentication

Auth is built in and can be opted in as needed. I've decided to use
[PASETO](https://paseto.io/) as it's opiniated and comes with safe defaults out
of the box. Although, this can be easily swapped out if you wish to do so.

## Why X and not Y?

The goal of this template is to act as a starter for projects. So I choose tools
that would get out of the way and allow for generation of code. This reduces
mistakes and the mudane gets taken care of. This is important because it's easy
to get side tracked while working on your project.

You can read more about some of these tools below.

## Tech

1. [Go](https://go.dev/)
2. [Postgres](https://www.postgresql.org/)
3. [Grpc](https://grpc.io/)
4. [Sqlc](https://sqlc.dev/)
