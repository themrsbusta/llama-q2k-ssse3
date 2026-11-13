# Compatibility

The new specialization is selected by `#elif defined(__SSSE3__)` inside `ggml_vec_dot_q2_K_q8_K`. Its added intrinsics use SSE2 and SSSE3; the branch contains no AVX or SSE4.2 intrinsics. When the compiler target does not define `__SSSE3__`, the existing upstream code path remains in use.

The benchmark and correctness test ran on an Intel Celeron N4100 that supports SSE4.2 but not BMI2. Use `GGML_BMI2=OFF` when building for this CPU; leaving BMI2 enabled can produce a binary that exits with an illegal instruction. The measured build used `GGML_SSE42=ON`. Therefore, this evidence confirms operation on that N4100 configuration; it does **not** establish a full llama.cpp build or run on a processor that supports SSSE3 but lacks SSE4.2. The patch also passed an apply-check at compatibility commit `31953bcf74ba489a8bbb0c936c5fcf24fcf9485c`; functional testing and benchmark numbers in this package refer to base commit `5676dfbe01e7ebe0387913680eee0ed39bf4a363`.

Only the N4100 and the listed Gemma 4 E2B Q2_K model were measured. No other CPU families, quantizations, or GPU backends were tested.
