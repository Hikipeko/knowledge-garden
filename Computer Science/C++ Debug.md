### Debug Segment

```c
#ifdef DEBUG_PRINT
printf("This be a debug message\n");
#endif

#ifdef DEBUG_PRINT
	#define dbgprintf(fmt, ...) printf("DEBUG: " fmt, ##__VA_ARGS__)
#else
	#define dbgprintf(fmt, ...)
#endif

dbg_printf("This be a debug message\n");
```

#### GDB

```shell
gdb [options] [executable] [core file]
r -> run
n -> next
b -> break
tui enable
tui disable
# commands
run [arguments] [file redirects]
next [count]: step overfunction
step [count]: step into function
finish: step out of current function
print <expression>: print variables
list [location]: list source code
# breakpoints
break <locaton>: set breakpoint
break main.cpp:21 if argc == 4
break coolfunction
# Watchpoints: stop when an expresison changes
watch <expression>: set watchpoint
watch a + b
disable 1
delete 1

info breakpoints, info watchpoints: list break/watchpoints
where, backtrace, bt: list stack frames
frame <stack frame>: change stack frame
```



### Valgrind

Memory leak check tool

`.valgrind --leak-check=full --show-leak-kinds=all ./program`



### Perf

Runtime ==profiling== tool

```shell
make profile
perf record --call-graph=dwarf -e cycles:u time ./program_debug < inputfile
perf report
```

### Time

**Measuring Time in C++**

```c++
#include <chrono>
Timer t;
t.start();
doStuff();
t.stop();
cout<<t.seconds();
t.reset();
```

**Produce runtime report**

``/usr/bin/time ./progName -options args`
