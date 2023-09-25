---
layout: docu
title: Iceberg
---

The `iceberg` extension is a loadable extension that implements support for the [Apache Iceberg format](https://iceberg.apache.org/).

> This extension is currently in an experimental state. Feel free to try it out, but be aware some things may not work as expected.

## Usage

To test the examples, download the [`iceberg_data.zip`](/data/iceberg_data.zip) file and unzip it.

```sql
SELECT count(*) FROM iceberg_scan('data/iceberg/lineitem_iceberg', ALLOW_MOVED_PATHS=true);
```
```text
51793
```

```sql
SELECT * FROM iceberg_snapshots('data/iceberg/lineitem_iceberg', ALLOW_MOVED_PATHS=true);
```
```text
1	3776207205136740581	2023-02-15 15:07:54.504	0	lineitem_iceberg/metadata/snap-3776207205136740581-1-cf3d0be5-cf70-453d-ad8f-48fdc412e608.avro
2	7635660646343998149	2023-02-15 15:08:14.73	0	lineitem_iceberg/metadata/snap-7635660646343998149-1-10eaca8a-1e1c-421e-ad6d-b232e5ee23d3.avro
```

> The `ALLOW_MOVED_PATHS` option ensures that some path resolution is performed, which allows scanning Iceberg tables that are moved.

## Known limitations
- The `iceberg` extension currently uses a file called `version-hint.txt` to figure out the latest table version. This file is not emitted by all systems (e.g. AWS Athena), which means DuckDB won't be able to read iceberg tables produces by those systems. Proper support for Iceberg Catalogs is still work in progress.
- Currently only reading is supported.
- Iceberg schemas are currently ignored, instead we let the DuckDB parquet reader figure out the schema.
  
## GitHub Repository

[<span class="github">GitHub</span>](https://github.com/duckdblabs/duckdb_iceberg)
