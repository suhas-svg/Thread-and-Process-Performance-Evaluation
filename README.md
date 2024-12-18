# Thread and Process Performance Evaluation Project

## Overview
This project explores the performance of **threads** and **processes** in various scenarios. It evaluates the overheads associated with thread creation, process creation, dynamic memory modifications, and parallel matrix multiplication using **Copy-On-Write (COW)** and other resource management techniques.

## Objective
The primary goal of this project is to:
- Understand **Copy-On-Write (COW)** and its impact on memory optimization.
- Measure and compare the time taken for:
  - Thread creation and deletion.
  - Process creation and deletion.
  - Parallel matrix multiplication using threads.
- Implement programs to evaluate performance metrics for threads and processes on multi-core systems.

## Files in the Project

### Source Files:
1. **`etime.c`**  
   - Contains the `etime()` function to calculate the elapsed time between two calls.
   - Uses `gettimeofday()` to measure time in microseconds, converting it to seconds.

2. **`et.c`**  
   - Evaluates the time taken for **thread creation** and **dynamic memory modification** using `pthread_create()` and `pthread_join()`.
   - Two modes:
     - `-a`: Modifies memory **after** the thread is created.
     - `-b`: Modifies memory **before** the thread is created.

3. **`ep.c`**  
   - Measures the time taken for **process creation** and **dynamic memory modification** using `fork()` and `waitpid()`.
   - Two modes:
     - `-a`: Modifies memory **in the child process** after it is forked.
     - `-b`: Modifies memory **in the parent process** before the child is forked.

4. **`para_mm.c`**  
   - Implements **parallel matrix multiplication** for two 400x400 matrices using multiple threads.
   - The number of threads is passed as a command-line argument.
   - Uses **semaphore synchronization** to ensure all threads complete before stopping the timer.

### Compilation Script:
5. **`makefile`**  
   - Automates the compilation of all programs using `gcc`.
   - Includes threading library (`-lpthread`) for thread-based programs.

### Output Files:
- **`et`**: Executable for thread creation performance evaluation.
- **`ep`**: Executable for process creation performance evaluation.
- **`para_mm`**: Executable for parallel matrix multiplication performance evaluation.

## Compilation Instructions
Run the following command in the terminal to compile all files:

```bash
make
```

This will generate the executables for all programs: `et`, `ep`, and `para_mm`.

To clean up the object files and executables, use:
```bash
make clean
```

## Execution Instructions

### 1. **Thread Creation and Modification (`et`)**
```bash
./et <mode> <size>
```
- **`<mode>`**: Either `-a` or `-b`.
  - `-a`: Modify memory **after** thread creation.
  - `-b`: Modify memory **before** thread creation.
- **`<size>`**: Size of memory to allocate in **KB** (e.g., 1024, 8192).

#### Example:
```bash
./et -a 1024
```

### 2. **Process Creation and Modification (`ep`)**
```bash
./ep <mode> <size>
```
- **`<mode>`**: Either `-a` or `-b`.
  - `-a`: Modify memory in the **child process** after `fork`.
  - `-b`: Modify memory in the **parent process** before `fork`.
- **`<size>`**: Size of memory to allocate in **KB** (e.g., 1024, 8192).

#### Example:
```bash
./ep -b 8192
```

### 3. **Parallel Matrix Multiplication (`para_mm`)**
```bash
./para_mm <num_threads>
```
- **`<num_threads>`**: Number of threads to use for matrix multiplication (e.g., 1, 2, 4, 8, 16).

#### Example:
```bash
./para_mm 4
```

## Expected Outputs
1. **`et` and `ep` Outputs**:
   - Reports the elapsed time for thread/process creation and memory modification.
   - Outputs vary based on:
     - Mode (`-a` or `-b`).
     - Memory size (e.g., 1024 KB, 8192 KB).

2. **`para_mm` Outputs**:
   - Reports the elapsed time for matrix multiplication.
   - Time decreases as the number of threads increases, up to the number of available CPU cores.

## Key Concepts Used

### 1. **Copy-On-Write (COW)**
- Ensures efficient memory usage by deferring memory duplication until a write operation occurs.

### 2. **Threading**
- `pthread_create()` and `pthread_join()` are used to create and join threads.
- Global variables are used to share data between threads.

### 3. **Process Management**
- `fork()` is used to create child processes.
- `waitpid()` ensures the parent process waits for the child process to complete.

### 4. **Semaphores**
- Synchronizes threads in `para_mm.c` to ensure all threads complete before calculating elapsed time.

### 5. **Matrix Multiplication**
- Divides the workload among threads to achieve parallel computation.
- Each thread is assigned a slice of the matrix.

## Observations

1. **Thread vs. Process Performance**:
   - Threads are faster to create and join compared to processes.
   - Processes have higher overhead due to memory duplication and resource allocation.

2. **Matrix Multiplication**:
   - Performance improves significantly as the number of threads increases, up to the number of available CPU cores.
   - Beyond the optimal thread count, performance gains diminish due to thread management overhead (as per Amdahl's Law).

## Notes
- Ensure the number of threads does not exceed the number of CPU cores for optimal performance in `para_mm.c`.
- Use appropriate memory sizes for `et` and `ep` to avoid memory allocation failures.

## Example Workflow
1. Compile the project:
   ```bash
   make
   ```
2. Test thread performance:
   ```bash
   ./et -a 1024
   ```
3. Test process performance:
   ```bash
   ./ep -b 8192
   ```
4. Test parallel matrix multiplication with 4 threads:
   ```bash
   ./para_mm 4
   ```
5. Clean up:
   ```bash
   make clean
   ```

## Author
This project was implemented as an exploration of thread and process performance on multi-core systems. It leverages concepts like **Copy-On-Write**, semaphores, and parallel computation.
