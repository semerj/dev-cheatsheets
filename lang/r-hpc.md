# High Performance Computing in R

## BLAS
[Which BLAS is used and how can it be changed?](http://cran.r-project.org/bin/macosx/RMacOSX-FAQ.html#Which-BLAS-is-used-and-how-can-it-be-changed_003f)

> The BLAS library used by R depends on the way R was compiled (see `R Installation and Administration' manual for details). Current R binaries supplied from CRAN provide both vecLib-based BLAS and reference BLAS shipped with R. vecLib is a part of Apple's Accelerate framework which provides an optimized BLAS implementation for Mac hardware. Although fast, it is not under our control and may possibly deliver inaccurate results.

> The CRAN binary uses `--enable-BLAS-shlib` option and two Rblas shared libraries are supplied: `libRblas.vecLib.dylib` which uses vecLib BLAS and `libRblas.0.dylib` which uses reference BLAS from R. A symbolic link `libRblas.dylib` determines which one is used. Currently the default is to use the R BLAS: this is recommended for precision.

> In order to change which BLAS is used, change the `libRblas.dylib` symlink correspondingly – for example in Terminal:

> ```sh
$ cd /Library/Frameworks/R.framework/Resources/lib

> # for vecLib use in R 2.x (for R 3.x see below)
$ ln -sf libRblas.vecLib.dylib libRblas.dylib

> # for R reference BLAS use
$ ln -sf libRblas.0.dylib libRblas.dylib
```

> This feature is only present in the R CRAN binary. Ordinarily compiled R from sources will only have one of the above BLAS libraries, corresponding to the configuration options used.

### Instructions
1. Check symlink (before creating new one)

```sh
$ ls -l /Library/Frameworks/R.framework/Resources/lib
total 16624
-rwxrwxr-x  1 root  admin  3125828 Apr 10 15:55 libR.dylib
drwxrwxr-x  3 root  admin      102 Apr 10 15:55 libR.dylib.dSYM
-rwxrwxr-x  1 root  admin   193712 Apr 10 15:55 libRblas.0.dylib
lrwxr-xr-x  1 root  admin       16 May 22 23:19 libRblas.dylib -> libRblas.0.dylib
drwxrwxr-x  3 root  admin      102 Apr 10 15:55 libRblas.dylib.dSYM
-rwxrwxr-x  1 root  admin  1890472 Apr 10 15:55 libRlapack.dylib
drwxrwxr-x  3 root  admin      102 Apr 10 15:55 libRlapack.dylib.dSYM
-rwxrwxr-x  1 root  admin   262604 Apr 10 15:55 libgcc_s.1.dylib
-rwxrwxr-x  1 root  admin   262604 Apr 10 15:55 libgcc_s_x86_64.1.dylib
-rwxrwxr-x  1 root  admin  1484568 Apr 10 15:55 libgfortran.3.dylib
-rwxrwxr-x  1 root  admin   264952 Apr 10 15:55 libquadmath.0.dylib
-rwxrwxr-x  1 root  admin   996788 Apr 10 15:55 libreadline.5.2.dylib
lrwxr-xr-x  1 root  admin       21 May 22 23:19 libreadline.dylib -> libreadline.5.2.dylib
```

2. Create symlink

```sh
$ cd /Library/Frameworks/R.framework/Resources/lib

# R reference BLAS
$ ln -sf libRblas.0.dylib libRblas.dylib

# vecLib in R 3.x
$ ln -sf /System/Library/Frameworks/Accelerate.framework/Versions/Current/Frameworks/vecLib.framework/Versions/Current/libBLAS.dylib libRblas.dylib

# openBLAS
$ ln -sf $HOME/code/OpenBLAS/lib/libopenblas.dylib libRblas.dylib
```

3. check which BLAS is in use

```sh
# R reference BLAS
$ R CMD config BLAS_LIBS
-L/Library/Frameworks/R.framework/Resources/lib -lRblas
# this links an external library located in a particular directory

# vecLib
$ R CMD config BLAS_LIBS
-framework Accelerate

# check dynamic libraries used by executable
$ otool -L /Library/Frameworks/R.framework/R
```

4. install openBLAS

```sh
$ cd $HOME/code/
$ git clone git://github.com/xianyi/OpenBLAS.git
$ cd OpenBLAS
$ make CC=clang
$ make PREFIX=$HOME/code/OpenBLAS install
```

### Benchmarks

download and run benchmark script

```sh
$ curl http://r.research.att.com/benchmarks/R-benchmark-25.R -O

$ cat R-benchmark-25.R | time R --slave
```

I. Matrix calculation

| Test                                                        | Rblas | vecLib | openBLAS |
|:------------------------------------------------------------|-------|--------|----------|
| Creation, transp., deformation of a 2500x2500 matrix        | 1.002 | 1.039  | 1.014    |
| 2400x2400 normal distributed random matrix ^1000            | 0.160 | 0.167  | 0.160    |
| Sorting of 7,000,000 random values                          | 0.703 | 0.709  | 0.700    |
| 2800x2800 cross-product matrix (b = a' * a)                 | 10.48 | 0.575  | 0.533    |
| Linear regr. over a 3000x3000 matrix (c = a \ b')           | 5.963 | 0.398  | 0.350    |
| Trimmed geom. mean (2 extremes eliminated)                  | 1.614 | 0.545  | 0.507    |

II. Matrix functions

| Test                                                        | Rblas | vecLib | openBLAS |
|:------------------------------------------------------------|-------|--------|----------|
| FFT over 2,400,000 random values                            | 0.444 | 0.444  | 0.417    |
| Eigenvalues of a 640x640 random matrix                      | 0.833 | 0.492  | 0.689    |
| Determinant of a 2500x2500 random matrix                    | 4.099 | 0.341  | 0.410    |
| Cholesky decomposition of a 3000x3000 matrix                | 4.326 | 0.323  | 0.400    |
| Inverse of a 1600x1600 random matrix                        | 3.168 | 0.278  | 0.318    |
| Trimmed geom. mean (2 extremes eliminated)                  | 2.212 | 0.366  | 0.409    |

III. Programmation

| Test                                                        | Rblas | vecLib | openBLAS |
|:------------------------------------------------------------|-------|--------|----------|
| 3,500,000 Fibonacci numbers calculation (vector calc)       | 0.261 | 0.263  | 0.251    |
| Creation of a 3000x3000 Hilbert matrix (matrix calc)        | 0.257 | 0.257  | 0.258    |
| Grand common divisors of 400,000 pairs (recursion)          | 1.037 | 0.906  | 0.914    |
| Creation of a 500x500 Toeplitz matrix (loops)               | 0.355 | 0.348  | 0.340    |
| Escoufier's method on a 45x45 matrix (mixed)                | 0.362 | 0.321  | 0.324    |
| Trimmed geom. mean (2 extremes eliminated)                  | 0.322 | 0.308  | 0.305    |

Summary

| Test                                                        | Rblas | vecLib | openBLAS |
|:------------------------------------------------------------|-------|--------|----------|
| Total time for all 15 tests                                 | 33.45 | 6.860  | 7.078    |
| Overall mean (sum of I, II and III trimmed means/3)         | 1.048 | 0.395  | 0.399    |

## Parallel

### Expicit

* `snow`/`parallel`: Can use PVM, MPI, NWS as well as direct networking sockets. It provides an abstraction layer by hiding the communications details. `snow` relies on the Master / Slave model of communication in which one device or process (known as the master) controls one or more other devices or processes (known as slaves). `snow` implements an interface to three different low level mechanisms for creating a virtual connection between processes:
  * Socket
  * PVM (Parallel Virtual Machine)
  * MPI (Message Passing Interface)

* `snowfall`: Provides a more recent alternative to `snow`. Functions can be used in sequential or parallel mode.

* `foreach`: Allows general iteration over elements in a collection without the use of an explicit loop counter. Using foreach without side effects also facilitates executing the loop in parallel which is possible via the `doMC` (using `multicore` on single workstations), `doSNOW` (using `snow`, see above), `doMPI` (using `Rmpi`) packages and `doRedis` (using `rredis`) packages.

### Implicit

* `multicore`/`parallel`: Provides a way of running parallel computations in R on machines with multiple cores or CPUs by making use of operating system facilities. `multicore` is restricted to Linux and OS X.

### Applications
* `multtest` package on Bioconductor can use `snow`, `Rmpi` or rpvm for resampling-based testing of multiple hypothesis.
