**Gerardo Aguirre EECE.4811 Fall 2026 HW.0**


**Questions**

1. Why can’t two ordinary processes simply communicate by reading and writing normal variables in each other’s address spaces?

Each process normally has its own separate virtual address space, so a variable in one process is not directly accessible to another. 

2. A pipe has a read end and a write end. What direction does information flow through a pipe?

Data flows from the pipe’s write end to its read end. One process writes data into the pipe, and another process reads that data from the other end.

3. What happens when a process tries to read from an empty pipe? How can this behavior be useful for synchronization?

If the pipe is empty and a writer is still connected, a normal blocking read() waits until data becomes available. This can synchronize processes because the reader automatically waits until the writer has produced something.

4. Why is it good practice for each process to close pipe ends that it does not use?

Closing unused pipe ends prevents resource leaks and ensures that the operating system can correctly detect when no writers or readers remain.

5. Does sending 1, 2, 3, 4, 5 through one pipe guarantee the printed order Producer 1, Consumer 1, Producer 2, Consumer 2, etc.?

No. The pipe preserves the order of the data, but the scheduler may allow the producer to print and write several values before the consumer gets a chance to run. The design needs additional synchronization, such as a second pipe used for acknowledgments so the producer waits for the consumer after each value.


**Code description and break-down**

This program demonstrates inter-process communication between two separate OS processes: a Producer and a Consumer. The Producer generates the integers 1 through 5 and sends them to the Consumer using a pipe.

A second pipe is used by the Consumer to send an acknowledgment back to the Producer. This ensures that the Producer does not produce or print the next number until the Consumer has received and printed the previous number.

The output will follow this order:
-----------------------------------------------------------
Producer: 1

Consumer: 1

Producer: 2

Consumer: 2

Producer: 3

Consumer: 3

Producer: 4

Consumer: 4

Producer: 5

Consumer: 5

-----------------------------------------------------------
The program creates two pipes before calling fork().

The first pipe is used to send integers from the Producer process to the Consumer process. The second pipe is used to send an acknowledgment from the Consumer back to the Producer after each value has been printed.

After sending a number, the Producer blocks while waiting for an acknowledgment. Because of this synchronization, the Producer cannot print the next number until the Consumer has printed the current number.

Each process closes the pipe ends that it does not use, and all remaining pipe descriptors are closed when communication is finished. The Producer also uses waitpid() to wait for the Consumer process to terminate.

**Compiling and running the code**

Used Windows Subsystem for Linux and
we compile using 

gcc producer_consumer.c -o producer_consumer

Then we run using 
./producer_consumer

**libraries used**

stdio.h — printing and error messages

stdlib.h — program exit values

unistd.h — pipe(), fork(), read(), write(), and close()

sys/wait.h — waitpid()

errno.h — error handling
