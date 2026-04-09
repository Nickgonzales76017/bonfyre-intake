# liblambda-tensors

Family-aware structural compression for JSON. **13-15% of raw JSON** with O(1) per-field random access.

## What it does

Given a "family" of structurally similar JSON records (same keys, different values), liblambda-tensors compresses them dramatically by exploiting shared structure:

| Scale | Raw JSON | liblambda-tensors | Ratio | gzip (no random access) |
|---|---:|---:|---:|---:|
| 100 members | 11,490 B | 1,664 B | **14.5%** | ~920 B (8.0%) |
| 1,000 members | 116,940 B | 12,525 B | **10.7%** | ~6,900 B (5.9%) |

**gzip is 2.5-2.8× smaller but requires full decompression.** liblambda-tensors preserves per-field random access.

## How it works

Five encoding tiers, each subsumes the previous:

1. **V1** — Type tags + varint + zigzag encoding + bitmask delta
2. **V2** — Small-int/float32/empty-string optimizations + LZ77 string back-references + sparse delta bitmask
3. **Interned** — Cross-member string deduplication via family string table (CB_FAMSTR)
4. **Huffman** — Per-position canonical Huffman coding with cost-model pruning
5. **Arithmetic** — Fractional-bit entropy measurement (confirms Shannon limit reached)

## Build

```sh
make        # builds liblambda-tensors.a + liblambda-tensors.so
make test   # builds + runs test suite (38 tests + compression benchmarks)
```

Requires: C11 compiler, libc, libm. **No other dependencies.**

Artifacts:
- `liblambda-tensors.a` — 41KB static library
- `liblambda-tensors.so` — 53KB shared library
- `include/lambda_tensors.h` — public API header (232 lines)

## Quick start

```c
#include "lambda_tensors.h"

// 1. Encode a single JSON binding array
unsigned char *packed;
size_t packed_len;
lt_encode_v2("[42,null,\"hello\",3.14,true]", &packed, &packed_len);

// 2. Decode back to JSON
char *json;
lt_decode_v2(packed, packed_len, &json);

// 3. Delta encode (only store what changed)
unsigned char *ref_packed, *tgt_packed, *delta;
size_t ref_len, tgt_len, delta_len;
lt_encode_v2("[42,null,\"hello\",3.14,true]", &ref_packed, &ref_len);
lt_encode_v2("[42,null,\"world\",3.14,false]", &tgt_packed, &tgt_len);
lt_delta_encode_v2(ref_packed, ref_len, tgt_packed, tgt_len, &delta, &delta_len);

// 4. Family compression (maximum savings)
LtStringTable strtab;
lt_strtab_init(&strtab);
LtStrtabCtx *ctx = lt_strtab_ctx_new();
for (int i = 0; i < n; i++)
    lt_strtab_ingest(ctx, family_members[i]);
lt_strtab_finalize(ctx, &strtab);
lt_strtab_ctx_free(ctx);

// Encode with interning
unsigned char *interned;
size_t interned_len;
lt_encode_v2_interned(family_members[0], &strtab, &interned, &interned_len);

// 5. Huffman (best compression with random access)
LtHuffTable htab;
lt_huff_init(&htab);
for (int i = 0; i < n; i++)
    lt_huff_ingest(&htab, family_members[i]);
lt_huff_finalize(&htab);

// Measure compressed size without allocating
int size = lt_measure_v2_huffman(family_members[0], &htab);

// Cleanup
lt_strtab_free(&strtab);
lt_huff_free(&htab);
free(packed); free(json); free(ref_packed); free(tgt_packed);
free(delta); free(interned);
```

## API overview

### Core encode/decode (V1 and V2)
- `lt_encode_v1()` / `lt_decode_v1()` — base format
- `lt_encode_v2()` / `lt_decode_v2()` — enhanced format (recommended)
- `lt_delta_encode_v2()` / `lt_delta_decode_v2()` — delta encoding
- `lt_measure_v2()` / `lt_delta_measure_v2()` — size without allocating

### Family string table (cross-member dedup)
- `lt_strtab_ctx_new()` → `lt_strtab_ingest()` → `lt_strtab_finalize()` → `lt_strtab_ctx_free()`
- `lt_encode_v2_interned()` / `lt_decode_v2_interned()`

### Huffman coding (best compression)
- `lt_huff_init()` → `lt_huff_ingest()` → `lt_huff_finalize()`
- `lt_measure_v2_huffman()` / `lt_delta_measure_v2_huffman()`

### Arithmetic (Shannon limit measurement)
- `lt_measure_v2_arithmetic()` / `lt_delta_measure_v2_arithmetic()`

## Thread safety

All state is in caller-owned structs. No global mutable state. Multiple threads can compress different families concurrently.

## License

TBD
