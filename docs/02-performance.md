# Performance

Good code should not take unnecessarily long to run or use more resources than are available.

Performance is mainly concerned with two things:

- execution time
- memory usage

The goal is not to make every piece of code as fast as possible. The goal is to make sure the code meets its actual requirements without wasting unnecessary resources.

---

## Premature Optimization

A common mistake in software engineering is trying to optimize code before knowing whether optimization is actually necessary.

A useful principle is:

> Measure first. Optimize second.

Before optimizing, you should know:

- what the performance requirements are;
- whether the current implementation actually violates them;
- where the real bottleneck is.

If the dataset is small or the code already meets its requirements, further optimization may add complexity without providing meaningful value.

### Check

- [ ] Does this code actually need optimization?
- [ ] Do I know the performance requirements?
- [ ] Is the dataset large enough for performance to matter?
- [ ] Have I measured the current performance?
- [ ] Do I know where the bottleneck is?
- [ ] Am I optimizing based on measurements rather than intuition?

---

## Choose the Right Algorithm

Different implementations of the same task can have very different performance characteristics.

The algorithm you choose determines how much work the program needs to perform.

For example, unnecessary nested loops can cause the same collection to be processed many more times than necessary.

```python
for item_a in items:
    for item_b in items:
        compare(item_a, item_b)
```

This may be appropriate in some cases, but if the same result can be obtained with fewer operations, the simpler algorithm will usually scale better.

### Check

- [ ] Is there unnecessary repeated computation?
- [ ] Are nested loops really necessary?
- [ ] Can the number of operations be reduced?
- [ ] Will this algorithm still perform well if the dataset grows?

---

## Choose the Right Data Structure

The data structure you choose can have a major impact on performance.

Different structures are optimized for different operations.

For example, repeatedly searching through a list may be slower than using a structure designed for fast lookup, such as a dictionary or set.

```python
# Repeated search
if value in my_list:
    ...

# Fast lookup structure
if value in my_set:
    ...
```

The best data structure depends on the operation being performed, the size of the data, and memory requirements.

### Check

- [ ] Is the data structure appropriate for the operation?
- [ ] Am I repeatedly searching through a collection?
- [ ] Would a `dict` or `set` improve lookup performance?
- [ ] Would an array-like structure be more appropriate?
- [ ] What are the memory trade-offs?

> Data structures are covered in more detail in the dedicated Data Structures section.

---

## Prefer Built-in Functions

When Python already provides an implementation for a common operation, it is usually better to use it than to recreate the same behavior manually.

Built-in functions and many standard-library utilities are highly optimized and may rely on lower-level implementations.

Useful modules include:

```python
collections
itertools
```

For example:

```python
from collections import Counter

counts = Counter(values)
```

may be both clearer and faster than manually implementing the same counting logic.

### Rule of Thumb

Before manually implementing a common operation, check whether Python or its standard library already provides it.

### Check

- [ ] Does Python already provide a built-in solution?
- [ ] Does the standard library solve this problem?
- [ ] Am I recreating something already available in `collections`, `itertools`, or another module?

---

## Advanced Optimization Techniques

Some performance problems require approaches beyond improving normal Python code.

These techniques should usually be considered only after measuring performance and identifying the actual bottleneck.

### Compiling Python

Python code can sometimes be accelerated using tools that compile or execute portions of the code differently.

Examples include:

- Cython
- Numba
- PyPy

These tools use different strategies.

- **Cython** extends Python with features that allow code to be compiled into C.
- **Numba** uses just-in-time compilation for supported numerical Python code.
- **PyPy** is an alternative Python implementation with a JIT compiler.

### Check

- [ ] Have simpler optimizations already been tried?
- [ ] Has profiling shown that this part of the code is actually a bottleneck?
- [ ] Is the expected speed improvement worth the added complexity?

---

## Asynchronous Code

Asynchronous programming can improve performance when a program spends significant time waiting for external resources.

Common examples include:

- API requests
- network calls
- database operations
- file I/O

Instead of waiting for one operation to finish before starting another, asynchronous code can make progress on other work.

### Important

Async programming is especially useful for **I/O-bound workloads**.

It does not automatically make CPU-heavy computations faster.

### Check

- [ ] Is the program spending significant time waiting for external resources?
- [ ] Is this an I/O-bound task?
- [ ] Could multiple waiting operations overlap?

---

## Parallel and Distributed Computing

### Parallel Computing

Parallel computing means executing work on multiple processors or CPU cores.

In Python, one option is:

```python
multiprocessing
```

This can be useful for CPU-heavy tasks that can be divided into independent pieces.

### Distributed Computing

Distributed computing means splitting work across multiple machines.

This is useful when the workload is too large for one machine or when large datasets need to be processed at scale.

### Check

- [ ] Is the workload CPU-bound?
- [ ] Can the work be divided into independent tasks?
- [ ] Is one machine insufficient for the workload?
- [ ] Does the added complexity justify parallel or distributed execution?

---

# Measuring Performance

Optimization should be driven by evidence.

A useful workflow is:

