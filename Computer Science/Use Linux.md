### Linux Basic

**POSIX** is a set of standards for the system.

**Components**

**1. Kernel**

Software that serves as the intermediary between hardware resources and user applications.

Handle multi-tasking, security file system, device drivers. 

**2. Library**

Reusable pre-written software you can call upon

**3. Applications**

Files serve as s sort of universal interface.

```
# show all the threads
ps
pgrep
```

**Process** #link 482

1. PID
2. Associated user
3. Current working directory
4. Environment variables (PATH, PWD, USER, HOME)
5. File descriptor table (terminal has a file interface)

**Process Creation**

1. `fork()`
2. `exec(), execvp("ls", args)`

```unix
# show file type
file <filename>
# change file access mode
chmod 600 secure-me
chmod u+x some-file
cat /prod/cpuinfo
```

**Command Line**

A type of interface where you provide a line of text that the interpreting software (shell) can interpret into commands to perform.



### Shell

Interpret its input as "create more processes".

cd is a built-in by the shell

the shell process doesn't change working directory when cd

```shell
# run in the background
sleep 5 &
# show current processes
jobs
# foreground process n
fg %n
# have the shell give up ownership of a process
disown
# give the exit status of the last command
echo $?
# set variable
x=1
echo $x
# append & file redirection
echo hello >> text
rev << CAT
rev <<< "here's a string!"
```

^C stop the program

^Z suspend the program



### Commands

**Basic Commands**

```
man
pwd
ls
cd
mv
cp
touch
rm
grep
cat
```

**Pipe line**

```
echo "hello world" | rev
```

**Script**

Save a list of commands into a file.

##### SSH

Security Shell Protocol is a [[Cryptography 376|cryptographic]] network protocol for operating network services securely over an unsecured network.

Allowing for secure connection of log into to another system.
![[Pasted image 20221222231232.png]]

Remember sometimes you see "Are you sure to continue connecting (yes/no)?" This is to make sure the server you are connected to is not a pretender.

**Login without password**

Store the client's PubKey inside server's `%~/.ssh/authorized_keys`.

![[Pasted image 20221222232404.png]]



### Shell Script

**String together commands**

```shell
# run if succeed
cmd1 && cmd2
# run is failed
cmd1 || cmd2
# run after
cmd1 ; cmd2
# pipeline
cmd1 | cmd2
x = 1145
echo ${x}14
# set to default

```

**File expansion**

```shell
# get string from command
i=$(whoami)
# match all strings
*.txt
# match a single char
disk.in?
# match a set
disk[1234].in
# replace output of command with value
echo $(rev <<< hello)
```

**Not And Or**

```shell
echo ! [ 100 -eq 100 ] && [ $a -lt $b ]
```

**Exe file**

```shell
#!/bin/sh
```

**Shell script**

```shell
# if
if test-commands; then
	commands
elif more-test-commands; then
	commands
else
	commands
fi

# while
while test-commands; do
	commands
done

# for
for var in list; do
	commands
done

#input
$n # n th argument
$@ # list of all arguments
$# # number of argume 
```
