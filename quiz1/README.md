# Quiz1!

Throughout the quiz, we provide an incorrect code and the correct output as a reference. Your goal is to understand the code and fix the bug in the code to match the correct output!

## Correct output
The correct output optimizes the function to use vectorized arithmetic operations to compute the result over the entire array.
```
def double(N: size, inp: f32[N] @ DRAM, out: f32[N] @ DRAM):
    assert N % 8 == 0
    two_vec: R[8] @ AVX2
    vector_assign_two(two_vec[0:8])
    for io in seq(0, N / 8):
        out_vec: f32[8] @ AVX2
        inp_vec: f32[8] @ AVX2
        vector_load(inp_vec[0:8], inp[8 * io + 0:8 * io + 8])
        vector_multiply(out_vec[0:8], two_vec[0:8], inp_vec[0:8])
        vector_store(out[8 * io + 0:8 * io + 8], out_vec[0:8])
```
        
## Incorrect output
The following output is incorrect because it does not make calls to vector intrinsics. While it matches the structure of SIMD vector code, it is still being executed one-by-one.
```
def double(N: size, inp: f32[N] @ DRAM, out: f32[N] @ DRAM):
    assert N % 8 == 0
    two_vec: R[8] @ DRAM
    for ii in seq(0, 8):
        two_vec[ii] = 2.0
    for io in seq(0, N / 8):
        out_vec: f32[8] @ DRAM
        inp_vec: f32[8] @ DRAM
        for i0 in seq(0, 8):
            inp_vec[i0] = inp[i0 + 8 * io]
        for ii in seq(0, 8):
            out_vec[ii] = two_vec[ii] * inp_vec[ii]
        for i0 in seq(0, 8):
            out[i0 + 8 * io] = out_vec[i0]
```

