# Correctness test

The CPU Q2_K `MUL_MAT` backend test completed with **55/55 cases passing** on the same llama.cpp base commit used by the benchmark, `5676dfbe01e7ebe0387913680eee0ed39bf4a363`. The complete captured output is [`test-backend-ops.log`](test-backend-ops.log).

Run it after building with `LLAMA_BUILD_TESTS=ON`:

```sh
build-q2k-ssse3/bin/test-backend-ops \
  -b CPU -o MUL_MAT -p 'type_a=q2_K' -j 3
```

The test exercises Q2_K matrix multiplication through the CPU backend. It does not replace testing on other processors or broader llama.cpp test suites.