```text
Requirements
    ↓
Measure
    ↓
Identify bottleneck
    ↓
Optimize
    ↓
Measure again
```

---

## Timing Your Code

The simplest way to investigate performance is to measure how long a piece of code takes to run.

For small snippets or notebook cells, IPython provides:

```python
%timeit
```

and:

```python
%%timeit
```

These tools repeatedly execute the code and provide timing information.

### Make One Change at a Time

When optimizing, make one meaningful change and then measure again.

If several things are changed at once, it becomes difficult to know which change caused the improvement or slowdown.

### Check

- [ ] Did I measure the baseline?
- [ ] Did I change only one relevant thing?
- [ ] Did I measure again afterwards?
- [ ] Was the improvement actually meaningful?

---

## Profiling

Timing tells you:

> How long did this take?

Profiling helps answer:

> Where is the program spending its time?

This is especially useful for longer functions and scripts.

### `cProfile`

`cProfile` is Python's built-in profiler.

It records information about function calls and execution time, helping identify which parts of a program consume the most runtime.

Example:

```bash
python -m cProfile my_script.py
```

Or from Python:

```python
import cProfile

cProfile.run("my_function()")
```

Some useful information returned by `cProfile` includes:

- number of function calls;
- total time spent in a function;
- time spent in the function excluding subcalls;
- cumulative time including subcalls.

The goal is not to optimize every function.

The goal is to identify where most of the runtime is actually being spent.

### Rule of Thumb

> Use timing to confirm that performance is a problem.  
> Use profiling to find where the problem is.

---

## Memory Profiling

Runtime is only one part of performance.

Memory consumption can also become a bottleneck, especially as datasets grow.

High memory usage can:

- exceed available RAM;
- cause swapping;
- increase runtime;
- make applications unstable;
- limit the size of datasets that can be processed.

### Memray

`Memray` is a memory profiler developed by Bloomberg.

It can help identify:

- where memory is allocated;
- which parts of a program allocate the most memory;
- allocation patterns over time;
- memory bottlenecks.

Memory profiling becomes especially important when the same code needs to run on significantly larger datasets than those used during development.

### Check

- [ ] Does memory usage grow significantly with dataset size?
- [ ] Could this code exceed available RAM?
- [ ] Am I keeping unnecessary copies of large objects?
- [ ] Are large intermediate objects being retained longer than necessary?
- [ ] Should memory usage be profiled?

---

# Time Complexity

Time complexity describes how the runtime of an algorithm grows as the size of its input increases.

It describes the growth pattern of an algorithm rather than the exact execution time on a specific computer.

This is useful because hardware can change, while the scaling behavior of the algorithm remains the same.

## Big O Notation

Big O notation describes how an algorithm behaves as the amount of input data grows.

A useful way to think about it is:

> If my dataset becomes 10 times larger, how much more work will this algorithm need to perform?

### O(1) — Constant Time

The runtime does not grow with the size of the dataset.

Example:

```python
last_value = values[-1]
```

Accessing one element by index takes approximately the same amount of work regardless of whether the list contains 10 elements or 10 million.

### O(n) — Linear Time

Runtime grows approximately in proportion to the size of the input.

```python
for value in values:
    process(value)
```

If the dataset doubles, the amount of work also tends to approximately double.

### O(n²) — Quadratic Time

Runtime grows approximately with the square of the input size.

```python
for value_a in values:
    for value_b in values:
        compare(value_a, value_b)
```

If the dataset doubles, the amount of work can grow by roughly four times.

Quadratic algorithms can therefore become expensive very quickly as datasets grow.

---

## Think About Scale

Code that performs well on a small dataset may perform poorly when the amount of data increases.

Always consider how today's solution will behave with tomorrow's data size.

### Check

- [ ] What is the approximate time complexity?
- [ ] How does the algorithm behave as the input grows?
- [ ] Is there an avoidable O(n²) operation?
- [ ] Could another algorithm scale better?
- [ ] Am I testing only on unrealistically small datasets?

---

# Performance Checklist

Before optimizing code, ask:

- [ ] Does this code actually need optimization?
- [ ] Do I know the performance requirements?
- [ ] Have I measured the current runtime?
- [ ] Have I identified the real bottleneck?
- [ ] Am I making one optimization at a time?
- [ ] Did I measure again after the change?
- [ ] Is the algorithm appropriate?
- [ ] Am I performing unnecessary repeated work?
- [ ] Is the chosen data structure appropriate?
- [ ] Is there a Python built-in or standard-library implementation I should use?
- [ ] Does the solution scale well as the dataset grows?
- [ ] Have I considered time complexity?
- [ ] Is memory usage acceptable?
- [ ] Should memory usage be profiled?
- [ ] Is this an I/O-bound problem that could benefit from async?
- [ ] Is this a CPU-bound problem that could benefit from parallelism?
- [ ] Do I actually need compilation or distributed computing?

---

# Key Principles

```text
Don't optimize blindly.

Measure first.

Find the bottleneck.

Optimize the bottleneck.

Measure again.
```

Performance is not about making every line of code as fast as possible.

It is about making sure the system meets its requirements while keeping the implementation as simple and maintainable as possible.
