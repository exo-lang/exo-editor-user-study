# Quiz 4!

## Correct output:
```
def exo_conv1d_tile_lt_kw(data: i32[4, 16] @ DRAM,
                          kernels: i32[16, 4, 4] @ DRAM,
                          out: i32[16, 16] @ DRAM):
    for io in seq(0, 4):
        out_tile: i32[4, 4, 4] @ RVM_TILE
        rvm_mzero(out_tile[0, 0:4, 0:4])
        rvm_mzero(out_tile[1, 0:4, 0:4])
        rvm_mzero(out_tile[2, 0:4, 0:4])
        rvm_mzero(out_tile[3, 0:4, 0:4])
        for c in seq(0, 4):
            y: i32[4, 4] @ DRAM_STATIC
            for ii in seq(0, 4):
                for r in seq(0, 4):
                    if ii + r + 4 * io < 16:
                        y[ii, r] = data[c, ii + r + 4 * io]
                    else:
                        y[ii, r] = 0
            kernel_tile_0: i32[4, 4] @ RVM_TILE
            kernel_tile_1: i32[4, 4] @ RVM_TILE
            kernel_tile_2: i32[4, 4] @ RVM_TILE
            data_tile: i32[4, 4] @ RVM_TILE
            rvm_mld(data_tile[0:4, 0:4], y[0:4, 0:4])
            rvm_mld(kernel_tile_0[0:4, 0:4], kernels[0:4, c, 0:4])
            rvm_mmasa(out_tile[0, 0:4, 0:4], data_tile[0:4, 0:4],
                      kernel_tile_0[0:4, 0:4])
            rvm_mld(kernel_tile_1[0:4, 0:4], kernels[4:8, c, 0:4])
            rvm_mmasa(out_tile[1, 0:4, 0:4], data_tile[0:4, 0:4],
                      kernel_tile_1[0:4, 0:4])
            rvm_mld(kernel_tile_2[0:4, 0:4], kernels[8:12, c, 0:4])
            rvm_mmasa(out_tile[2, 0:4, 0:4], data_tile[0:4, 0:4],
                      kernel_tile_2[0:4, 0:4])
            rvm_mld(kernel_tile_0[0:4, 0:4], kernels[12:16, c, 0:4])
            rvm_mmasa(out_tile[3, 0:4, 0:4], data_tile[0:4, 0:4],
                      kernel_tile_0[0:4, 0:4])
        rvm_mst(out_tile[0, 0:4, 0:4], out[0:4, 4 * io:4 + 4 * io])
        rvm_mst(out_tile[1, 0:4, 0:4], out[4:8, 4 * io:4 + 4 * io])
        rvm_mst(out_tile[2, 0:4, 0:4], out[8:12, 4 * io:4 + 4 * io])
        rvm_mst(out_tile[3, 0:4, 0:4], out[12:16, 4 * io:4 + 4 * io])
```


## Incorrect output:
```
def exo_conv1d_tile_lt_kw(data: i32[4, 16] @ DRAM,
                          kernels: i32[16, 4, 4] @ DRAM,
                          out: i32[16, 16] @ DRAM):
    for io in seq(0, 4):
        out_tile: i32[4, 4, 4] @ RVM_TILE
        rvm_mzero(out_tile[0, 0:4, 0:4])
        rvm_mzero(out_tile[1, 0:4, 0:4])
        rvm_mzero(out_tile[2, 0:4, 0:4])
        rvm_mzero(out_tile[3, 0:4, 0:4])
        for c in seq(0, 4):
            y: i32[4, 4] @ DRAM_STATIC
            for ki in seq(0, 4):
                for ii in seq(0, 4):
                    for r in seq(0, 4):
                        if ii + r + 4 * io < 16:
                            y[ii, r] = data[c, ii + r + 4 * io]
                        else:
                            y[ii, r] = 0
            for ki in seq(0, 4):
                for ii in seq(0, 4):
                    for r in seq(0, 4):
                        if ii + r + 4 * io < 16:
                            y[ii, r] = data[c, ii + r + 4 * io]
                        else:
                            y[ii, r] = 0
            for ki in seq(0, 4):
                for ii in seq(0, 4):
                    for r in seq(0, 4):
                        if ii + r + 4 * io < 16:
                            y[ii, r] = data[c, ii + r + 4 * io]
                        else:
                            y[ii, r] = 0
            for ki in seq(0, 4):
                for ii in seq(0, 4):
                    for r in seq(0, 4):
                        if ii + r + 4 * io < 16:
                            y[ii, r] = data[c, ii + r + 4 * io]
                        else:
                            y[ii, r] = 0
            kernel_tile_0: i32[4, 4] @ RVM_TILE
            kernel_tile_1: i32[4, 4] @ RVM_TILE
            kernel_tile_2: i32[4, 4] @ RVM_TILE
            data_tile: i32[4, 4] @ RVM_TILE
            rvm_mld(data_tile[0:4, 0:4], y[0:4, 0:4])
            rvm_mld(kernel_tile_0[0:4, 0:4], kernels[0:4, c, 0:4])
            rvm_mmasa(out_tile[0, 0:4, 0:4], data_tile[0:4, 0:4],
                      kernel_tile_0[0:4, 0:4])
            rvm_mld(kernel_tile_1[0:4, 0:4], kernels[4:8, c, 0:4])
            rvm_mmasa(out_tile[1, 0:4, 0:4], data_tile[0:4, 0:4],
                      kernel_tile_1[0:4, 0:4])
            rvm_mld(kernel_tile_2[0:4, 0:4], kernels[8:12, c, 0:4])
            rvm_mmasa(out_tile[2, 0:4, 0:4], data_tile[0:4, 0:4],
                      kernel_tile_2[0:4, 0:4])
            rvm_mld(kernel_tile_0[0:4, 0:4], kernels[12:16, c, 0:4])
            rvm_mmasa(out_tile[3, 0:4, 0:4], data_tile[0:4, 0:4],
                      kernel_tile_0[0:4, 0:4])
        for ko in seq(0, 4):
            rvm_mst(out_tile[ko, 0:4, 0:4], out[4 * ko:4 + 4 * ko,
                                                4 * io:4 + 4 * io])
```
