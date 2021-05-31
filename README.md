# hpc_lecture

- Student ID   : 21M30689
- Student Name : Sora Takashima

- base (MPI)                                          : example.cpp
- my_answer (MPI + cache blocking + Open MP + SIMD)   : my_matmul.cpp
- my_answer (MPI + CUDA + shared memory)              : my_matmul.cu

## Example

- prepare

- (on CPU or GPU)

```
21M30689@login0:~/hpc_lecture_2021/16_final_report> module load gcc/10.2.0 intel-mpi/19.9.304
21M30689@login0:~/hpc_lecture_2021/16_final_report> mpicxx my_matmul.cpp -fopenmp -march=native -O3
```

or

- (on GPU)

```
21M30689@login0:~/hpc_lecture_2021/16_final_report> module load vim cmake gcc cuda/11.2.146 openmpi nccl cudnn intel
21M30689@login0:~/hpc_lecture_2021/16_final_report> nvcc my_matmul.cu -lmpi
```

### base (MPI)

- N=1024, np=1 (on login node)

```
21M30689@login0:~/hpc_lecture_2021/16_final_report> ./a.out
N    : 1024
comp : 2.524679 s
comm : 0.002729 s
total: 2.527408 s (0.849678 GFlops)
error: 0.000129
21M30689@login0:~/hpc_lecture_2021/16_final_report>
```

- N=1024, np=4 (on login node)

```
21M30689@login0:~/hpc_lecture_2021/16_final_report> mpirun -np 4 ./a.out 
N    : 1024
comp : 0.520160 s
comm : 0.004226 s
total: 0.524385 s (4.095239 GFlops)
error: 0.000129
21M30689@login0:~/hpc_lecture_2021/16_final_report>
```

### my_answer (MPI + cache blocking + Open MP + SIMD)

- N=1024, np=4 (on login node)

```
21M30689@login0:~/hpc_lecture_2021/16_final_report> mpirun -np 4 ./a.out
N    : 1024
comp : 0.009060 s
comm : 0.004591 s
total: 0.013651 s (157.311971 GFlops)
error: 0.000102
21M30689@login0:~/hpc_lecture_2021/16_final_report> 
```

- N=2048, np=4,8 (on login node)

```
21M30689@login0:~/hpc_lecture_2021/16_final_report> mpirun -np 4 ./a.out
N    : 2048
comp : 0.060282 s
comm : 0.026178 s
total: 0.086460 s (198.702711 GFlops)
error: 0.000258
21M30689@login0:~/hpc_lecture_2021/16_final_report> mpirun -np 8 ./a.out
N    : 2048
comp : 0.057917 s
comm : 0.015460 s
total: 0.073377 s (234.132636 GFlops)
error: 0.000258
```

- baseに比べてかなり高速に．cache blockingとSIMDが強い

### my_answer (MPI + CUDA + shared memory)

- N=2048, np=1 (on f_node)

```
(b-thesis-takashima) 21M30689@r2i6n1:~/hpc_lecture_2021/16_final_report> nvcc my_matmul.cu -lmpi
(b-thesis-takashima) 21M30689@r2i6n1:~/hpc_lecture_2021/16_final_report> ./a.out
N    : 2048
comp : 0.052542 s
comm : 0.072600 s
total: 0.125142 s (137.283188 GFlops)
error: 0.000364
```

- N=2048, np=4 (on f_node)

```
(b-thesis-takashima) 21M30689@r2i6n1:~/hpc_lecture_2021/16_final_report> mpirun -np 4 ./a.out 
N    : 2048
comp : 0.033700 s
comm : 0.304231 s
total: 0.337931 s (50.838465 GFlops)
error: 0.000364
(b-thesis-takashima) 21M30689@r2i6n1:~/hpc_lecture_2021/16_final_report>
```

- compは早くなっているので，Nをより大きくしてnpを増やすと，commの増大よりcompの減少が優って高速に動作するかも．結果は正しそう．


|          | Topic                                | Sample code               |
| -------- | ------------------------------------ | ------------------------- |
| Class 1  | Introduction to parallel programming |                           |
| Class 2  | Shared memory parallelization        | 02_openmp                 |
| Class 3  | Distributed memory parallelization   | 03_mpi                    |
| Class 4  | SIMD parallelization                 | 04_simd                   |
| Class 5  | GPU programming 1                    | 05_openacc                |
| Class 6  | GPU programming 2                    | 06_cuda                   |
| Class 7  | Parallel programing models           | 07_starpu                 |
| Class 8  | Cache blocking                       | 08_cache_cpu,08_cache_gpu |
| Class 9  | High Performance Python              | 09_python                 |
| Class 10 | I/O libraries                        | 10_io                     |
| Class 11 | Parallel debugger                    | 11_debugger               |
| Class 12 | Parallel profiler                    | 12_profiler               |
| Class 13 | Containers                           |                           |
| Class 14 | Scientific computing                 | 14_pde                    |
| Class 15 | Deep Learning                        | 15_dl                     |
