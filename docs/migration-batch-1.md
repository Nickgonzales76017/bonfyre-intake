# Migration Batch 1

This is the first extraction batch for `bonfyre-intake`.

## Goal

Move the live intake-facing binaries out of `bonfyre-oss` into their own layer repo,
while keeping the dependency boundary explicit: shared substrate stays in `bonfyre-core`.

## Batch Contents

### 1. `BonfyreIngest`

Current source:

- `/Users/nickgonzales/Projects/bonfyre-oss/cmd/BonfyreIngest`

Target:

- `cmd/bonfyre-ingest/`

### 2. `BonfyreMediaPrep`

Current source:

- `/Users/nickgonzales/Projects/bonfyre-oss/cmd/BonfyreMediaPrep`

Target:

- `cmd/bonfyre-media-prep/`

### 3. `BonfyreMFADict`

Current source:

- `/Users/nickgonzales/Projects/bonfyre-oss/cmd/BonfyreMFADict`

Target:

- `cmd/bonfyre-mfa-dict/`

### 4. `BonfyreParagraph`

Current source:

- `/Users/nickgonzales/Projects/bonfyre-oss/cmd/BonfyreParagraph`

Target:

- `cmd/bonfyre-paragraph/`

### 5. `BonfyreTranscribe`

Current source:

- `/Users/nickgonzales/Projects/bonfyre-oss/cmd/BonfyreTranscribe`

Target:

- `cmd/bonfyre-transcribe/`

Note:

- depends on the Homebrew whisper toolchain in addition to `libbonfyre`

### 6. `BonfyreTranscriptClean`

Current source:

- `/Users/nickgonzales/Projects/bonfyre-oss/cmd/BonfyreTranscriptClean`

Target:

- `cmd/bonfyre-transcript-clean/`

### 7. `BonfyreTranscriptFamily`

Current source:

- `/Users/nickgonzales/Projects/bonfyre-oss/cmd/BonfyreTranscriptFamily`

Target:

- `cmd/bonfyre-transcript-family/`

## Acceptance Criteria

Batch 1 is complete when:

1. all seven binaries are copied into `bonfyre-intake`
2. local Makefiles build against a local `bonfyre-core` path
3. at least the non-whisper binaries verify from the extracted paths
4. `bonfyre-oss` stops being treated as canonical for these modules after cutover

## Current Validation Strategy

- vendor a local snapshot of `bonfyre-core` under `vendor/bonfyre-core/`
- point intake Makefiles at `../../vendor/bonfyre-core/lib/libbonfyre`
- verify the non-whisper commands first
- treat `bonfyre-transcribe` as a dependency-aware build because it also needs the Homebrew whisper stack

## Progress

- imported:
  - `BonfyreIngest`
  - `BonfyreMediaPrep`
  - `BonfyreMFADict`
  - `BonfyreParagraph`
  - `BonfyreTranscribe`
  - `BonfyreTranscriptClean`
  - `BonfyreTranscriptFamily`
- verification:
  - vendored `bonfyre-core/lib/libbonfyre` builds locally
  - all seven intake binaries build from extracted paths
  - usage checks run successfully for `bonfyre-ingest`, `bonfyre-paragraph`, `bonfyre-transcript-clean`, and `bonfyre-transcribe`
