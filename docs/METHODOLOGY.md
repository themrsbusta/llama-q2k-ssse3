# A/B methodology and results

## Setup

- Upstream commit for both builds: `5676dfbe01e7ebe0387913680eee0ed39bf4a363`.
- Only the `ggml-cpu` library differed between baseline and optimized runs.
- CPU: Intel Celeron N4100, 4 cores/4 threads; 3 llama.cpp threads.
- Model: Gemma 4 E2B Q2_K, 4,647,450,147 parameters; SHA-256 `dce13bfe3684ed92bce8a60b60d87a840869dd54ba76c08144ba760c506cde00`.
- Backend: CPU, `-ngl 0`; prompt 128 tokens; generation 32 tokens; batch 2048; ubatch 512; 3 repetitions.
- Build: Release, `GGML_NATIVE=OFF`, `GGML_BMI2=OFF`, `GGML_SSE42=ON`, AVX/AVX2/FMA/F16C/CUDA/Vulkan/BLAS disabled, OpenMP enabled.
- Kernel: Ubuntu `7.0.0-43-generic`. The accepted baseline and optimized run were measured in the same boot.
- Temperature observed during the A/B run: 70–75 °C (CPU and package sensors).
- Yoosee remained active; other background load was not controlled.

## Results

| Variant | Prompt tok/s (mean ± SD) | Generation tok/s (mean ± SD) | Ratio vs. baseline |
| --- | ---: | ---: | ---: |
| Upstream baseline | 2.238986 ± 0.002929 | 0.576651 ± 0.005388 | 1.000× |
| SSSE3 patch | 6.413858 ± 0.012860 | 1.390631 ± 0.006469 | 2.865× / 2.412× |

The ratios are calculated from the reported means. The raw `llama-bench` JSON is in [`../benchmarks/upstream.json`](../benchmarks/upstream.json) and [`../benchmarks/ssse3.json`](../benchmarks/ssse3.json); [`../benchmarks/temperature.csv`](../benchmarks/temperature.csv) records the sensor samples. The model is not included.

## Limits

This is a short, end-to-end measurement on one CPU and one model; it does not isolate the Q2_K dot-product kernel. Yoosee and other background work were active and not controlled. Treat the numbers as observations for this machine and run, not expected performance on other systems. An earlier optimized run from the previous kernel boot is retained under `../benchmarks/archived/` and excluded from this comparison.
