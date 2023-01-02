```makefile
targets: prerequisites
	command1
	command2
	...
```

**g++**

`g++命令：g++ main.cpp file1.cpp file2.cpp -o main`

```makefile
CC = g++

main: main.o file1.o
	$(CC) main.o message.o -o output

main.o: main.cpp
	$(CC) -c main.cpp
	
file1.o: file1.cpp
	$(CC) -c file1.cpp

clean:
	rm *.o
```

**Medium-Sized C/C++ Projects**

```makefile
TARGET ?= a.out
SRC_DIRS ?= ./src

SRCS := $(shell find $(SRC_DIRS) -name *.cpp -or -name *.c -or -name *.s)
OBJS := $(addsuffix .o,$(basename $(SRCS)))
DEPS := $(OBJS:.o=.d)

INC_DIRS := $(shell find $(SRC_DIRS) -type d)
INC_FLAGS := $(addprefix -I,$(INC_DIRS))

CPPFLAGS ?= $(INC_FLAGS) -MMD -MP

$(TARGET): $(OBJS)
	$(CC) $(LDFLAGS) $(OBJS) -o $@ $(LOADLIBES) $(LDLIBS)

.PHONY: clean
clean:
	$(RM) $(TARGET) $(OBJS) $(DEPS)

-include $(DEPS)
```

**Build system**

Tool/system to automate building software.

**GNU Make**

```make
target: prerequisites
	recipe
# $@: target 
# $^: prerequisites
# $?: all prerequisites newer than the target
# $<: first prerequisite
app: f1.o f2.o f3.o f4.o
	gcc -o $@ $^
	
$(<var>:<pattern>=<replacement>)
$(wildcard <pattern>)
e.g.:
FS_SOURCES=$(wildcard *.cpp)
FS_OBJS=${FS_SOURCES:.cpp=.o}

# use % to match file names
%.o : %.c
	gcc -c -o $@ $<
```

