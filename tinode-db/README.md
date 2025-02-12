# Utility to Create and initialize `tinode` DB in a RethinkDB or MySQL

This utility initializes the `tinode` database and optionally loads it with sample data. If the tool finds that the database already exists and of the same version as the expected, it won't do anything. To force database reset use command line option `--reset=true`.

## Build the package:

 - **RethinkDB**
  `go build -tags rethinkdb` or `go build -i -tags rethinkdb` to automatically install missing dependencies.

 - **MySQL**
  `go build -tags mysql` or `go build -i -tags mysql` to automatically install missing dependencies.


## Run

Run from the command line.

`tinode-db [parameters]`

Command line parameters:
 - `--reset`: delete the database then re-create it in a blank state. Has no effect if the database does not exist.
 - `--data=FILENAME`: fill `tinode` database with data from the provided file. See [data.json](data.json).
 - `--config=FILENAME`: load configuration from FILENAME. Example config is included as [tinode.conf](tinode.conf).
 

Configuration file options:
 - `uid_key` is a base64-encoded 16 byte XTEA encryption key to (weakly) encrypt object IDs so they don't appear sequential. You probably want to use your own key in production.
 - `store_config.adapters.mysql` and `store_config.adapters.rethinkdb` are database-specific sections:
  - `database` is the name of the database to generate.
  - `addresses` is RethinkDB's host and port number to connect to. An array of hosts can be provided as well `["host1", "host2"]`.
  - `dsn` is MySQL's Data Source Name.

The `uid_key` is only used if the sample data is being loaded. It should match the key of a production server and should be kept private.

The default `data.json` file creates six users with user names `alice`, `bob`, `carol`, `dave`, `frank`, and `tino` (chat bot user). Passwords are the same as the user names with 123 appended, e.g. user `alice` gets password `alice123`; `tino` gets a randomly generated password. It also creates three group topics, and multiple peer to peer topics. Users are subscribed to topics and to each other. All topics are randomly filled with messages.

Avatar photos curtesy of https://www.pexels.com/ under [CC0 license](https://www.pexels.com/photo-license/).

## Links:

* [RethinkDB schema](https://github.com/Joichiro/chat/tree/master/server/db/rethinkdb/schema.md)
* [MySQL schema](https://github.com/Joichiro/chat/tree/master/server/db/mysql/schema.sql)
