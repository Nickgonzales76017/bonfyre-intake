# bonfyre-intake

Media and text intake layer for the Bonfyre runtime.

This repo is the first dependent extraction built on top of `bonfyre-core`.
It is intended to become the canonical home for:

- ingestion binaries
- media preprocessing
- transcription entrypoints
- transcript cleanup and paragraphization
- transcript family grouping
- intake-adjacent dictionaries and utilities

## Why This Exists

`bonfyre-core` now owns the shared substrate and core runtime library.

`bonfyre-intake` exists so the input-facing binaries can evolve together without
staying mixed into the stage integration repo.

The current live source for these binaries is:

- `/Users/nickgonzales/Projects/bonfyre-oss/cmd/*`

This repo is the extraction target for intake-specific runtime modules from that live tree.

## Initial Scope

First migration batch:

- `BonfyreIngest`
- `BonfyreMediaPrep`
- `BonfyreMFADict`
- `BonfyreParagraph`
- `BonfyreTranscribe`
- `BonfyreTranscriptClean`
- `BonfyreTranscriptFamily`

Source inventory and rationale live in:

- [docs/migration-batch-1.md](/Users/nickgonzales/Projects/bonfyre-intake/docs/migration-batch-1.md)

## Intended Layout

- `cmd/`
  - intake binaries
- `vendor/bonfyre-core/`
  - temporary local dependency slice for extraction validation
- `docs/`
  - extraction docs and dependency notes

## Relationship To Other Repos

- `bonfyre-core`
  - shared substrate, contracts, and core runtime library
- `bonfyre-intake`
  - input-facing runtime binaries
- `bonfyre-intelligence`
  - analysis, retrieval, indexing, brief, proof
- `bonfyre-publish`
  - render, emit, narrate, pack, distribute
- `bonfyre-control`
  - orchestration, API, auth, runtime control plane
- `bonfyre-oss`
  - stage integration and shared workflows
- `bonfyre`
  - prod integration and promoted releases

## Dependency Note

During extraction, this repo vendors the minimum current `bonfyre-core` dependency
surface under `vendor/bonfyre-core/` so the binaries can build against a stable local path.

Right now that vendored surface is just `lib/libbonfyre/`.

This is a transition step for migration validation, not the long-term coupling model.
