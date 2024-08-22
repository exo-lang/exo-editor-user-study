# Quiz2!

## Correct Output
```
def scaled_add(N: size, a: f32[N] @ DRAM, b: f32[N] @ DRAM, c: f32[N] @ DRAM):
    assert N % 8 == 0
    for io in seq(0, N / 8):
        vec: R[8] @ AVX2
        vec_1: R[8] @ AVX2
        vec_2: f32[8] @ AVX2
        vec_3: R[8] @ AVX2
        vec_4: R[8] @ AVX2
        vec_5: f32[8] @ AVX2
        for ii in seq(0, 8):
            vec_1[ii] = 2
        for ii in seq(0, 8):
            vec_2[ii] = a[8 * io + ii]
        for ii in seq(0, 8):
            vec[ii] = vec_1[ii] * vec_2[ii]
        for ii in seq(0, 8):
            vec_4[ii] = 3
        for ii in seq(0, 8):
            vec_5[ii] = b[8 * io + ii]
        for ii in seq(0, 8):
            vec_3[ii] = vec_4[ii] * vec_5[ii]
        for ii in seq(0, 8):
            c[8 * io + ii] = vec[ii] + vec_3[ii]
```

## Incorrect output (compiler error)
```
Traceback (most recent call last):
  File "/home/yuka/.local/bin/exocc", line 8, in <module>
    sys.exit(main())
  File "/home/yuka/.local/lib/python3.9/site-packages/exo/main.py", line 55, in main
    library = [
  File "/home/yuka/.local/lib/python3.9/site-packages/exo/main.py", line 58, in <listcomp>
    for proc in get_procs_from_module(load_user_code(mod))
  File "/home/yuka/.local/lib/python3.9/site-packages/exo/main.py", line 107, in load_user_code
    loader.exec_module(user_module)
  File "<frozen importlib._bootstrap_external>", line 790, in exec_module
  File "<frozen importlib._bootstrap>", line 228, in _call_with_frames_removed
  File "/home/yuka/exo-editor-user-study/quiz2/quiz2.py", line 82, in <module>
    print(wrong_schedule(scaled_add))
  File "/home/yuka/exo-editor-user-study/quiz2/quiz2.py", line 71, in wrong_schedule
    p = expand_dim(p, vector_reg, 8, "ii")
  File "/home/yuka/.local/lib/python3.9/site-packages/exo/API_scheduling.py", line 97, in __call__
    bargs[nm] = argp(bargs[nm], bargs)
  File "/home/yuka/.local/lib/python3.9/site-packages/exo/API_scheduling.py", line 720, in __call__
    expr = parse_fragment(
  File "/home/yuka/.local/lib/python3.9/site-packages/exo/parse_fragment.py", line 31, in parse_fragment
    return ParseFragment(
  File "/home/yuka/.local/lib/python3.9/site-packages/exo/parse_fragment.py", line 210, in __init__
    self._results = self.parse_e(pat)
  File "/home/yuka/.local/lib/python3.9/site-packages/exo/parse_fragment.py", line 216, in parse_e
    raise ParseFragmentError(
exo.parse_fragment.ParseFragmentError: ii not found in the current environment
```
