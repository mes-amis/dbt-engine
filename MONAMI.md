# Mon Ami dbt engine fork

Base: dbt-labs/dbt v2.0.4 (`89ca6a5d819606d66050642ad05de5b17463d9f3`).
Python wheel version: `2.0.4+monami.1`. The engine reports `dbt-oss 2.0.4`.

BigQuery relation discovery now uses the existing target-qualified
`INFORMATION_SCHEMA.TABLES` query directly. The released driver's
`GetObjects(Tables)` enumerates schemas and fetches each table's metadata
serially, although the caller only needs names and types. See upstream
[issue 16318](https://github.com/dbt-labs/dbt/issues/16318).

No model SQL, relation-cache semantics, column discovery, or other adapter
is changed. Missing datasets and permission failures still propagate through
the existing query/error handling. Dataset existence checks are a separate
path and remain unchanged (upstream issue 16297).

Run the manually dispatched **Mon Ami dbt wheels** workflow on the desired
commit. It runs focused Rust tests, builds CPython 3.11+ ABI3 wheels for
macOS ARM64 and Linux x86_64/ARM64, and smoke-tests installation. Release
artifacts must be published under a new immutable version; do not replace
assets referenced by a downstream `uv.lock`.

Remove the downstream override once an upstream version passes the same
single-model network benchmark without the per-object metadata sweep.
