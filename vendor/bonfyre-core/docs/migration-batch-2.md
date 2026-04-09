# Migration Batch 2

This is the second extraction batch for `bonfyre-core`.

## Goal

Move the runtime-facing core binaries that sit directly on top of the shared substrate from batch 1.

This batch only works cleanly if `libbonfyre` moves with them, because `BonfyreCanon` and `BonfyreCompress` both link against it.

## Batch Contents

### 1. `libbonfyre`

Current source:

- `/Users/nickgonzales/Projects/bonfyre-oss/lib/libbonfyre`

Target:

- `lib/libbonfyre/`

Why now:

- canonical shared C runtime contract for Bonfyre binaries
- direct dependency of `BonfyreCanon` and `BonfyreCompress`
- belongs in core rather than stage integration

Status:

- imported into `bonfyre-core/lib/libbonfyre/`
- extracted copy keeps source, header, and tests but not built archives or object files

### 2. `BonfyreCanon`

Current source:

- `/Users/nickgonzales/Projects/bonfyre-oss/cmd/BonfyreCanon`

Target:

- `cmd/bonfyre-canon/`

Why now:

- core canonicalization binary on top of Tree-sitter and `libbonfyre`
- aligns with the repo-split story around structure-aware substrate

Status:

- imported into `bonfyre-core/cmd/bonfyre-canon/`
- verified by local build and parse smoke check from the extracted path

### 3. `BonfyreCompress`

Current source:

- `/Users/nickgonzales/Projects/bonfyre-oss/cmd/BonfyreCompress`

Target:

- `cmd/bonfyre-compress/`

Why now:

- core family-aware compression binary
- depends on `libbonfyre` and belongs close to shared compression substrate

Status:

- imported into `bonfyre-core/cmd/bonfyre-compress/`
- verified by local build and `savings` command check from the extracted path

### 4. `BonfyreHash`

Current source:

- `/Users/nickgonzales/Projects/bonfyre-oss/cmd/BonfyreHash`

Target:

- `cmd/bonfyre-hash/`

Why now:

- content-addressing and Merkle hashing are core substrate responsibilities
- standalone build made it a low-risk first binary import

Status:

- imported into `bonfyre-core/cmd/bonfyre-hash/`
- verified by local build and file hash check from the extracted path

## Acceptance Criteria

Batch 2 is complete when:

1. `libbonfyre` and all three binaries are copied into `bonfyre-core`
2. extracted copies build from the new repo paths
3. at least one lightweight functional verification runs per binary
4. `bonfyre-oss` stops being treated as canonical for these modules after cutover

## Progress

- imported:
  - `libbonfyre`
  - `BonfyreCanon`
  - `BonfyreCompress`
  - `BonfyreHash`
- verification:
  - `make clean test` in `lib/libbonfyre/`
  - `make clean all` in `cmd/bonfyre-hash/`
  - `make clean all` in `cmd/bonfyre-compress/`
  - `make clean all` in `cmd/bonfyre-canon/`
  - `./bonfyre-hash file /tmp/bonfyre-core-hash.txt`
  - `./bonfyre-compress savings <tmpdir>`
  - `./bonfyre-canon parse <tmp.json> <tmpdir/out>`
