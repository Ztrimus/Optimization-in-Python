# What I Learned
- `%run file_name.py` : In Ipython this magic command which run python scripts
- `%run -n file_name.py` only load functions from that python scripts, not run whole script.
- In terminal `python -m cProfile prof.py`, m ->  run library module as a script
- A decorators lets you extends a function behavior without changing its code.

# Measuring Time
1. module
	- time module
		- perf_counter()
	- timeit module
		- in ipython
		`%timeit function_name`

# CPU Profiling
 - "cProfile" is recommended by python
 	```python
 	import cProfile
 	cProfile.run("function_name(args)")
 	```
 	in Ipython
 	`%prun function_name(args)`

 - Type of profiler
 	1. Deterministic Profiler - Record every function call, return and exception
 	2. Statistical profiler - records where thr program is at small intervals
 - pstats - to displays profiler-generated statics files
 	- include below line in your code
 	```python
 	import cProfile
 	cProfile.run("function_name(args)", filename='prof.out')
 	```
 	then run code script in terminal as
 	```shell
 	python code.py
 	python -m pstats prof.out
 	```
 	- after that options in pstats
 		- stats 10 - show first ten
 		- sort cumtime - sort stats by cumtime column
 - SnakeViz - Fanicer UI than pstats
 	- in terminal `snakeviz prof.out` opens in web browser

# line_profiler & kernprof
 - **This will show time taken by each line of function.**
 - it can be used to measure with finer granularity than just functions
 - it is not in std. library
 - It Include a command line program called "kernprof"
 - Used @proflie decorators to select which parts of the code to profile.
 - add make changes in code according to folowing line
	```python
	@profile
	def function_name(args):
		# do_something_with args
		pass
	```

	then run in the code file like
	```shell
	>>> kernprof -f code.py
	Wrote profile results to code.py.lprof
	>>> python -m line_profiler code.py.lprof
	```
 - line_profiler in **IPython**
 	- remove @profile from script
 	- then go to ipython
 	```python
 	%run -n code.py # will load only function not run whole script
 	%load_ext line_profiler
 	%lprun -f fn_to_display fn_to_run(args)
 	```

# Tracing Memory Allocations
- If data is smaller, it has great chance to fit in cache. and runtime will be faster.
- Tracemalloc
	- Tool to understand memory allocations
	- good tool to investigate memory leak in program 
- syntax
```python
import tracemalloc
# line of codes
# line of codes
snapshot = tracemalloc.take_snapshot()
for stat in snapshot.statistics('lineno')[:10]:
	print(stat)
```
- 