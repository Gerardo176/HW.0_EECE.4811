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
