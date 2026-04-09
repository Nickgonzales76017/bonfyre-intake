# bonfyre-core

Shared substrate for the Bonfyre runtime.

This repo is the first extraction from the current mixed Bonfyre source surface. It is intended to become the canonical home for:

- shared libraries
- artifact contracts and schemas
- canonicalization helpers
- compression substrate
- cross-binary build utilities

## Why This Exists

Bonfyre currently has two overlapping source forests:

- `/Users/nickgonzales/Documents/Bonfyre/10-Code/*`
- `/Users/nickgonzales/Projects/bonfyre-oss/cmd/*`

`bonfyre-core` is the first cleanup step. Its job is to hold the reusable substrate that multiple runtime layers depend on, so later extractions can target a stable foundation instead of copying shared code across repos.

## Phase 1 Scope

Initial migration batch:

- `liblambda-tensors`
- `ArtifactManifest`
- `TreeSitterCanon`
- `JqCanon`
- `ZstdFamilyCompressor`
- `_shared/bonfyre_toolkit.py`

## Phase 2 Scope

Second migration batch:

- `libbonfyre`
- `BonfyreCanon`
- `BonfyreCompress`
- `BonfyreHash`

Source inventory and rationale live in:

- [docs/migration-batch-1.md](/Users/nickgonzales/Projects/bonfyre-core/docs/migration-batch-1.md)
- [docs/migration-batch-2.md](/Users/nickgonzales/Projects/bonfyre-core/docs/migration-batch-2.md)
- [docs/source-imports.md](/Users/nickgonzales/Projects/bonfyre-core/docs/source-imports.md)

## Intended Layout

- `lib/`
  - shared libraries
- `schemas/`
  - artifact schema and validation contract
- `tools/`
  - canonicalization and compression helpers
- `cmd/`
  - core Bonfyre binaries backed by shared substrate
- `scripts/`
  - migration and packaging helpers
- `docs/`
  - extraction docs and dependency maps

## Relationship To Other Repos

- `bonfyre-core`
  - shared substrate and contracts
- `bonfyre-intake`
  - media/text intake stages
- `bonfyre-intelligence`
  - retrieval, indexing, brief, graph, proof layers
- `bonfyre-publish`
  - render, emit, narrate, pack, distribute
- `bonfyre-control`
  - orchestration, API, auth, runtime control plane
- `bonfyre-oss`
  - stage integration, workflows, examples, shared Pages runtime
- `bonfyre`
  - prod integration and promoted releases
