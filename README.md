# ubx-schema-google

A real, frozen, versioned Google Cloud provider schema snapshot -- the
pinnable distribution artifact `ubx-provider-dynamic` and `ubiquex`
resolve a single `[providers.google]` entry against, with zero network
calls at schema resolution time (see `provider/acquireschema.go` in
`ubiquex`, and `internal/snapshot`'s own doc comment in
`ubx-provider-dynamic`). The resource/data-source split below is a
real, internal discovery-time detail -- one pin resolves all of it.

## What's here

Google Cloud's own real published identity is a GROUP of 524 members --
262 real, distinct Discovery Documents (one per product/version
channel, e.g. `google_compute`, `google_compute_beta`,
`google_compute_alpha` as three separate real channels), each fetched
once and built into BOTH a resource-mode member and a data-source-mode
member from that one fetch:

- 262 resource-mode members (1,546 real resource types total).
- 262 data-source-mode members (792 real, unclaimed read-only
  operations total).

- `manifest.json` -- the group's own real identity: `schema_format`,
  `provider`, one `version` for the WHOLE group, and which member names
  it bundles.
- `members/<name>.json` -- one real, complete, independently-diffable
  file per member. Committed as separate files, not one combined blob,
  so a real version bump's own git diff shows exactly which of the 524
  real members actually changed -- this is the whole reason the format
  exists at this scale: a single product's own Discovery Document
  changing should never touch the other 523 files.
- `.github/workflows/hash-watch.yml` -- runs weekly (and on manual
  dispatch), regenerates every member from its own live Discovery
  Document and opens a PR only when the group's own mechanically-derived
  version (the highest real change level found across every member --
  `internal/snapshot`'s `AssembleGroup`) actually moves. Never
  auto-merges.
- `.github/workflows/publish.yml` -- manual-dispatch-only. Packs
  `manifest.json` and every `members/*.json` into one compressed archive
  (`snapshot.tar.gz`, ~19MB from ~198MB raw content) and cuts a real
  GitHub Release tagged `v<version>` carrying exactly two assets:
  `snapshot.tar.gz` and `SHA256SUMS`. The archive exists purely so a
  real pinned resolution is still one real download regardless of
  member count -- the COMMITTED files (what a reviewer actually sees)
  are always the separate, per-member ones above.

## Consuming a real, published version

In `ubiquex`, one real pin resolves the whole group -- all 524 real
members are served together from the SAME launch, the SAME real
download:

```toml
[providers.google]
source  = "ubiquex/google"
version = "1.0.0"
```

`provider.AcquireSchema`'s own cache-by-source+version resolves ONE real
download and ONE extracted cache directory
(`~/.ubx/schemas/ubiquex/google/1.0.0/`) -- the launched process merges
every real member of the group (`internal/snapshot.MergeDiscoveryDocGroup`)
into one served schema, `ResourceSchemas` and `DataSourceSchemas`
together, exactly like a real, hand-written Terraform provider already
looks from the outside.

## Versioning

One real, mechanically-derived semver number for the WHOLE group, not
one per member: the highest real change level found across every
member (a brand new resource type or a field that gained write access
bumps MINOR; a resource type or field that disappeared, or a field that
lost write access, bumps MAJOR; a pure description-text change bumps
PATCH), plus an unconditional MAJOR if a member the group used to bundle
is gone entirely. See `internal/snapshot/diff.go` and `AssembleGroup` in
`ubx-provider-dynamic` for the real rule.

`v1.0.0` is this group's real, first-ever snapshot, built directly on
top of `ubx-provider-dynamic`'s UBI-182 resource/data-source collapse --
one `[dynamic_providers.google_<product>]` entry per real Discovery
Document driving generation from the start, no separate `_ds` table per
product to collapse away later, matching `ubx-schema-github`'s own
real, first-ever shape rather than `ubx-schema-kubernetes`'s/
`ubx-schema-datadog`'s, which were regenerated and republished onto
this shape after starting on the older, two-table one.
