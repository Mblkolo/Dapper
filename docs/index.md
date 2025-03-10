# Dapper - a simple object mapper for .NET

## Overview

A brief guide is available [on github](https://github.com/DapperLib/Dapper/blob/main/Readme.md)

Questions on Stack Overflow should be tagged [`dapper`](https://stackoverflow.com/questions/tagged/dapper)

## Installation

From NuGet:

    Install-Package Dapper

or

    Install-Package Dapper.StrongName

Note: to get the latest pre-release build, add ` -Pre` to the end of the command.

## Release  Notes

### unreleased

(note: new PRs will not be merged until they add release note wording here)

### 2.0.143

- add missing non-generic `AsyncEnumerable<dynamic> QueryUnbufferedAsync(...)` API (#1925 via mgravell, fixes #1922)
- formally mark all `struct` types as `readonly` (#1925 via mgravell)
- reinstate fallback support for `IDataReader`, and implement missing `DbDataReader` async APIs (#1913 via mgravell)

### 2.0.138

- (#1910 via mgravell, fix #1907, #1263)
  - add support for `SqlDecimal` and other types that need to be accessed via `DbDataReader.GetFieldValue<T>`
  - add an overload of `AddTypeMap` that supports `DbDataReader.GetFieldValue<T>` for additional types
  - acknowledge that in reality we only support `DbDataReader`; this has been true (via `DbConnection`) for `async` forever
- (#1912 via mgravell)
  - add missing `AsyncEnumerable<T> QueryUnbufferedAsync<T>(...)` and `GridReader.ReadUnbufferedAsync<T>(...)` APIs (.NET 5 and later)
  - implement `IAsyncDisposable` on `GridReader` (.NET 5 and later)

### 2.0.123

- Parameters can now be re-used on subsequent commands (#952 via jamescrowley)
- Array query support (`.Query<int[]>`) on supported platforms (e.g. Postgres) (#1598 via DarkWanderer)
- `SqlMapper.HasTypeHandler` is made public for consumers (#1405 via brendangooden)
- Improves multi-mapping error message when a specified column in splitOn can't be found (#1664 via NickCraver)
- Improves `DbString.ToString()` (#1665 via NickCraver)
- `DbType` for date/time types is no longer explicitly specified (resolves `Npgsql` v6 issue)
- add `Settings.UseIncrementalPseudoPositionalParameterNames`, to support "snowflake" parameter naming conventions

### 2.0.90

- logo added; license updated to mention logo usage (via mgravell)
- moved to DapperLib org; links updated (#1656)
- RepoDb benchmark added (#1626 via stevedesmond-ca)
- excise unrelated Soma tests (#1642 via kant2002)
- SqlMarshl benchmark added (#1646 via kant2002)
- documentation fixes (#1615 via Rollerss, #1604 via GitHubPang)

### 2.0.78

- fix `DynamicParameters` loop bug - wrong index (#1443 via DamirAinullin)
- fix nullable tuple handling (#1400 via JulianRooze)
- support update set in `SqlBuilder` (#1404 via Wei)
- initialize collections with counts when possible (#1449 via DamirAinullin)
- general code cleanup (#1452, #1457, #1458, #1459 all via DamirAinullin)
- C# 9 and .NET 5 preparation/cleanup (#1572 via mgravell)
- GitHub action, Docker, AppVeyor work (build/test) work (#1563 via Tyrrrz, #1559 via craver, #1450 via craver)
- Test project rationalization (#1556 via craver)
- ClickHouse detection (#1462 via DarkWanderer)
- Switched to "main" branch (via craver)
- documentation fixed (#1596 via royal, #1560 via wswind, #1558 via paul42, #1507 via imba-tjd, #1508 via dogac00, 899c9feb via BlackjacketMack, 0b17133 via BlackjacketMack, 9b6c8c7d via bryancrosby, #1202 via craver)
- MightyOrm benchmark added (455b3f3b via cdonnellytx)
- EF6 performance test updates (#1361 via AlexBagnolini)

### 2.0.35

- build tooling: enable "deterministic builds" and enable SDK roll-foward
- fix culture related formatting/parsing issue with Sqlite (#1363 via sebastienros)
- documentation fixes (#1357 via jawn)
- add tests for `SqlBuilder` (#1369 via shps951023)

### 2.0.30

- upstream library updates; project (build) cleanup
- reinstated net461 build target
- add Dapper.ProviderTools library (to help with System vs Microsoft SqlClient migration, etc)
- fix double dictionary lookup (#1339 via DamirAinullin)
- fix bug with dynamic parameters accessing the wrong member (#1334 via DamirAinullin)
- fix explicit-key issue with `DeleteAsync` (#1309 via james-hester-ah)
- fix for `char` on Postgres (#1326 via jjonescz)
- documentation fixes (#1340 via jawn)
- test and benchmark fixes (#1337 via DamirAinullin, #1206 via yesmey, #1331 via andresrsanchez, #1335 via DamirAinullin)

### 2.0.4

Primary changes:

- remove the System.Data.SqlClient dependency, allowing consumers to use System.Data.SqlClient or Microsoft.Data.SqlClient (or neither, or both) as they choose
 - this means that some users may need to *re-add* one of the above as a `<PackageReference>` for their project to build, if they were previously relying on Dapper to provide System.Data.SqlClient
 - the `AsTableValuedParameter(this IEnumerable<SqlDataRecord>)` extension method is now `AsTableValuedParameter<T>(this IEnumerable<T>) where T : IDataRecord`; this is a breaking change but should be code-compatible and just requires a rebuild
- unify the target platform at NetStandard2.0 (and .NET Framework 4.6.2 for the EF DB geometry/geography types)
- fix bug with `Identity` not enforcing type identity of multi-mapped types

Other changes merged:

- fix #1242, #1280, #1282 - fix value-tuple mapping
- fix #1295 - add `ExecuteReaderAsync` overload to expose `DbDataReader`
- fix #569 - handing of `IN` and similar clauses in some scenarios
- fix #1256 - make `Dispose()` polymorphic in "rainbow"
- fix #1257 - make the `.Connection` available in "rainbow"

### 1.60.6

- improve performance of descriptor API

### 1.60.5

- add descriptor API to `DapperRow` (enables UI binding with non-generic `Query()` API)

### 1.60.1

- Fix [#1196](https://github.com/DapperLib/Dapper/issues/1196) - versioning fix only ([#1198](https://github.com/DapperLib/Dapper/pull/1198)) - assembly version is now locked at 1.60.0 to resolve some mismatch issues with .NET Core assembly loading/binding.

### 1.50.7

- Fix [#1190](https://github.com/DapperLib/Dapper/issues/1190) - incorrect unmanaged pointer when processing parameters that are a boxed struct (rare error relating to GC)
- Fix [#1111](https://github.com/DapperLib/Dapper/issues/1111) - make `SqlMapper.Parse` consistent with `QueryImpl`
- Fix #111- - improve error message for invalid literal types
- Fix [#1149](https://github.com/DapperLib/Dapper/pull/1149) - improve error messages in "contrib"
- Improved detection of empty table-valued-parameters

### 1.50.5

- Fixes empty result set hanging with `QueryAsync`
- `DapperRow` now implements `IReadOnlyDictionary<string, object>`
- Improved error messages for `Async` when the provided `IDbConnection` is not a `DbConnection`
- Contrib: `GetAll` now handles nullable types

### 1.50.4

- Added back missing .NET Standard functionality (restored in `netstandard2.0`)
- Bumped `SqlClient` dependency to 4.4.0 (to help propagate the newer client)

### 1.50.2

- Fix issue [#569](https://github.com/DapperLib/Dapper/issues/569) (`in` expansions using ODBC pseudo-positional arguments)

### 1.50.1

- Change to how `string_split` is used for `InListStringSplitCount`

### 1.50.0

- No changes; stable release

### 1.50.0-rc3

- Updated for .Net Core RTM package dependencies

### 1.50.0-rc2b

- New `InListStringSplitCount` global setting; if set (non-negative), `in @foo` expansions (of at least the specified size) of primitive types (`int`, `tinyint`, `smallint`, `bigint`) are implemented via the SQL Server 2016 (compat level 130) `STRING_SPLIT` function
- Fix for incorrect conversions in `GridReader` ([#254](https://github.com/DapperLib/Dapper/issues/254))

### 1.50.0-rc2 / 1.50.0-rc2a

- Packaging for .NET Core rc2

### 1.50-beta9

- Fix for `PadListExpansions` to work correctly with `not in` scenarios; now uses last non-null value instead of `null`; if none available, don't pad
- Fix problems with single-result/single-row not being supported by all providers (basically: sqlite, [#466](https://github.com/DapperLib/Dapper/issues/466))
- Fix problems with enums - nulls ([#467](https://github.com/DapperLib/Dapper/issues/467)) and primitive values ([#468](https://github.com/DapperLib/Dapper/issues/468))
- Add support for C# 6 get-only properties ([#473](https://github.com/DapperLib/Dapper/issues/473))
- Add support for various xml types ([#427](https://github.com/DapperLib/Dapper/issues/427))

### 1.50-beta8

- Addition of `GetRowParser<T>` extension method on `IDataReader` API - allows manual construction of discriminated unions, etc
- Addition of `Settings.PadListExpansions` - reduces query-plan saturation by padding list expansions with `null` values (opt-in, because on some DB configurations this could change the meaning) *(note: bad choice of `null` revised in 1.50-beta9)*
- Addition of `Settings.ApplyNullValues` - assigns (rather than ignores) `null` values when possible
- Fix for [#461](https://github.com/DapperLib/Dapper/issues/461) - ensure type-handlers work for constructor-based initialization
- Fix for [#455](https://github.com/DapperLib/Dapper/issues/455) - make the `LookupDbType` method available again

### 1.50-beta7

- Addition of `GetRowParser(Type)` (and refactor the backing store for readers to suit)
- Column hash should consider type, not just name

### 1.50-beta6

- Fix for issue [#424](https://github.com/DapperLib/Dapper/issues/424) - defensive `SqlDataRecord` handling

### 1.50-beta5

- Add "single", "first", "single or default" to complement the "first or default" options from 1.50-beta4
- Use single-row/single-result when possible
- Fix for proxy-generator (issue #361)

### 1.50-beta4

- Add `QueryFirstOrDefault` / `ReadFirstOrDefault` methods that optimize the single-row scenario
- Remove some legacy `dynamic` usage from the async API
- Make `DynamicTypeMap` public again (error during core-clr migration)
- Use `Hashtable` again on core-clr

### 1.50-beta3

- Core CLR support: add explicit `dnx451` support in addition to `dotnet5.4` (aka `netstandard1.4`)

### 1.50-beta2

- Core CLR now targets rc1 / 23516
- Various Core CLR fixes
- Code cleanup and C# 6 usage (assorted)

### 1.50-beta1

- Split `SqlMapper.cs` as it was becoming too unmaintainable; NuGet is now the only supported deployment channel
- Remove down-level C# requirements, as "drop in the file" is no longer the expected usage
- `SqlMapper.Settings` added; provides high-level global configuration; initially `CommandTimeout` (@Irrational86)
- improve error message if an array is used as a parameter in an invalid context
- Add `Type[]` support for `GridReader.Read` scenarios (@NikolayGlynchak)
- Support for custom type-maps in collection parameters (@gjsduarte)
- Fix incorrect cast in `QueryAsync<T>` (@phnx47, [#346](https://github.com/DapperLib/Dapper/issues/346))
- Fix incorrect null handling re `UdtTypeName` (@perliedman)
- Support for `SqlDataRecord` (@sqmgh)
- Allow `DbString` default for `IsAnsi` to be specified (@kppullin)
- provide `TypeMapProvider` with lazy func-based initialization (@garyhuntddn)
- Core-clr updated to beta-8 and various cleanups/fixes
- Built using core-clr build tools


### 1.42

- Fix bug with dynamic parameters where `.Get<T>` is called before the command is executed

### 1.41-beta5

- Core-clr packaging build and workarounds
- Fix bug with literal `{=val}` boolean replacements

### 1.41-beta4

- Core-clr packaging build
- Improve mapping to enum members (@BrianJolly)

### 1.41-beta

- Core-clr packaging build

### 1.41-alpha

- Introduces dnx (core-clr) experimental changes
- Adds `SqlBuilder` project
- Improve error message when incorrectly accessing parameter values

### 1.40

- Workaround for broken `GetValues()` on Mono; add `AsList()`

### 1.39

- Fix case on SQL CLR types; grid-reader should respect no-cache flags; make parameter inclusion case-insensitive

### 1.38

- Specify constructor explicitly; allow value-type parameters (albeit: boxed)

### 1.37

- Reuse StringBuilder instances when possible (list parameters in particular)

### 1.36

- Fix Issue [#192](https://github.com/DapperLib/Dapper/issues/192) (expanded parameter naming glitch) and Issue [#178](https://github.com/DapperLib/Dapper/issues/178) (execute reader now wraps the command/reader pair, to extend the command lifetime; note that the underlying command/reader are available by casting to `IWrappedDataReader`)

### 1.35

- Fix Issue [#151](https://github.com/DapperLib/Dapper/issues/151) (Execute should work with `ExpandoObject` etc); Fix Issue #182 (better support for db-type when using `object` values);
- Output expressions / callbacks in dynamic args (via Derek); arbitrary number of types in multi-mapping (via James Holwell);
- Fix `DbString`/Oracle bug (via Mauro Cerutti); new support for **named positional arguments**

### 1.34

- Support for `SqlHierarchyId` (core)

### 1.33

- Support for `SqlGeometry` (core) and `DbGeometry` (EF)

### 1.32

- Support for `SqlGeography` in core library

### 1.31

- Fix issue with error message when there is a column/type mismatch

### 1.30

- Better async cancellation

### 1.29

- Make underscore name matching optional (opt-in) - this can be a breaking change for some people

### 1.28

- Much better numeric type conversion; fix for large oracle strings; map `Foo_Bar` to `FooBar` (etc); `ExecuteScalar` added; stability fixes

### 1.27

- Fixes for type-handler parse; ensure type-handlers get last dibs on configuring parameters

### 1.26

- New type handler API for extension support

### 1.25

- Command recycling and disposing during pipelined async multi-exec; enable pipeline (via sync-over-async) for sync API"
