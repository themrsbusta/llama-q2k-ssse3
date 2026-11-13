# Benchmark records

`upstream.json` and `ssse3.json` are the raw `llama-bench` JSON results for the accepted A/B run. Each file has prompt processing and generation records with three samples, mean, and standard deviation. The model filename is reduced to its basename; all benchmark measurements and parameters are retained. `temperature.csv` records the CPU and package sensor samples during both variants (70–75 °C).

## Reproduce

Build two copies of the same llama.cpp commit and apply the same CMake options. Leave the patch unapplied in the baseline copy; in the optimized copy apply `/path/to/llama-q2k-ssse3/patches/llama-n4100-q2-review.patch`. Run this command for each build, changing only the executable path:

```sh
MODEL=/path/to/gemma-4-e2b-Q2_K.gguf
./build/bin/llama-bench \
  -m "$MODEL" -t 3 -p 128 -n 32 -b 2048 -ub 512 -ngl 0 -r 3 -o json
```

For build flags and machine details, see [`../docs/BUILD.md`](../docs/BUILD.md) and [`../docs/METHODOLOGY.md`](../docs/METHODOLOGY.md). On the tested Celeron N4100, disable BMI2 (`-DGGML_BMI2=OFF`); the CPU does not support that instruction set.

The model is not distributed. Its SHA-256 is `dce13bfe3684ed92bce8a60b60d87a840869dd54ba76c08144ba760c506cde00`.

`archived/ssse3-before-reboot-superseded.json` is retained as historical raw output from a prior kernel boot. It is excluded from the A/B summary because it was not measured in the same boot as the accepted baseline.
