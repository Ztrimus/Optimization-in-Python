## What I have learned
- `%run file_name.py`: In IPython this magic command will run python scripts.
- `%run -n file_name.py`: In IPython this magic command only load functions from that python scripts, not run the whole script.
- In the terminal, **"`m`"** argument in  **`python -m cProfile prof.py`** run library module as a script.
- Decorators let you extend a function behavior without changing its code.

## Read
- "Rob Pike's Five rules of programming"
- [Awesome Falsehoods Programmers Believe About Time article](https://goo.gl/K9wvL)
## General tips while optimizing
- Profile before optimizing. Profiling measures where code spends time.
- Use real data to optimizing.
- Turn off anti-virus programs while coding (windows).
- Have a test suite ready.
- Know when to measure (where to measure in which part of code).

## Measuring Time
Python Modules to measure time:
 1. time module
	 - `time.perf_counter()`
	 - `time.time()`
 2. timeit module
		- in ipython
		`%timeit function_name`

## CPU Profiling
 - Type of Profiler :
 	1. **Deterministic Profiler** - Records every function call, return and an exception.
 	2. **Statistical profiler** - Records where the program is at small intervals
1. **`cProfile`** is recommended by python for profiling
 	``` python
 	import cProfile
 	cProfile.run("function_name(args)")
	``` 
 	in IPython
 	`%prun function_name(args)`
 	
2. **`pstats`** - to displays profiler-generated statics files. 
	 - Include below line in your code:
		```python
	 	import cProfile
	 	cProfile.run("function_name(args)", filename='prof.out')
	 	```
	 - then run code script in terminal as:
	   ```bash
	 	>>> python code.py
	 	>>> python -m pstats prof.out
	 	```
 	- after that you can see state using different options in `pstats`
	 	- `stats 10` - show first ten stat.
 		- `sort cumtime` - sort stats by cumtime column.
3.  **SnakeViz** - Fanicer UI than pstats
 	- In terminal, type `snakeviz prof.out`. it will open in a web browser.

## line_profiler & kernprof
- This will show **time taken by each line** of function.
- It can be used to measure with **finer granularity** than just functions.
- It is not in standrd library.
- It include a command line program called **"`kernprof`"**.
- Use @proflie decorators to select which parts of the code to profile and make changes in code according to following:
	```python
	@profile
	def function_name(args):
		# do_something_with args
		# instruction 1
		pass
	```
	then run in the code file like
	```bash
	>>> kernprof -f code.py
	Wrote profile results to code.py.lprof
	>>> python -m line_profiler code.py.lprof
	```
 - **"`line_profiler`"** in IPython
 	- remove @profile from script
 	- then go to ipython
 	```python
 	%run -n code.py # will load only function not run whole script
 	%load_ext line_profiler
 	%lprun -f fn_to_display fn_to_run(args)
 	```
## Tracing Memory Allocations
- If data is smaller, it has great chance to fit in cache and runtime will be faster.
- **Tracemalloc** : `tracemalloc`
	- Tool to understand memory allocations.
	- Good tool to investigate memory leak in program.
	```python
	import tracemalloc
	# line of codes
	# line of codes
	snapshot = tracemalloc.take_snapshot()
	for stat in snapshot.statistics('lineno')[:10]:
		print(stat)
	```
