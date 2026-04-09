# Migration Batch 1

This is the first extraction batch for `bonfyre-core`.

## Goal

Move the shared substrate first, before splitting runtime layers like intake, intelligence, publish, or control.

That keeps later migrations simpler because they can all depend on one clearly defined foundation.

## Batch Contents

### 1. `liblambda-tensors`

Current source:

- `/Users/nickgonzales/Documents/Bonfyre/10-Code/liblambda-tensors`

Target:

- `lib/liblambda-tensors/`

Why first:

- already an independent nested git repo
- clearly a shared library
- low ambiguity about ownership

Status:

- imported into `bonfyre-core/lib/liblambda-tensors/`
- build artifacts removed from the extracted copy so the new repo stays source-first

### 2. `ArtifactManifest`

Current source:

- `/Users/nickgonzales/Documents/Bonfyre/10-Code/ArtifactManifest`

Target:

- `schemas/artifact-manifest/`

Why first:

- shared contract across Bonfyre binaries and Pages apps
- useful before any later repo split

Status:

- imported into `bonfyre-core/schemas/artifact-manifest/`

### 3. `TreeSitterCanon`

Current source:

- `/Users/nickgonzales/Documents/Bonfyre/10-Code/TreeSitterCanon`

Target:

- `tools/tree-sitter-canon/`

Why first:

- small standalone canon helper
- easy to verify with its existing smoke test

Status:

- imported into `bonfyre-core/tools/tree-sitter-canon/`

### 4. `JqCanon`

Current source:

- `/Users/nickgonzales/Documents/Bonfyre/10-Code/JqCanon`

Target:

- `tools/jq-canon/`

Why first:

- shared JSON canonicalization helper
- already packaged as a tiny standalone tool

Status:

- imported into `bonfyre-core/tools/jq-canon/`

### 5. `ZstdFamilyCompressor`

Current source:

- `/Users/nickgonzales/Documents/Bonfyre/10-Code/ZstdFamilyCompressor`

Target:

- `tools/zstd-family-compressor/`

Why first:

- shared compression helper that other layers can build on
- low ambiguity about ownership

Status:

- imported into `bonfyre-core/tools/zstd-family-compressor/`

### 6. `_shared/bonfyre_toolkit.py`

Current source:

- `/Users/nickgonzales/Documents/Bonfyre/10-Code/_shared/bonfyre_toolkit.py`

Target:

- `scripts/bonfyre_toolkit.py`

Why first:

- shared utility module used by lightweight scripts
- simple to validate with Python compilation

Status:

- imported into `bonfyre-core/scripts/bonfyre_toolkit.py`

## Not In Batch 1

These are core-adjacent, but should wait until batch 1 lands:

- `BonfyreCanon`
- `BonfyreCompress`
- `BonfyreHash`

Reason:

- they depend on the substrate this batch is trying to clarify
- they belong in `bonfyre-core`, but after the substrate itself is moved

## Acceptance Criteria

Batch 1 is complete when:

1. all six sources are copied into `bonfyre-core`
2. a repo-local build/test path exists for each imported tool/library
3. `SOURCE_OF_TRUTH_MAP.md` rows for these modules are marked migrated
4. `bonfyre-oss` and the vault stop being treated as canonical for these items

## Progress

- imported:
  - `liblambda-tensors`
  - `ArtifactManifest`
  - `TreeSitterCanon`
  - `JqCanon`
  - `ZstdFamilyCompressor`
  - `_shared/bonfyre_toolkit.py`
- verification:
  - `schemas/artifact-manifest/tests/smoke.sh`
  - `tools/tree-sitter-canon/tests/smoke.sh`
  - `tools/jq-canon/tests/smoke.sh`
  - `tools/zstd-family-compressor/tests/smoke.sh`
  - `python3 -m py_compile scripts/bonfyre_toolkit.py`
