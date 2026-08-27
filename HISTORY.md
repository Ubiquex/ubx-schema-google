# HISTORY.md — narrative archive

> Consulted only when a session needs to know why a decision was made, not on
> every open. For what's current, read `STATE.md` instead.

This file is new as of UBI-183 (2026-08-27). Real history predating it lives
in `ubiquex`'s own `HISTORY.md` (search `UBI-182`, `UBI-186`, `UBI-189`,
`UBI-194`) and in this repo's own real `git log`/merged-PR history, which is
authoritative for what actually shipped and when.

## Real, known decisions worth carrying forward

**First real published version was `1.0.0`, not `0.1.0`** — same correction
as every other schema repo in this org; see `ubx-schema-kubernetes`'s own
`HISTORY.md` for the full account.

**`google_dlp_dlp_job`'s own real triple-repeat bug, fixed at the source.**
A real generation bug produced a tripled field/entry for this one resource
type; root-caused and fixed in `ubx-provider-dynamic` itself (not a
workaround here), with the full ripple applied across affected downstream
artifacts.

**Bootstrap fallback, not yet retired (2026-08-27).** This repo's own
`v1.0.0` predates `min_binary_version` (UBI-194). Deliberately not forced to
regenerate; see `STATE.md`.
