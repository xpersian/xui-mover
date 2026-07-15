# Changelog

All notable changes to this project are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

## [Unreleased]

## [1.0.0] - 2026-07-15

### Added

- `backup.sh`: backend detection (`XUI_DB_TYPE`/`XUI_DB_DSN`/`XUI_DB_FOLDER`), Postgres dump via `pg_dump -Fc` over TCP with `PGPASSWORD` (no `sudo -u postgres`, avoiding the classic `/root`-permission failure entirely), SQLite stop/copy/restart snapshot, cert + reference-env-file bundling, `meta.json` with source row counts, timestamped tarball with SHA256 checksum, optional direct `--push-to` SSH delivery (key or password auth, verified by remote checksum comparison — not just `scp` exit status), multi-node detection with a loud warning and required `--i-know-this-is-multi-node` override.
- `restore.sh`: target backend/DSN detection, upfront Postgres connectivity check with guidance to provision the role/db via `x-ui`'s own menu, archive backend-mismatch detection, boxed confirmation summary requiring a typed `RESTORE` (never `y/n`), unconditional stop-x-ui-first ordering before any database operation, `pg_restore --no-owner --role=<target-role> -c --if-exists` restore, SQLite file replace with `PRAGMA integrity_check` verification, cert restore with sane key/cert permissions, health-polled service start, post-restore sanity-check row counts compared against the recorded source counts.
- Cross-cutting: numbered step-banner UI, color-coded log levels, all output tee'd to `/var/log/xui-mover-<timestamp>.log`, distinct non-zero exit code per failure class, automatic last-20-lines x-ui log tail on any failure once the service has been stopped, `--dry-run` on both scripts, `--yes`/`--confirm-restore` non-interactive mode that still requires the explicit confirmation flag (never silently skips the safety gate), guarded temp-workdir cleanup on both success and failure.
- `lib/common.sh`: documented reference copy of every shared helper function (not sourced at runtime — both entry-point scripts remain independently runnable via `bash <(curl -fsSL ...)`).
