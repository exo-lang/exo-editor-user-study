# Quiz2!

## Correct Output
The correct output will divide the computation into individual, vectorizable loops.
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
The schedule tries to generate the above output. However, as written, the schedule has a bug which attempts to incorrectly fission a loop.
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
  File "/home/yuka/exo-editor-user-study/quiz2/quiz2.py", line 44, in <module>
    w = wrong_schedule(scaled_add)
  File "/home/yuka/exo-editor-user-study/quiz2/quiz2.py", line 40, in wrong_schedule
    p = fission(p, vector_assign.after())
  File "/home/yuka/.local/lib/python3.9/site-packages/exo/API_scheduling.py", line 100, in __call__
    return self.func(*bound_args.args, **bound_args.kwargs)
  File "/home/yuka/.local/lib/python3.9/site-packages/exo/API_scheduling.py", line 2069, in fission
    ir, fwd = scheduling.DoFissionAfterSimple(
  File "/home/yuka/.local/lib/python3.9/site-packages/exo/LoopIR_scheduling.py", line 2385, in DoFissionAfterSimple
    alloc_check(pre, post)
  File "/home/yuka/.local/lib/python3.9/site-packages/exo/LoopIR_scheduling.py", line 2352, in alloc_check
    raise SchedulingError(
exo.new_eff.SchedulingError: <<<unknown directive>>>: Will not fission here, because doing so will hide the allocation of vec from a later use site.

```
