# llama.cpp Q2_K SSSE3 Optimization

**Division:** Panthera Research. **License:** [MIT](LICENSE).


An independent Panthera Softwares patch for the `ggml_vec_dot_q2_K_q8_K` CPU kernel in [`ggml-org/llama.cpp`](https://github.com/ggml-org/llama.cpp). This package contains the patch and validation artifacts, not a llama.cpp source tree or model weights. It is not an official llama.cpp release.

## Recorded A/B results

Three `llama-bench` repetitions on the same Intel Celeron N4100, model, upstream commit, compiler flags, and boot. See [benchmark methodology](benchmarks/README.md) and [raw JSON](benchmarks/).

| Variant | Prompt tok/s (mean ± SD) | Generation tok/s (mean ± SD) | Ratio vs. baseline |
| --- | ---: | ---: | ---: |
| Upstream baseline | 2.238986 ± 0.002929 | 0.576651 ± 0.005388 | 1.000× |
| SSSE3 patch | 6.413858 ± 0.012860 | 1.390631 ± 0.006469 | 2.865× / 2.412× |

These are end-to-end measurements for one CPU and model, not isolated kernel timings. Background load was not controlled. See the [limitations](docs/METHODOLOGY.md).

## Quick start

Apply [`patches/llama-n4100-q2-review.patch`](patches/llama-n4100-q2-review.patch) to llama.cpp commit `5676dfbe01e7ebe0387913680eee0ed39bf4a363`, then follow [build and test instructions](docs/BUILD.md).

## Project details

- Upstream base: `5676dfbe01e7ebe0387913680eee0ed39bf4a363`.
- Compatibility apply-check: `31953bcf74ba489a8bbb0c936c5fcf24fcf9485c`.
- Modified path: `ggml/src/ggml-cpu/arch/x86/quants.c`, function `ggml_vec_dot_q2_K_q8_K`.
- AI-assisted implementation: Argos Codex, for Panthera Softwares.
- Human maintainer and release responsibility: Busta Supastar.
- The optimized branch is guarded by `__SSSE3__`; see [compatibility notes](docs/COMPATIBILITY.md).
- Upstream MIT notice is preserved in [`LICENSE-llama.cpp`](LICENSE-llama.cpp); this package is MIT-licensed under [`LICENSE`](LICENSE).

## Contents

- [`patches/`](patches/): standalone patch.
- [`benchmarks/`](benchmarks/): raw A/B JSON and reproduction notes.
- [`tests/`](tests/): correctness test output and command.
- [`docs/`](docs/): build, methodology, compatibility, attribution, and validation record.
- [`SHA256SUMS`](SHA256SUMS): checksums for published files (excluding the checksum file itself).
