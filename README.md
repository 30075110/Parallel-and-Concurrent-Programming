# Word Frequency Counter – Sequential, Parallel (TBB), and Concurrent (Pthreads)
<br>
This project implements a word frequency counter in three different ways:

1. **Sequential version**: single thread baseline  
2. **Parallel version (TBB)**: data parallel word counting using:  
   - `parallel_reduce`  
   - `enumerable_thread_specific` (ETS)  
3. **Concurrent version (Pthreads)**: producer–consumer model with mutex control  

Each program reads a text file, normalises the words, counts their frequency, and outputs the top **N** most frequent words along with timing information.

---

## Files
### 1. Sequential
- Source: `wordCountSeq.cpp`  


### 2. Parallel (TBB)
- Source: `wordCountParallel.cpp`  

### 3. Concurrent (Pthreads)
- Source: `wordCountConcurrent.cpp`  
<br>

---

## Build Instructions

### 1. Sequential
```bash
g++ -std=c++11 -o seq wordCountSeq.cpp
```

### 2. Parallel (TBB)
```bash
g++ -std=c++11 -march=native -flto \
    wordCountParallel.cpp \
    -I/opt/homebrew/opt/tbb/include \
    -L/opt/homebrew/opt/tbb/lib \
    -ltbb \
    -o parallel
```

### 3. Concurrent (Pthreads)
```bash
g++ -std=c++11 -pthread -o conc wordCountConcurrent.cpp
```

---

## Usage
### 1. Sequential
```bash
./seq <filename>
```

### 2. Parallel (TBB)
```bash
./parallel <filename> [threads] [strategy] [block_size]
```
- strategy = reduce or ets
- Example:
```bash
./parallel smallSize.txt 4 ets 5000
```

### 3. Concurrent (Pthreads)
```bash
./conc <filename> [workers] [topN]
```
- Example:
```bash
./conc smallSize.txt 6 20
```
---

## Program Features 
- Word normalisation (remove punctuation, convert to lowercase) 2. 
- Local maps per thread (parallel + concurrent versions)
- Map merging at the end
- Timing for: **reading **counting **merging **total runtime
- Configurable number of threads / workers
- Top-N most frequent words output

---

## Requirements 
- Both parallel and concurrent programs automatically run the sequential version first for baseline timing.
- Requires TBB to be installed for the parallel version.
- Pthreads are included by default on Linux/macOS.
- Make sure `seq`, `parallel`, and `conc` are compiled in the same folder.
