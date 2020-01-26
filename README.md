# kemal-session-postgres

[![Build Status](https://travis-ci.org/iomcr/kemal-session-postgres.svg?branch=master)](https://travis-ci.org/crisward/kemal-session-postgres)

This is a postgres adaptor for [Kemal Session](https://github.com/kemalcr/kemal-session)

## NOTE

This is a ctrl+f replace fork of `kemal-session-mysql`. I am using it until I need to switch to redis for sessions in the future. I have not updated the tests (yet).


## Installation

Add this to your application's `shard.yml`:

```yaml
dependencies:
  kemal-session-postgres:
    github: iomcr/kemal-session-postgres
```

## Usage

```crystal
require "kemal"
require "kemal-session-postgres"
require "postgres"

# connect to postgres, update url with your connection info (or perhaps use an ENV var)
connection = DB.open "postgres://root@localhost/test?sslmode=require"

Session.config do |config|
  config.cookie_name = "postgres_test"
  config.secret = "a_secret"
  config.engine = Session::PostgresEngine.new(connection)
  config.timeout = Time::Span.new(1, 0, 0)
end

get "/" do
  puts "Hello World"
end

post "/sign_in" do |context|
  context.session.int("see-it-works", 1)
end

Kemal.run
```

If you are already using crystal-postgres you can re-use the reference to your connection.

## Optional Parameters

```
Session.config do |config|
  config.cookie_name = "postgres_test"
  config.secret = "a_secret"
  config.engine = Session::PostgresEngine.new(
    connection: connection,
    sessiontable: "sessions",
    cachetime: 5
  )
  config.timeout = Time::Span.new(1, 0, 0)
end
```
|Param        |Description
|----         |----
|connection   | A Crystal postgres DB Connection
|sessiontable | Name of the table to use for sessions - defaults to "sessions"
|cachetime    | Number of seconds to hold the session data in memory, before re-reading from the database. This is set to 5 seconds by default, set to 0 to hit the db for every request.


## Contributing

1. Fork it ( https://github.com/iomcr/kemal-session-postgres/fork )
2. Create your feature branch (git checkout -b my-new-feature)
3. Commit your changes (git commit -am 'Add some feature')
4. Push to the branch (git push origin my-new-feature)
5. Create a new Pull Request

## Contributors

- [iom](https://github.com/InstanceOfMichael) IOM - forked for postgres
- [crisward](https://github.com/crisward) Cris Ward - creator, maintainer (of mysql version)
