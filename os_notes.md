# Notes on Operating System:
---

- **Process** : An instance of program, e.g. an instance of chrom is one process and two instance of gedit are two saperate processes.
	- a process has at least one thread and a process has its own code and data (e.g. variables.)
	- two process cannot share their data space, if they want to share data they have to use <u>Queue or Pipe</u>
	

- **Thread** : the smallest sequance of instructions that can be managed by os.
	- a thread has its own stack and
registers.
	- OS can determine for when and how long a thread can run.
	- all threades can access process level data.	
	- just like processes, threads can also use queue or pipe to share data.
	
- [This](https://youtu.be/AZnGRKFUU0c?si=ujoqf5796oryhyLk) is a really good video about understanding threads in python.

### Understanding Multithreading with python:
---

#### **Benchmarking**:

- There are two ways to benchmark python code, first one is basic and can be used for 90% of the time which is **timeit** module, and other one is advance and used for those 10% of rare cases where everything needs to be accurate which is **pyperf** module, it also gives a lot of analysis and automates some stuff.

- ##### **timeit module**:
	- Basic syntax : **`python -m tiemit -s "setup code" -n num_of_times_to_execute "code to benchmark"`**
	
	- if you don't specify the -n parameter, timeit will try to run your code 1,2,5,10,20,50,... **until total execution time exceeds 0.2 seconds.**
	
	- the above was its CLI, we can also use the timeit.timeit function to achive same results inside a python script.
	
	- by default timeit() disables the garbage collector, to enable it do this for example:
	**`timeit.Timer('for i in range(10): oct(i)', 'gc.enable()').timeit()`**
	
	- the output of timeit returns three fields, first how many times the time loop has executed, second how many times the repetations of timeit statement has executed and last one is the best average time from given repetations, measn the total time to execute timeit divided by the number of time loops.
	
	- for example: **1000 loops, best of 5: 0.342 usec per loop** 
	here, the code is executed total 5000 times, where there were 5 time loops where each time loop executed 1000 times, and we get the best out of average time for each time loops that is 0.342 usec. 

	- the setup code and code to execute can be callables.
	
	- We also have a **Timer** class which has usefull methods like timeit, autorange, repeat and print_exc (print exception).

	
#### **Working with processes in linux**:

- **`ps`**: shows the processes associated with the current terminal session, we can use 'x' option to get processes regardless of which terminal they are, it will also show new column "STAT" means status of processes, it can be running, sleeping, stopped etc...

- **NOTHING IS SERVER AND NOTHING IS CLIENT, THERE IS ONLY TWO COMPUTERS TALKING WITH EACH OTHER WHICH CAN ONLY HAVE TWO ROLES : SENDER AND RECIVER, A COMPUTER CAN HAVE THOSE ROLES IN ORDER TO COMMUNICATE IN A MEANINGFUL WAY.**


### General:
---

- An OpenSSH server will set the variable $SSH_CLIENT, which contains the current ip, client port and server port separated by spaces:
`echo $SSH_CLIENT`

- disable apache2 service on boot: `systemctl disable apache2`, use `systemctl --all` to list all services and use `systemctl list-unit-files` to list all startable units.
