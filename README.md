# Word Frequency Counter – Sequential, Parallel (TBB), and Concurrent (Pthreads)

This project implements a word frequency counter in three different ways:
1. Sequential version: single-thread baseline
2. Parallel version (TBB): data parallel word counting using:
    **`parallel_reduce`**
    **`enumerable_thread_specific` (ETS)**
3. Concurrent version (Pthreads): producer–consumer model with mutex control

Each program reads a text file, normalises the words, counts their frequency, and outputs the top **N** most frequent words along with timing information.

---

## Implementations & Files
1. Sequential implementation**
  ** Source: `wordCountSeq.cpp`**
  ** Output binary: `seq`**
2. Parallel TBB implementation
  ** Source: `wordCountParallel.cpp`**
  ** Output binary: `parallel`**
3. Concurrent Pthreads implementation
  ** Source: `wordCountConcurrent.cpp`**
  ** Output binary: `conc`**
   
Make sure `seq`, `parallel`, and `conc` are compiled in the same folder.

---

## Build Instructions

1. Sequential
```bash
g++ -std=c++11 -o seq wordCountSeq.cpp

2. Parallel (TBB)
```bash
g++ -std=c++11 -march=native -flto \
    wordCountParallel.cpp \
    -I/opt/homebrew/opt/tbb/include \
    -L/opt/homebrew/opt/tbb/lib \
    -ltbb \
    -o parallel
3. Concurrent (Pthreads)
```bash
g++ -std=c++11 -pthread -o conc wordCountConcurrent.cpp


## Usage
1. Sequential
```bash
./seq <filename>

2. Parallel (TBB)
```bash
./parallel <filename> [threads] [strategy] [block_size]
**strategy = reduce or ets**

Example:
```bash
./parallel smallSize.txt 4 ets 5000

3. Concurrent (Pthreads)
```bash
./conc <filename> [workers] [topN]
Example:
```bash
./conc smallSize.txt 6 20

## Program Features
1. Word normalisation (remove punctuation, convert to lowercase)
2. Local maps per thread (parallel + concurrent versions)
3. Map merging at the end
4. Timing for:
**reading
**counting
**merging
**total runtime
5. Configurable number of threads / workers
6. Top-N most frequent words output

# Requirements
1. Both parallel and concurent programs can be compared against the sequential version for baseline timing.
2. TBB must be installed for the parallel version.
3. Pthreads are included by default on Linux and macOS.
4. Tested with g++ and C++11.
