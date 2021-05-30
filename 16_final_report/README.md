Student ID   : 21M30689
Student Name : Sora Takashima

base (MPI)                                          : example.cpp
my_answer (MPI + cache blocking + Open MP + SIMD)   : my_matmul.cpp

## Example

- prepare

```
21M30689@login0:~/hpc_lecture_2021/16_final_report> module load gcc/10.2.0 intel-mpi/19.9.304
21M30689@login0:~/hpc_lecture_2021/16_final_report> mpicxx my_matmul.cpp -fopenmp -march=native -O3
```

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