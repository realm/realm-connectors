2.7.3 Release notes (2019-07-03)
================================
## Enhancements
* None.

## Fixes
* If Realm instructions of more than one type were inserted, only the first type would get the `sqlserverPrimaryKey` populated back to the Realm property. This could be observed if a new Realm object links to another new Realm object of a different type in the same transaction. ([#566](https://github.com/realm/data-adapters/issues/566), since v1.6.0).

## Compatibility
* Realm Object Server: 3.21.0 or later
* APIs are backwards compatible with all previous releases in the 2.x.y series.

2.7.2 Release notes (2019-07-02)
================================
## Enhancements
* None.

## Fixes
* Fix MSSQL inserts causing a crash if the new row goes to a table with a compound primary key which is also a Realm link. Users would see a message similar to `error: Critical exception: {} JS value must be of type 'object', got (3) Error: JS value must be of type 'object', got (3).` ([#568](https://github.com/realm/data-adapters/issues/568), since v1.6.2)

## Compatibility
* Realm Object Server: 3.21.0 or later
* APIs are backwards compatible with all previous releases in the 2.x.y series.

2.7.1 Release notes (2019-06-12)
================================
## Enhancements
* None.

## Fixes
* When performing initial load (SQL Server -> Realm Object Server), the adapter generates a unique value for primary keys for tables where SQL and Realm have separate primary keys. This leads to an `UPDATE` statement, and if `loaderBatchSize` is larger than 1050, this can result in a crash with the error message `The incoming request has too many parameters. The server supports a maximum of 2100 parameters.` ([#561](https://github.com/realm/data-adapters/issues/561), since v1.0.0)

## Compatibility
* Realm Object Server: 3.21.0 or later
* APIs are backwards compatible with all previous releases in the 2.x.y series.

2.7.0 Release notes (2019-05-28)
================================
NOTE: The minimum version of Realm Object Server has been increased to 3.21.0 and attempting to connect to older versions will produce protocol mismatch errors. Realm Cloud has already been upgraded to this version, and users using that do not need to worry about this.

## Enhancements
* Added support for SSL via the [Realm.Sync.SSLConfiguration](https://realm.io/docs/javascript/2.26.0/api/Realm.Sync.html#~SSLConfiguration) configuration ([#180](https://github.com/realm/data-adapters/issues/180)).
* Improved query performance when querying integer properties with indexes e.g., primary key properties. ([realm-core#3272](https://github.com/realm/realm-core/pull/3272))
* Improved write performance when writing changes to disk. ([realm-sync#2927](https://github.com/realm/realm-core/ssues/2927))

## Fixes
* Fixed links for relationships on tables which do not define a `sqlserverPrimaryKey` but use the `primaryKey` as a primary key in both realm and SQL. ([#558](https://github.com/realm/data-adapters/issues/558), since v1.0.0)
* Some queries might have additional parameters, and the SQL Server's limit of 2100 parameters would not be respected. ([#554](https://github.com/realm/data-adapters/issues/554), since v2.5.0)
* Links and lists might have been incompletely loaded if the server's estimate (`sys.dm_db_partition_stats.row_count`) was off by more than the loader's batch size. ([#392](https://github.com/realm/data-adapters/issues/392), since v2.3.0)

## Compatibility
* Realm Object Server: 3.21.0 or later
* APIs are backwards compatible with all previous releases in the 2.x.y series.

2.6.0 Release notes (2019-05-16)
================================
## Enhancements
* Upgrade node mssql version from 4.2.3 to 5.0.5. This brings improved robustness in the MSSQL connection pool. See https://github.com/tediousjs/node-mssql/releases for details.

## Fixes
* When migrating a Realm object to SQL, coverting a binary data property could lead to a crash. (unrelated to but discovered by investigiting [#549](https://github.com/realm/data-adapters/issues/549), since v1.0.0)
* A primary key referenced by a foreign key with non-string data type could lead to a crash with the error message like `No property 'Douglas Adams' on object of type 'Author' when processing object:`. ([#551](https://github.com/realm/data-adapters/issues/551), since v1.0.0)

## Compatibility
* Realm Object Server: 3.11.0 or later
* APIs are backwards compatible with all previous releases in the 2.x.y series.

2.5.1 Release notes (2019-04-08)
================================
## Enhancements
* None.

## Fixes
* When using the configuration option `switchToReferenceRealm` an error was thrown unless an admin token was configured. It's now only required that the user is an admin user. ([#257](https://github.com/realm/data-adapters/pull/527), since v1.6.0)
* Using a column of type link when defining a compound key, could cause the loader/adapter to crash with a critical exception `Invalid type 'undefined' Error: Invalid type 'undefined'` ([#510](https://github.com/realm/data-adapters/issues/510), since v1.0.0).

## Compatibility
* Realm Object Server: 3.11.0 or later
* APIs are backwards compatible with all previous releases in the 2.x.y series.

2.5.0 Release notes (2019-03-26)
================================
## Enhancements
* The validation of the configuration is now more strict which should lead to fewer surprises at runtime. ([#284](https://github.com/realm/data-adapters/issues/284))
* No destructive changes to the Realm schema are allowed as they will lead to a crash at runtime. ([#284](https://github.com/realm/data-adapters/issues/284))

## Fixes
* A crash with the message `The incoming request has too many parameters. The server supports a maximum of 2100 parameters.` could occur if the number of columns in tables multiplied by `adapterSQLBatchSize` were larger than 2100. ([#509](https://github.com/realm/data-adapters/issues/509), since v1.0.0)

## Compatibility
* Realm Object Server: 3.11.0 or later
* APIs are backwards compatible with all previous releases in the 2.x.y series.

2.4.2 Release notes (2019-03-18)
================================
## Enhancements
* None.

## Fixes
* When empty binary data is inserted in Realm, the adapter could crash once it was transferred to SQL. ([#505](https://github.com/realm/data-adapters/issues/505), since v2.1.0)

## Compatibility
* Realm Object Server: 3.11.0 or later
* APIs are backwards compatible with all previous releases in the 2.x.y series.

2.4.1 Release notes (2019-02-27)
================================
## Enhancements
* None.

## Fixes
* Using binary data (the Realm type `data` and SQL Server types `VARBINARY` and `BINARY`) could lead to the error `Validation failed for parameter 'var4'. Invalid buffer.`. ([#505](https://github.com/realm/data-adapters/issues/505), since v2.1.0)

## Compatibility
* Realm Object Server: 3.11.0 or later
* APIs are backwards compatible with all previous releases in the 2.x.y series.

2.4.0 Release notes (2019-02-13)
================================
## Enhancements
* If the SQL schema is changed (adding a column or a table) and the changes are added to the Realm schema, the adapter will now load the new data after a restart. ([#203](https://github.com/realm/data-adapters/issues/203))
* Adding an index to the internal compound key tables to speed up loading time. ([#406](https://github.com/realm/data-adapters/issues/406))

## Fixes
* When running the loader with the setting `trackLoaderState = true` you could get an error `Critical Exception: Error: LoaderState.numRows must be of type 'number'`. ([#442](https://github.com/realm/data-adapters/pull/442), since v2.2.1)
* Having a column available in ROS but not included via the schema configuration of the adapter would cause a critical exception to be thrown when integrating changes from ROS to SQL (`Critical exception: {} TypeError: Cannot read property 'type' of undefined TypeError: Cannot read property 'type' of undefined`). ([#485](https://github.com/realm/data-adapters/issues/485), since v1.0.0)
* When running the loader with a mismatched schema definition between Realm and SQL a critical error could be thrown (`Critical Exception: RequestError: Statement(s) could not be prepared`) but without exiting the loader. ([494](https://github.com/realm/data-adapters/issues/494), since v1.0.0)

## Compatibility
* Realm Object Server: 3.11.0 or later
* APIs are backwards compatible with all previous releases in the 2.x.y series.

2.3.0 Release notes (2019-02-01)
================================
## Enhancements
* Fast table count for the loader ([#392](https://github.com/realm/data-adapters/issues/392), since v1.0.0).

## Fixes
* Querying Realm instances would sometimes pin the read transaction version, resulting in the file rapidly growing in size as further transactions were processed. ([realm/realm-object-store#766](https://github.com/realm/realm-object-store/pull/766), since v1.0.0)

## Compatibility
* Realm Object Server: 3.11.0 or later
* APIs are backwards compatible with all previous releases in the 2.x.y series.


2.2.1 Release notes (2019-01-18)
================================
## Enhancements
* None.

## Fixes
* Fixed a bug where users cannot create loggers using `createFileLogger()` and `createConsoleLogger()`, and they had to rely on either custom or default logger ([#447](https://github.com/realm/data-adapters/pull/477), since v1.8.0)

## Compatibility
* Realm Object Server: 3.11.0 or later
* APIs are backwards compatible with all previous releases in the 2.x.y series.


2.2.0 Release notes (2019-01-17)
================================
## Enhancements
* Added a config option `adapterPollingPeriod` which controls how often the adapter checks for MSSQL changes. ([#446](https://github.com/realm/data-adapters/pull/446))
* Added partial support for re-running the loader against an existing ROS server. All data in MSSQL will overwrite the data in ROS, but objects which exist in ROS but not MSSQL will not be deleted or propagated to MSSQL. This requires loading in online mode. ([#440](https://github.com/realm/data-adapters/pull/440))
* Improved performance and memory usage of `Realm.Sync.Adapter`. ([realm/realm-js-private#501](https://github.com/realm/realm-js-private/pull/501))
* To avoid excessive logging to the Console and hitting this node.js [issue](https://github.com/nodejs/node/issues/6379), a new logger is provided. It logs to a file `realm-adapter-log-YYYY-MM-DD-MM-SS.txt` created in the directory where the Adapter is started. The default logger is unchanged (logging to the Console) but you can switch to the file based logger by adding `logger: createFileLogger(loglevel, filename)` to the configuration. `loglevel` is optional and defaults to `INFO`, and `filename` is optional and defaults to `realm-adapter-log-YYYY-MM-DD-MM-SS.txt`. ([#451](https://github.com/realm/data-adapters/issues/451), since v1.7.0)

## Fixes
* SQL table and column names containing dashes (`-`) and other nonletters were not supported and resulted in a crash with the error message `Incorrect syntax near '-'.`. ([#372](https://github.com/realm/data-adapters/issues/372), since v1.0.0)
* Some errors were causing the default logger to crash when trying to report them. ([#450](https://github.com/realm/data-adapters/issues/450), since v1.7.0)
* Avoid blocking the node.js runloop for extremely long periods of time while loading data from MSSQL to ROS. ([#457](https://github.com/realm/data-adapters/issues/457), since v1.0.0)

## Compatibility
* Realm Object Server: 3.11.0 or later
* APIs are backwards compatible with all previous releases in the 2.x.y series.


2.1.0 Release notes (2018-12-07)
================================
## Enhancements
* Added option `SQLServerAdapterConfig.logSyncClient`. If true, the logs from the connection between the adapter and Realm Object Server will be written to the logger specified by `SQLServerAdapterConfig.logger`. ([#219](https://github.com/realm/data-adapters/issues/219))

## Fixed
* `SQLServerAdapterConfig.loadOnline` configuration option was not set appropriately causing the loader to still upload changes to Realm Object Server if the option was set to `false`. ([#418](https://github.com/realm/data-adapters/issues/418), since v1.2.1-rc28)
* The [logLevel](https://github.com/realm/data-adapters/blob/master/packages/mssql/src/SQLServerAdapterConfig.ts#L217) parameter is passed correctly to the logger when using TypeScript to configure the project, previously it would fallback to `info` level. ([#437](https://github.com/realm/data-adapters/issues/437), since v1.7.0)

## Compatibility
* Realm Object Server: 3.11.0 or later
* APIs are backwards compatible with all previous releases in the 2.x.y series.

2.0.0 Release notes (2018-11-28)
================================
## Enhancements
* Upgraded to Realm JavaScript v2.20.0. You will need to upgrade your Realm Object Server to at least version 3.11.0 or use [Realm Cloud](https://cloud.realm.io). If you try to connect to a ROS v3.10.x or previous, you will see an error like `Wrong protocol version in Sync HTTP request, client protocol version = 25, server protocol version = 24`. ([#391](https://github.com/realm/data-adapters/issues/391))
  - Improved the proactive token refresh mechanism to make several attempts to refresh the token before it expires.
  - Added support for Node 10.
  - A set of bugs that could lead to bad changesets have been fixed. An example of error message is `Failed to parse, or apply received changeset: ndx out of range`.
* MSSQL primary keys (`sqlserverPrimaryKey` properties) are now populated from a query OUTPUT of each insert instead of waiting for changes to "echo" back. ([#224](https://github.com/realm/data-adapters/issues/224))

## Fixed
* Fixed a bug where conflicting writes from both MSSQL and Realm to the same rows/objects would result in the opposite values on either side. Now there is a consistent result (though it is not deterministic which side will "win"). ([#224](https://github.com/realm/data-adapters/issues/224), since v1.0.0)
* Fixed a crash when processing a realm instruction for a type which is not included in the adapter schema. Instructions for an unknown type are now ignored ([#402](https://github.com/realm/data-adapters/issues/402), since v1.2.1-rc16)
* Fixed Realm objects becoming orphans when SQL delete events occur quickly after Realm inserts object but before the SQL generated primary keys are echoed back to Realm. This is no longer an issue because SQL id's are populated immediately from "INSERT" statements so there is no "echo" period where the realm objects do not have their `sqlserverPrimaryKey` property populated. ([#199](https://github.com/realm/data-adapters/issues/199), since v1.0.0)
* Added a workaround to get stack trace progagated to the log. ([#397](https://github.com/realm/data-adapters/issues/397), since v1.7.0)

## Breaking changes
* Removed the config option `applyRealmSchemaChangesToSQLServer`. Adding a Realm class which is not part of the adapter schema would cause the adapter to crash. Now, all instructions which relate to classes which are not defined in the adapter schema are ignored. If additional classes should be handled by the adapter, add them manually to MSSQL, then add the corresponding definitions to the adapter schema and reload. If you need help creating the Realm schema, use the schema generator (scripts/realm_schema_generator/index.js). ([#402](https://github.com/realm/data-adapters/issues/402), since v1.0.0)

## Compatibility
* Realm Object Server: 3.11.0 or later
* APIs are backwards compatible with all previous releases in the 1.x.y series.

## Internal
* Cleaned up distribution package. ([#405](https://github.com/realm/data-adapters/pull/405))
* Upgraded to Realm JavaScript v2.20.0.
* Upgraded our `mssql` dependency from 4.2.1 to 4.2.3. See https://github.com/tediousjs/node-mssql/releases for details. One noticeable change is that the upgrade fixes the following deprecation warning which used to be printed every time: "tedious deprecated The default value for `options.encrypt` will change from `false` to `true`. Please pass `false` explicitly if you want to retain current behaviour. node_modules/mssql/lib/tedious.js:230:23". ([#224](https://github.com/realm/data-adapters/issues/224))


1.7.0 Release notes (2018-11-21)
================================

## Enhancements
* Added an optional custom logger named `SQLServerAdapterConfig.logger`. It uses [winston](https://github.com/winstonjs/winston). The default logger will log to the console with a log level set to `info` (see winston for details). An example of the output is `2018-11-09T13:57:28.425Z [REALM ADAPTER] info: Finished processing table MyPerson. Imported 4 rows in 0:00:00.1 (40 rows/second)`. ([#303](https://github.com/realm/data-adapters/issues/303))

## Fixed
* Fixed a bug where column name and property name are different in a compound key. The adapter stopped with the error message `Error: No property 'BAR' on object of type 'foo' processing object`. ([#378](https://github.com/realm/data-adapters/issues/378), since v1.5.3-rc1)
* Added a small performance optimization to the compound key cache. ([#379](https://github.com/realm/data-adapters/issues/379), since v1.6.2)
* When loading data into ROS for a second time without wiping ROS data, objects with a compound primary key would end up being duplicated with only the realm primary key changing. The fix does not affect performance of the first load. ([#383](https://github.com/realm/data-adapters/pull/383), since v1.0.0)
* Fixed a missing translation through the config mapping of property name to column name when setting a Realm property quickly after inserting it on a table which has a `sqlserverPrimaryKey` set. ([#383](https://github.com/realm/data-adapters/pull/383), since v1.6.0)
* Fixed a bug where deleting a row with an integer primary key would crash the adapter. ([#390](https://github.com/realm/data-adapters/pull/390), since v1.0.0)
* Fixed a bug where deleting a row and an object simultaneously could lead to the error `object must be of type 'object', got (undefined) processing object`. ([#389](https://github.com/realm/data-adapters/issues/389), since v1.0.0)

## Compatibility
* Realm Object Server: 3.0.0 or later
* APIs are backwards compatible with all previous releases in the 1.x.y series.

## Internal
* Loading compound keys which include a date is slightly faster. ([#383](https://github.com/realm/data-adapters/pull/383))


1.6.3-rc1 Release notes (2018-11-09)
================================

## Fixed
* Better robustness in the compound key cache in case of corruption.
* If the compound key cache (internal realm tables) already contained an object but an insertion operation was processed again, an error is no longer thrown. This could happen if the adapter is restarted and processes an insert operation a second time. The error looked like: `Attempting to create an object of type '__CompoundKeyMapping__MyObject' with an existing primary key value ''some_id''.` (since v1.6.2).

## Compatibility
* Realm Object Server: 3.0.0 or later
* APIs are backwards compatible with all previous releases in the 1.x.y series.

## Internal
* Added an `AdapterMonitor` for more robust tests.
* The adapter's `onceEvent` no longer destroys all listeners when an event occurs, now individual listeners are cleaned up as needed.


1.6.2 Release notes (2018-11-02)
================================

## Fixed
* Fixed compound keys assuming that the MSSQL database has a column for storing realm primary keys when using compound keys. Now translation between realm primary keys and MSSQL compound keys is done using a cache in Realm. This redesign changes the migration function to version 3 - a reload is not required. ([#365](https://github.com/realm/data-adapters/issues/365), since v1.6.1).

## Compatibility
* Realm Object Server: 3.0.0 or later
* APIs are backwards compatible with all previous releases in the 1.x.y series.


1.6.1 Release notes (2018-10-31)
================================

## Enhancements
* None.

## Fixed
* In an object which has a compound primary key, the value of the realm primary key no longer matters. Previously, apps adding objects to a model which has a compound primary key in the adapter must use an internal format for the primary key. (Since v1.0)
* Changes to compound primary key properties are now allowed from Realm events, these changes would previously be ignored by the adapter. (Since v1.0)
* The migration function no longer changes the primary key of an object which has a compound primary key, this nullified links and lists. ([#349](https://github.com/realm/data-adapters/issues/349), since v1.6.1-rc2)

## Compatibility
* Realm Object Server: 3.0.0 or later
* APIs are backwards compatible with all previous releases in the 1.x.y series.

## Internal
* Better config checking to explicitly disallow schemas which have links to objects with compound primary keys. Unsupported since v1.0.


1.6.1-rc2 Release notes (2018-10-24)
====================================

## Enhancements
* None.

## Fixed
* Fixes a bug in handling compound primary keys. Previously, the adapter would concatenate the values using hyphen (`-`). This issue is that if one value contains a hyphen, it is not possible to decode the Realm primary key to the SQL compound key. Instead, each value is hex encoded before concatenating them. This ensures that decoding can be done at the expense of the readability of the Realm primary key. Migration of previous compound primary key format is done automatically if necessary. ([#349](https://github.com/realm/data-adapters/issues/349), since v1.0.0).

## Compatibility
* Realm Object Server: 3.0.0 or later
* APIs are backwards compatible with all previous releases in the 1.x.y series.

## Internal
* Now storing load and adapter versions locally for each run for future compatibility.


1.6.1-rc1 Release notes (2018-10-12)
================================

## Enhancements
* None.

## Fixed
* If a critical exception occurs, but something other than an `Error` object is thrown, print it as a string instead of 'undefined'. Otherwise critical exceptions could be difficult to track down. Since 1.0.0. [#329](https://github.com/realm/data-adapters/issues/329).
* Running the adapter after a load would cause events generated in the load to be processed again. This could cause various errors such as duplicate primary key insertions which SQL would reject. Since 1.5.0 when realm-js became a direct dependency. [#333](https://github.com/realm/data-adapters/issues/333).
* Better error handling from Realm event generated errors.

## Compatibility
* Realm Object Server: 3.0.0 or later
* APIs are backwards compatible with all previous releases in the 1.x.y series.


1.6.0 Release notes (2018-10-05)
================================

## Enhancements
* Added configuration parameter `switchToReferenceRealm` to indicate that Realms must switch reference Realm when uploading to a Realm Object Server. Switching to a reference Realm is required if you are using query-based sync at your clients. The Realm user must have admin rights, and must be logged in using the admin token. ([#256](https://github.com/realm/data-adapters/issues/256))

## Fixed
* Fix crashes in the adapter when Realm links referenced a newly created object without an assigned MSSQL primary key.
  (Issue #293)[https://github.com/realm/data-adapters/issues/293]. Since 1.0.
* Fix crashes which could occur if a Realm object was created and then deleted quickly. Since 1.0.
* Clean up stale link property tables after a CLEAR event triggered by deleting all objects of a type from Realm. Since 1.0.
* Added type to configuration parameter `ignoreSQLerrors`. (since v1.5.0)

## Compatibility
* Realm Object Server: 3.0.0 or later

## Internal
* Removed an old config option which did not work and does not make sense `mapRealmChangeToSQLServerTable`.


1.5.0 Release notes (2018-10-01)
================================

## Enhancements
* The impact of Realm objects which cannot be inserted into SQL depends on the setting of the configuration parameter `ignoreSQLerrors`. If set to false, the adapter will stop at SQL errors. This is useful during development. Setting the parameter `ignoreSQLerrors` to true or not setting it, the adapter will ignore these objects, but write the error to the log. ([#190](https://github.com/realm/data-adapters/issues/190))
* The realm dependency is no longer a peer dependency, it is now a direct dependency so the correct version will be automatically installed with `npm install`.
* Added a new optional configuration parameter `realmIgnoreProperties` to mark which properties in Realm to ignore (as they might not be defined in SQL). ([#200](https://github.com/realm/data-adapters/issues/200))
* Added support for lists of objects.

## Fixed
* Fixed a crash when loading a table which has link properties which are part of a compound key. ([#294](https://github.com/realm/data-adapters/pull/294), since v1.2.1-rc.23)

## Internal
* Updated node packages.
* realm-sync-server is now a dev dependency so the `npm run install:jssyncserver` step is not necessary for testing.


1.4.1-rc1 Release notes (2018-09-20)
====================================

## Enhancements
* None.

## Bug fixes
* Fix errors when the sql database has multiple schemas with similar tables.
  (Issue #289)[https://github.com/realm/data-adapters/issues/289].

## Compatibility
* Realm Object Server: 3.0.0 or later
* Requries realm js version > 2.10.0


1.4.0 Release notes (2018-09-17)
================================

## Enhancements
* None.

## Bug fixes
* Fixed a bug which cause the adapter to seg fault when an event is triggered and the adapter is shut down. (#280).

## Compatibility
* Realm Object Server: 3.0.0 or later
* Requries realm js version > 2.10.0


1.4.0-rc.1 Release notes (2018-09-12)
================================

## Enchancements
* Updating the value of a compound primary key is not supported, and instead of silently ignoring it, it is now logged as an error.  An example of the error message is: `Updating a primary key (class "MyPerson") is not supported and will be ignored: {"birthday":"2018-09-06T15:14:14.926Z","name":"Jim"}`.

## Bug fixes
* Fixed a bug where the opposite mapping function was applied to a class name. If the Realm class name is A and the SQL table name is B, A will not be mapped to B and the adapter will crash due to a missing SQL table (since A does not exist).

## Compatibility
* Realm Object Server: 3.0.0 or later
* Requries realm js version > 2.10.0


1.3.0 Release notes (2018-08-27)
================================

## Enhancements
* Upload to ROS after loading will wait for a specified time `loaderUploadMaxWaitTime` (default 60 seconds) before stopping.
* Support loading an integer column into a realm boolean type, 0/1 conversion to false/true (> 1 is true).

## BugFixes
* Fixed SQL syntax of loading and populating primary keys.
* Fixed SQL syntax to allow naming columns SQL reserved words.

## Compatibility
* Realm Object Server: 3.0.0 or later
* Requries realm js version > 2.10.0

## Internals
* Now including a mocha test suite!

1.2.1-rc28 Release notes (2018-07-18)
================================
## Compatibility
* Realm Object Server: 3.0.0 or later
* Requries realm js version > 2.10.0

## BugFixes
* Add retry logic when requests timeout when fetching the current tracking version.
* Terminate the process when an exception occurs during fetching changes, so it can be restarted by pm2.

1.2.1-rc27 Release notes (2018-07-18)
================================
## Compatibility
* Realm Object Server: 3.0.0 or later
* Requries realm js version > 2.10.0

## Enhancements
* Adds a new config parameter `adapterSQLBatchSize`

## BugFixes
* Batch changes from Realm to SQL and retry on timeouts.

## Internals
* Better logging on Realm to SQL changes.
* Relieve polling pressure on SQL to Realm changes.

1.2.1-rc26 Release notes (2018-07-16)
================================
## Compatibility
* Realm Object Server: 3.0.0 or later
* Requries realm js version > 2.10.0

## BugFixes
* Holding the adapter transaction open while processing changes could
  lead to connection timeouts. Now we use two transactions and write
  data to Realm locally without holding one open.

## Internals
* Improve performance of adapter change processing.

1.2.1-rc25 Release notes (2018-07-13)
================================
## Compatibility
* Sync protocol: 24
* Server-side history format: 4
* File format: 7
* Realm Object Server: 3.0.0 or later
* Requries realm js version > 2.10.0

## BugFixes
* Fix update/delete on objects with a property mapping
* Supporting updates on objects with a list property
* Better string sanitization

1.2.1-rc24 Release notes (2018-07-10)
================================
## Compatibility
* Sync protocol: 24
* Server-side history format: 4
* File format: 7
* Realm Object Server: 3.0.0 or later
* Requries realm js version > 2.10.0

## Breaking changes
* Use a less strict config hash not including the realm schema.
  Upgrades from earlier versions will happen automatically.
  Downgrading from this version is not supported without reloading.
* An undefined compaction interval of the loader config now means never stop for compaction.
  Previously this would mean to use a default of 100 batches.

## Enhancements
* Adds a new parameter to the loader to determine if the load should be online or offline of ROS.

## BugFixes
* Paused and continued loads use the correct change tracking version.
  Previously, changes made to SQL between pauses could be lost.
* Fix a crash in the adapter introduced in rc23 when processing changes on a
  object containing a link.
* Support splitting data to different realm files if specified in the config.

## Internals
* Improved progress reporting of the loader.
* Compact less often, only when a meaningful space improvement is expected.
* Performance enchancements to the adapter.

1.2.1-rc23 Release notes (2018-06-25)
================================
## Compatibility
* Sync protocol: 24
* Server-side history format: 4
* File format: 7
* Realm Object Server: 3.0.0 or later
* Requries realm js version > 2.10.0

## Internals
* More optimisations to the loader including:
  - loading indpendently of ROS, sync will occur when the adapter starts
  - only querying SQL for the required fields
  - reduced internal data transfomation paths
* Support for (unused) one to many relationships is disabled

1.2.1-rc22 Release notes (2018-06-22)
================================
## Compatibility
* Sync protocol: 24
* Server-side history format: 4
* File format: 7
* Realm Object Server: 3.0.0 or later

## Internals
* Several speed optimisations to the load stage including streaming queries.

1.2.1-rc21 Release notes (2018-06-19)
================================
## Compatibility
* Sync protocol: 24
* Server-side history format: 4
* File format: 7
* Realm Object Server: 3.0.0 or later

## Enhancement
* Add per batch rows/second print to the loader.

1.2.1-rc20 Release notes (2018-06-19)
================================
## Compatibility
* Sync protocol: 24
* Server-side history format: 4
* File format: 7
* Realm Object Server: 3.0.0 or later

## Enhancement
* Periodically compact the local loader Realm during loading.

1.2.1-rc19 Release notes (2018-06-18)
================================
## Compatibility
* Sync protocol: 24
* Server-side history format: 4
* File format: 7
* Realm Object Server: 3.0.0 or later

## Bug fixes
* Fix the adapter not advancing after recieving a changeset which is all metadata.

1.2.1-rc18 Release notes (2018-06-16)
================================
## Compatibility
* Sync protocol: 24
* Server-side history format: 4
* File format: 7
* Realm Object Server: 3.0.0 or later

## Bug fixes
* Allow the loader to handle tables with 0 rows.

1.2.1-rc17 Release notes (2018-06-16)
================================
## Compatibility
* Sync protocol: 24
* Server-side history format: 4
* File format: 7
* Realm Object Server: 3.0.0 or later

## Enhancements
* Add time to completion estimation to the loader

## Bug fixes
* Fix update errors on instructions that would get processed multiple times
  if the schema contained more than one of the same property name.
* Fix state loading of lists.

1.2.1-rc16 Release notes (2018-06-12)
================================
## Compatibility
* Sync protocol: 24
* Server-side history format: 4
* File format: 7
* Realm Object Server: 3.0.0 or later

## Bug fixes
* Fix a bug causing the LRU cache of rc14 not to work properly.

1.2.1-rc15 Release notes (2018-06-11)
================================
## Compatibility
* Sync protocol: 24
* Server-side history format: 4
* File format: 7
* Realm Object Server: 3.0.0 or later

## Bug fixes
* Do not write redundant information in the link stage.
* Fix local storage sometimes causing exceptions, by keeping results up to date.

1.2.1-rc14 Release notes (2018-06-07)
================================
## Compatibility
* Sync protocol: 24
* Server-side history format: 4
* File format: 7
* Realm Object Server: 3.0.0 or later

## Bug fixes
* Store change tracking versions in a fault tolerant Realm file rather than with node-persist.
* Use a LRU cache of realm files instead of closing every time, should improve loading times.
* Add an optional parameter to the config `realmWriterCacheSize`.
* Fix a loader speed regression introduced in rc10.

1.2.1-rc13 Release notes (2018-05-31)
================================
## Compatibility
* Sync protocol: 24
* Server-side history format: 4
* File format: 7
* Realm Object Server: 3.0.0 or later

## Bug fixes
* Ignore changes to query based sync metadata emitted from the adapter.

1.2.1-rc12 Release notes (2018-05-24)
================================
## Compatibility
* Sync protocol: 24
* Server-side history format: 4
* File format: 7
* Realm Object Server: 3.0.0 or later

## Enhancements
* Added state tracking to the loader, available by a new optional loader option `trackLoaderState`
  It will allow consecutive loads to resume where a previous load left off (per table).

## Internals
* Some optimisations to the loader speed.

1.2.1-rc11 Release notes (2018-05-22)
================================
## Compatibility
* Sync protocol: 24
* Server-side history format: 4
* File format: 7
* Realm Object Server: 3.0.0 or later

## Bug fixes
* Fix a problem in 1.2.1-rc10 where empty changes on a table would cause an error.

1.2.1-rc10 Release notes (2018-05-22)
================================
## Compatibility
* Sync protocol: 24
* Server-side history format: 4
* File format: 7
* Realm Object Server: 3.0.0 or later

## Bug fixes
* Force the loader to wait for upload success before continuing to the next batch.
  This may solve some out of memory problems, and will fix the loader sometimes closing before sync finishes.

1.2.1-rc9 Release notes (2018-05-18)
================================
## Compatibility
* Sync protocol: 24
* Server-side history format: 4
* File format: 7
* Realm Object Server: 3.0.0 or later

## Bug fixes
* Fix the adapter not working in cases where identical tables exist in a
  different database schema.

1.2.1-rc8 Release notes (2018-05-16)
================================
## Compatibility
* Sync protocol: 24
* Server-side history format: 4
* File format: 7
* Realm Object Server: 3.0.0 or later

## Bug fixes
* Fix query error if both primaryKey and sqlserverPrimaryKey are the same.
* If initializeRealmFromSQLServer is set to true, throw a descriptive error
  instead of doing nothing.
* Fix incorrect column mappings when setting realm PKs back to SQL.
* Fix dates in compound PKs causing updates and inserts from ROS->SQL to be ignored.

1.2.1-rc7 Release notes (2018-05-14)
================================
## Compatibility
* Sync protocol: 24
* Server-side history format: 4
* File format: 7
* Realm Object Server: 3.0.0 or later

## Bug fixes
* Fix `mapSQLServerTableName` and `mapRealmClassName`.

1.2.1-rc6 Release notes (2018-05-09)
================================
## Compatibility
* Sync protocol: 24
* Server-side history format: 4
* File format: 7
* Realm Object Server: 3.0.0 or later

## Bug fixes
* Fix compound primary keys not working on case sensitive servers in the adapter.
* Fix adapter crashing when negative numbers are used in a compound primary key.

1.2.1-rc5 Release notes (2018-05-08)
================================
* This release contains changes which were insufficient and should not be used.

1.2.1-rc4 Release notes (2018-05-07)
================================
## Compatibility
* Sync protocol: 24
* Server-side history format: 4
* File format: 7
* Realm Object Server: 3.0.0 or later

## Bug fixes
* Fix case sensitive issue with "CONTEXT" in the adapter.

1.2.1-rc3 Release notes (2018-05-07)
================================
## Compatibility
* Sync protocol: 24
* Server-side history format: 4
* File format: 7
* Realm Object Server: 3.0.0 or later

## Internal
* Heavily optimise the loader when processing tables with links.
  This is done by requesting the realm PK from SQL instead of
  searching through all objects locally for the SQL PK.

1.2.1-rc2 Release notes (2018-05-04)
================================
## Compatibility
* Sync protocol: 24
* Server-side history format: 4
* File format: 7
* Realm Object Server: 3.0.0 or later

## Internal
* Loader optimisations:
  - Don't do a second pass on tables which don't have links or lists.
  - Process one realm file at a time, and close it when finished,
    This saves memory for designs which write to many realm files.

1.2.1-rc1 Release notes (2018-04-24)
================================
## Compatibility
* Sync protocol: 24
* Server-side history format: 4
* File format: 7
* Realm Object Server: 3.0.0 or later

## Enhancements
* Add an optional filter function for data going from SQL to ROS.

1.2.0 Release notes (2018-04-24)
================================
## Compatibility
* Sync protocol: 24
* Server-side history format: 4
* File format: 7
* Realm Object Server: 3.0.0 or later

## Breaking changes
* None.

## Enhancements
* Performance improvements on the loader by increasing the batch size. The number is now tuneable
  by setting `loaderSQLBatchSize` in the config.

## Bug fixes
* Fix the check for change tracking on some databases which have failed over, using a different database id.
* Changed string escaping character to make it compatible with MSSQL rather than MySQL.
* Checking if the columns beloging to a compound key don't already exist on the "names" and "values" arrays before re-adding them.
* Replacing realm FKs (RealmIDs) with MSSQL FKs (SQL ids) when inserting an object from the Realm into MSSQL.
* Propagating list changes from MSSQL to Realm on UPDATEs.
* Inserting list changes only when the foreign key is filled.
* Preventing SQL malformation due to commas on change tracking SQLs.

## Internal
* None.

1.1.2 Release notes (2018-04-18)
================================
## Compatibility
* Sync protocol: 24
* Server-side history format: 4
* File format: 7
* Realm Object Server: 3.0.0 or later

## Breaking changes
* None.

## Enhancements
* Added support for binary data type (#130).
* Consistent use for upper case in SQL statements to help databases with a case sensitive collation (#152).

## Bug fixes
* Added `mapSQLServerTableName()` and `mapSQLServerColumnName()` to a number of missed places.

## Internal
* None.

1.1.1 Release notes (2018-03-22)
================================
## Compatibility
* Sync protocol: 24
* Server-side history format: 4
* File format: 7
* Realm Object Server: 3.0.0 or later

## Breaking changes
* None.

## Enhancements
* Adding options `convertSQLServerValueToRealm` and `convertRealmValueToSQLServer`.

## Bug fixes
* None.

## Internal
* None.

1.1.0 Release notes (2018-03-15)
================================
## Breaking changes
* None.

## Enhancements
* Added `mapSQLServerTableName` and `mapSQLServerColumnName` to help mapping table and column names (#129).

## Bug fixes
* None.

## Internal
* Updated to Realm JavaScript 2.3.0.
