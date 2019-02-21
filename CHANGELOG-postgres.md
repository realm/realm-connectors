1.9.0 Release notes (2019-02-21)
================================
## Enhancements
* Reduced memory usage when processing Realm changesets which contain a very large number of changes. ([#461](https://github.com/realm/data-adapters/pulls/461))
* Improve performance and reduce server-side file size growth by eliminating some redundant re-setting of properties. ([#461](https://github.com/realm/data-adapters/pulls/461))

## Fixes
* Fix a race condition which could lead to updates to objects made shortly after they are inserted being lost and not propagated to postgres. ([#461](https://github.com/realm/data-adapters/pulls/461), since v1.0.0)
* When column/property mapping is enabled, deleting a row could lead to `Invalid null value for non-nullable primary key`. ([forum#2461](https://forum.realm.io/t/postgresql-adapter-potential-bug-with-delete-operations/2481), since v1.5.0)

## Compatibility
* Realm Object Server: 3.11.0 or later
* APIs are backwards compatible with all previous releases in the 1.x.y series.


1.8.2 Release notes (2019-02-01)
================================
## Enhancements
* None.

## Fixes
* Querying Realm instances would sometimes pin the read transaction version, resulting in the file rapidly growing in size as further transactions were processed. ([realm/realm-object-store#766](https://github.com/realm/realm-object-store/pull/766), since v1.0.0)

## Compatibility
* Realm Object Server: 3.11.0 or later
* APIs are backwards compatible with all previous releases in the 1.x.y series.


1.8.1 Release notes (2019-01-18)
================================
## Enhancements
* None.

## Fixes
* Fixed a bug where users cannot create loggers using `createFileLogger()` and `createConsoleLogger()`, and they had to rely on either custom or default logger ([#447](https://github.com/realm/data-adapters/pull/477), since v1.8.0)

## Compatibility
* Realm Object Server: 3.11.0 or later
* APIs are backwards compatible with all previous releases in the 1.x.y series.


1.8.0 Release notes (2019-01-17)
================================
## Enhancements
* Reduced memory usage when processing a large number of Realm changesets at once. ([#452](https://github.com/realm/data-adapters/pulls/452))
* Improved performance and memory usage of `Realm.Sync.Adapter`. ([realm/realm-js-private#501](https://github.com/realm/realm-js-private/pull/501))
* To avoid excessive logging to the Console and hitting this node.js [issue](https://github.com/nodejs/node/issues/6379), a new logger is provided. It logs to a file `realm-adapter-log-YYYY-MM-DD-MM-SS.txt` created in the directory where the Adapter is started. The default logger is unchanged (logging to the Console) but you can switch to the file based logger by adding `logger: createFileLogger(loglevel, filename)` to the configuration. `loglevel` is optional and defaults to `INFO`, and `filename` is optional and defaults to `realm-adapter-log-YYYY-MM-DD-MM-SS.txt`. ([#451](https://github.com/realm/data-adapters/issues/451), since v1.5.0)

## Fixes
* Convert recursive processing of changesets pushed from Realm to Postgres to an iterative loop to avoid hitting the maximum call stack size when a very large number of changesets are processed at once. ([#441](https://github.com/realm/data-adapters/issues/441), since v1.0.0)
* Some errors were causing the default logger to crash when trying to report them. ([#450](https://github.com/realm/data-adapters/issues/450), since v1.5.0)
* Changes to rows which set a string field to a string containing a newline character are now propagated to Realm rather than being skipped ([#458](https://github.com/realm/data-adapters/pull/458), since v1.7.0)
* Fixed incorrect conversion of timestamps when the Postgres server's default timezone was not set to UTC. ([#444](https://github.com/realm/data-adapters/pull/444), since v1.0.0)

## Compatibility
* Realm Object Server: 3.11.0 or later
* APIs are backwards compatible with all previous releases in the 1.x.y series.


1.7.0 Release notes (2018-12-07)
================================
## Enhancements
* Added option `PostgresAdapterConfig.logSyncClient`. If true, the logs from the connection between the adapter and Realm Object Server will written to the logger specified by `PostgresAdapterConfig.logger`. ([#219](https://github.com/realm/data-adapters/issues/219))
* Added option `PostgresAdapterConfig.ignoreSQLErrors`. If true, rejected SQL statements will be ignored and the adapter will continue, otherwise the adapter will throw an exception and stop. ([#190](https://github.com/realm/data-adapters/issues/190))

## Fixed
* The [logLevel](https://github.com/realm/data-adapters/blob/master/packages/postgres/src/PostgresAdapterConfig.ts#L166) parameter is passed correctly to the logger when using TypeScript to configure the project, previously it would fallback to `info` level. ([#437](https://github.com/realm/data-adapters/issues/437), since 1.5.0)

## Compatibility
* Realm Object Server: 3.11.0 or later
* APIs are backwards compatible with all previous releases in the 1.x.y series.

1.6.0 Release notes (2018-11-28)
================================
## Enhancements
* Upgraded to Realm JavaScript v2.20.0. You will need to upgrade your Realm Object Server to at least version 3.11.0 or use [Realm Cloud](https://cloud.realm.io). If you try to connect to a ROS v3.10.x or previous, you will see an error like `Wrong protocol version in Sync HTTP request, client protocol version = 25, server protocol version = 24`. ([#391](https://github.com/realm/data-adapters/issues/391))
  - Improved the proactive token refresh mechanism to make several attempts to refresh the token before it expires.
  - Added support for Node 10.
  - A set of bugs that could lead to bad changesets have been fixed. An example of error message is `Failed to parse, or apply received changeset: ndx out of range`.

## Fixed
* Added a workaround to get stack trace progagated to the log. ([#397](https://github.com/realm/data-adapters/issues/397), since v1.5.0)

## Compatibility
* Realm Object Server: 3.11.0 or later
* APIs are backwards compatible with all previous releases in the 1.x.y series.

## Internal
* Cleaned up the distribution package. ([#405](https://github.com/realm/data-adapters/pull/405))
* Upgraded to Realm JavaScript v2.20.0.


1.5.0 Release notes (2018-11-21)
================================

## Enhancements
* Added an optional custom logger named `PostgresAdapterConfig.logger`. The default logger will log to the console with a log level set to `info` (see [winston](https://github.com/winstonjs/winston) for details). An example of the output is `2018-11-09T13:44:29.974Z [REALM ADAPTER] info: Found Postgres version < 10.`. ([#303](https://github.com/realm/data-adapters/issues/303))

## Fixed
* Fixed a bug where clients could miss changes from Postgres if the adapter crashes while integrating a change. Now restarting the adapter should resume from the last unprocessed Postgres transaction. ([#344](https://github.com/realm/data-adapters/issues/344) and [#47](https://github.com/realm/data-adapters/issues/47), since v1.0.1)
* Fixed a bug where deleting a row and an object simultaneously could lead to the error `object must be of type 'object', got (undefined) processing object`. ([#389](https://github.com/realm/data-adapters/issues/389), since v1.0.0)
* Failed SQL queries can now be retried with an exponential backoff delay. This is configurable via `QueryRetryConfig` in the configuration. ([#305](https://github.com/realm/data-adapters/issues/305))

## Compatibility
* Realm Object Server: 3.0.0 or later
* APIs are backwards compatible with all previous releases in the 1.x.y series.

## Internal
* None.


1.4.2 Release notes (2018-10-26)
================================
## Enhancements
* None.

## Fixed
* Fixes a bug where clients are using the permissions system. The system classes must be ignored (classes prefixed by `__`), but previously the adapter would throw an exception, for example `Error: No property 'undefined' on object of type '__Role'`. ([#356](https://github.com/realm/data-adapters/issues/356), since v1.3.0-rc.2)

## Breaking changes
* None.

## Compatibility
* Realm Object Server: 3.0.0 or later
* APIs are backwards compatible with all previous releases in the 1.x.y series.

## Internal
* None.

1.4.1 Release notes (2018-10-19)
================================

## Fixed
* Added a check to prevent wrong schema. The error message was previously `LINE 1: ...T "deleted_fk" FOREIGN KEY ("deleted") REFERENCES [object Ob...` when the schema was incorrect. Now, an exception will be thrown during adapter start-up. (since v1.0.0)
* Fixed an error about "Unexpected nonnullability" when using the option `createPostgresSchema` with a schema containing object types (foreign keys). Since 1.4.0. [#336](https://github.com/realm/data-adapters/issues/336).

## Compatibility
* Realm Object Server: 3.0.0 or later
* APIs are backwards compatible with all previous releases in the 1.x.y series.

1.4.0 Release notes (2018-10-16)
================================

## Enhancements
* Added `scripts/create-join-tables.js` to help modifying Postgres database when Realm schema has links. ([#336](https://github.com/realm/data-adapters/issues/336))
* Added the option `createPostgresSchema`. When the option is set to true, the adapter will create Postgres tables, columns, and constraints at start-up (before processing any Realm or Postgres changesets). The option can be combined with option `createPostgresDB` if starting from a clean Postgres installation. ([#336](https://github.com/realm/data-adapters/issues/336))
* Added a static method `PostgresAdapter.validate_configuration()` which can check if a `PostgresAdapterConfig` object is valid.

## Fixed
* Fixed a bug so empty Postgres passwords are allowed. (https://realmio.slack.com/archives/C3ESCDT26/p1539213394000200, since v1.0.0)

## Compatibility
* Realm Object Server: 3.0.0 or later
* APIs are backwards compatible with all previous releases in the 1.x.y series.

1.4.0-rc1 Release notes (2018-10-09)
================================
## Compatibility
* Realm Object Server: 3.0.0 or later

## Breaking changes
* None.

## Enhancements
* Added configuration parameter `switchToReferenceRealm` to indicate that Realms must switch reference Realm when uploading to a Realm Object Server. Switching to a reference Realm is required if you are using query-based sync at your clients. The Realm user must have admin rights, and must be logged in using the admin token. ([#256](https://github.com/realm/data-adapters/issues/256))

## Bug Fixes
* Fix a crash when inserting/updating string values with an empty string.
  [#325](https://github.com/realm/data-adapters/pr/305). Since v1.3.0-rc2.

## Internal
* None.

1.3.0 Release notes (2018-10-01)
================================
## Compatibility
* Realm Object Server: 3.0.0 or later

## Breaking changes
* None.

## Enhancements
* None.

## Bug Fixes
* Fixed a crash when linking to a row which has a Postgres primary key and the key is not yet known and the Realm key name is different than the name of the Realm key of the object linking to it. ([#306](https://github.com/realm/data-adapters/issues/306), since v1.3.0-rc2)

## Internal
* None.


1.3.0-rc.2 Release notes (2018-09-28)
================================
### Compatibility
* Realm Object Server: 3.0.0 or later

## Breaking changes
* None.

## Enhancements
* The realm dependency is no longer a peer dependency, it is now a direct dependency so the correct version will be automatically installed with `npm install`.

## Bug Fixes
* Fixed a bug which cause the adapter to seg fault when an event is triggered and the adapter is shut down. In practice, this is only observable in a test environment. ([#280](https://github.com/realm/data-adapters/pull/280), since v1.0.0)
* Fixed a crash when setting links to objects just created in the same transaction. ([#276]([https://github.com/realm/data-adapters/issues/276), since v1.3.0-rc.1)
* Fixed a crash when setting links which have a primary key that contains characters which need to be escaped. ([#276]([https://github.com/realm/data-adapters/issues/276), since v1.3.0-rc.1)
* Fixed a bug which prevented models with links having the same primary key in Realm and Postgres. The adapter would throw an exception like `Error: ERROR: syntax error at or near ","`. ([#297](https://github.com/realm/data-adapters/issues/297), since v1.3.0-rc.1)

## Internal
* Updated node packages.
* realm-sync-server is now a dev dependency so the `npm run install:jssyncserver` step is not necessary for testing.

1.3.0-rc1 Release notes (2018-09-14)
================================
### Compatibility
* Realm Object Server: 3.0.0 or later

## Breaking changes
* None.

## Enhancements
* Added an optional parameter `connect_timeout` to `PostgresConfig` to make it possible to specify value different from the client's default value. This parameter can be used if you experience the error message like `UnhandledPromiseRejectionWarning: Error: could not connect to server: Operation timed out` (#257).
* Added `ADD_PROPERTIES` to ignored instructions (schema changes are processed elsewhere). Unknown instructions are also logged (before the adapter would stop). Partly reported by #271.
* Added support for Postgres version 10 (#167).
* Added support for specifying in `PostgresAdapterConfig` an alternative Postgres schema to search for tables. It is configured by the optional parameter `dbSchema`. The default schema `public` is kept in the search path. An example is: `dbSchema: 'WeAreUsingRealm'`. ([#238](https://github.com/realm/data-adapters/issues/238))

## Bug fixes
* Fixed a bug in property/column mapping when Postgres transactions are processed (#247).
* Fixed a bug when destination to a link has not yet been created (#258). For example, A have a link to B, and A is created before B. Since B does not exist, setting the link is not possible. The fix splits the processing of Postgres transactions into two passes: i) creating objects ii) linking objects. This makes the processing slower as the second pass is duplicating work done in the first pass.
* Fixed a bug where the name of aux. tables for Realm lists can end up being wrong.

## Internal
* Stablizing testing.
* Security updates based on `npm audit` report.

1.2.2 Release notes (2018-08-27)
================================
### Compatibility
* Sync protocol: 24
* Server-side history format: 4
* File format: 7
* Realm Object Server: 3.0.0 or later

## Bug fixes
* Fixed a bug setting the value of a primary key (string is required). The bug would cause the adapter to stop by throwning an exception due to an faulty SQL `UPDATE` statement. The `UPDATE` is updating the value of primary key in Postgres. The `SET` part correctly quotes the value but the `WHERE` clause didn't quote the value if it is a string.
* Fixed loading links to realm objects.


1.2.1 Release notes (2018-08-21)
================================
### Compatibility
* Sync protocol: 24
* Server-side history format: 4
* File format: 7
* Realm Object Server: 3.0.0 or later

## Bug fixes
* Aligned mapping functionality with SQL Server adapter (#233 and #234).

1.2.0 Release notes (2018-08-07)
================================
### Compatibility
* Sync protocol: 24
* Server-side history format: 4
* File format: 7
* Realm Object Server: 3.0.0 or later

## Breaking changes
* None.

## Enhancements
* Added closing of the list cache Realm at shutdown.
* Added mapping of Realm property names (#204).
* Propagate time zone to Postgres (external contribution).
* Added Postgres `real` type (external contribution).

## Bug fixes
* None

## Internal
* None

1.1.1 Release notes (2018-03-22)
================================
### Compatibility
* Sync protocol: 24
* Server-side history format: 4
* File format: 7
* Realm Object Server: 3.0.0 or later

## Breaking changes
* None.

## Enhancements
* Adding options `convertPostgresValueToRealm` and `convertRealmValueToPostgres`.

## Bug fixes
* None.

## Internal
* None.

1.1.0 Release notes (2018-03-15)
================================
## Breaking changes
* None.

## Enhancements
* None.

## Bug fixes
* None.

## Internal
* Updated to Realm JavaScript 2.3.0.
