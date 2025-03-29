# TI-68k Status Line Modification Benchmarks

How fast can a TI-68k calculator change its status line's content in a real-world workload? It's **statline** vs. **Flib 3.2** on twisted-nematic57's HW4 TI-89 Titanium, clocked at approximately 25.3 MHz, running AMS 3.10.

The BASIC (*.89p) programs under "bench" directory are labeled according to the status line modification program they use. All are pre-tokenized. Before running the benchmarks, "sierpgdb.89d" should be RCL'd. They must be located in on-calc folder "ΔSTATL", which stands for "delta(difference) between status line programs".

statline itself is included in the repo in the "deps" folder. apdreset is also used to deter the calc from sleeping automatically. Flib can be downloaded from [here.](https://www.ticalc.org/pub/89/asm/libs/flib.zip) All of the dependencies should be *archived* in the "misc" folder on-calc. (They will likely remain archived in real-world scenarios; therefore this makes sense for this benchmark.)

## Methodology

Each benchmarking program's exact job is to render a Sierpinski triangle to the Graph Screen using floating-point math routines and `PtOn`. No attempt has been made at optimizing the math routines used in rendering, but they are not unreasonably slow. The code should be clear enough to be self-documenting. Again, for exact parity across runs, "sierpgdb" should be recalled before running the first test, and the window vars should not be touched afterwards; this helps ensure that `PtOn`'s internal logic does not interfere too much with each benchmark run.

In the benchmarking programs, the status line modification program is called **once** to update the iteration number to let the user know how many are done so far.

Each of the benchmarking programs (from the "bench" directory) were run and timed from an automation program, "autobench.89p", in the ΔSTATL folder. Final execution times were saved to variables.

## Results

| Sierpinski Triangle Renderer - 50000 Iterations | Execution Time (s) |
| ----------------------------------------------- | ------------------ |
| No status line updates (`nosltest`)           | 2166               |
| Flib (`flibtest`)                             | 4287               |
| **statline (`stlntest`)**              | **2914**     |

Using Flib instead of no status line updates at all adds 2121s of execution time to the baseline of 2166s. statline, however, adds only 748s, which means it is **~183% faster than Flib in a real world scenario.** Therefore, it should be used in place of Flib for all status line modification tasks from BASIC.

## Synthetic benchmark (because why not?)

Just for funsies I did a benchmark where the status line is updated 50000 times with an integer ranging from 1-50000 and absolutely nothing else. It's a purely synthetic benchmark that is named `rawbench.89p` in the "bench" folder.

| 1 -> 50000 incrementer            | Execution Time (s) |
| --------------------------------- | ------------------ |
| Flib (`flibtest`)               | 2242               |
| **statline (`stlntest`)** | **777**      |

In the synthetic benchmark, statline is... **~188% faster than Flib.** Unsurprisingly, it is not far off from the real-world Sierpinski triangle rendering benchmark.

Okay, I'm done bragging about my creation, now go use it.
