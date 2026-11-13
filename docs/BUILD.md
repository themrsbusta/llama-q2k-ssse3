# Build and test

The patch was developed against llama.cpp commit `5676dfbe01e7ebe0387913680eee0ed39bf4a363`.

```sh
git clone https://github.com/ggml-org/llama.cpp.git
cd llama.cpp
git checkout 5676dfbe01e7ebe0387913680eee0ed39bf4a363
git apply /path/to/llama-q2k-ssse3/patches/llama-n4100-q2-review.patch
cmake -S . -B build-q2k-ssse3 \
  -DCMAKE_BUILD_TYPE=Release \
  -DGGML_NATIVE=OFF \
  -DGGML_BMI2=OFF \
  -DGGML_SSE42=ON \
  -DGGML_AVX=OFF \
  -DGGML_AVX2=OFF \
  -DGGML_FMA=OFF \
  -DGGML_F16C=OFF \
  -DGGML_OPENMP=ON \
  -DGGML_BLAS=OFF \
  -DGGML_CUDA=OFF \
  -DGGML_VULKAN=OFF \
  -DLLAMA_BUILD_TESTS=ON
cmake --build build-q2k-ssse3 --target llama-bench test-backend-ops -j2
build-q2k-ssse3/bin/test-backend-ops \
  -b CPU -o MUL_MAT -p 'type_a=q2_K' -j 1
```

The measured build used Ubuntu 24.04.5, GCC, Release mode, CPU backend, and the flags listed above. The CMake configuration enabled SSE4.2 for the full build; the added branch itself uses only SSE2 and SSSE3 intrinsics. See [`COMPATIBILITY.md`](COMPATIBILITY.md) before treating this as a test of an SSSE3-only system.

For the A/B command, use the same commit and options for baseline and patch builds, and follow [`../benchmarks/README.md`](../benchmarks/README.md).
