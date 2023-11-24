---
title:             "MPI Introduction"
date:              2023-11-25
permalink:         /posts/2023/11/25/MPI-introduction
tags:              MPI
---

This post is my note on MPI coding.

# History of MPI

Before the 1990’s, programmers weren’t as lucky as us. Writing parallel applications for different computing architectures was a difficult and tedious task. At that time, many libraries could facilitate building parallel applications, but there was not a standard accepted way of doing it.

During this time, most parallel applications were in the science and research domains. The model most commonly adopted by the libraries was the message passing model. What is the message passing model? All it means is that an application passes messages among processes in order to perform a task. This model works out quite well in practice for parallel applications. For example, a manager process might assign work to worker processes by passing them a message that describes the work. Another example is a parallel merge sorting application that sorts data locally on processes and passes results to neighboring processes to merge sorted lists. Almost any parallel application can be expressed with the message passing model.

Since most libraries at this time used the same message passing model with only minor feature differences among them, the authors of the libraries and others came together at the Supercomputing 1992 conference to define a standard interface for performing message passing - the Message Passing Interface. This standard interface would allow programmers to write parallel applications that were portable to all major parallel architectures. It would also allow them to use the features and models they were already used to using in the current popular libraries.

By 1994, a complete interface and standard was defined (MPI-1). Keep in mind that MPI is only a definition for an interface. It was then up to developers to create implementations of the interface for their respective architectures. Luckily, it only took another year for complete implementations of MPI to become available. After its first implementations were created, MPI was widely adopted and still continues to be the de-facto method of writing message-passing applications.

# Basic MPI conceptions

   1. The MPI coding model follows SPMD (single program multiple data) and loosely synchronous principle.
   2. Communicators: a series of processes sending and receiving data from each other. They are labeled by rank.
   3. Point-to-point communication: a sender and a receiver.
   4. Collective communication: a process communicates with all other processes.

# Commonly used general APIs

   1. `MPI_Init(int* argc, char*** argv)`
   2. `MPI_Comm_size(MPI_Comm communicator, int* size)`
   3. `MPI_Comm_rank(MPI_Comm communicator, int* rank)`
   4. `MPI_Finalize()`
   5. `int MPI_Test(MPI_Request *request, int *flag, MPI_Status *status)`
   6. `int MPI_Wait(MPI_Request *request, MPI_Status *status)`

```cpp
#include <mpi.h>

int main(int argc, char* argv[])
{
    int npes, myrank;
    MPI_Init(&argc, &argv);
    MPI_Comm_size(MPI_COMM_WORLD, &npes);
    MPI_Comm_rank(MPI_COMM_WORLD, &myrank);
    printf("From process %d out of %d, Hello World!\n", myrank, npes);
    MPI_Finalize();
}
```

# Point-to-point communication APIs

   1. `int MPI_Send(void *buf, int count, MPI_Datatype datatype, int dest, int tag, MPI_Comm comm)`
   2. `int MPI_Recv(void *buf, int count, MPI_Datatype datatype, int source, int tag, MPI_Comm comm, MPI_Status *status)`
   3. `int MPI_Isend(void *buf, int count, MPI_Datatype datatype, int dest, int tag, MPI_Comm comm, MPI_Request *request)`
   4. `int MPI_Irecv(void *buf, int count, MPI_Datatype datatype, int source, int tag, MPI_Comm comm, MPI_Request *request)`

Non-buffered communication: the `send` function does not return until the corresponding `receive` finishes. Apparently, this is not optimal because of low efficiency and possible deadlock.

Blocking communication: the `send` function copy the data into a given buffer. Correspondingly, the `receive` function take data out from the buffer. Deadlock can still happen in this case.

Non-blocking communication: the `send` and `receive` function directly return without checking the status. Programmers need to manually check.

# Collective communication APIs

   1. `MPI_Bcast(void* data, int count, MPI_Datatype datatype, int root, MPI_Comm communicator)` using a tree-like broadcasting algorithm to improve communication efficiency.
   2. `MPI_Scatter(void* send_data, int send_count, MPI_Datatype send_datatype, void* recv_data, int recv_count, MPI_Datatype recv_datatype, int root, MPI_Comm communicator)`
   3. `MPI_Gather(void* send_data, int send_count, MPI_Datatype send_datatype, void* recv_data, int recv_count, MPI_Datatype recv_datatype, int root, MPI_Comm communicator)`
   4. `int MPI_Barrier(MPI_Comm comm)`
   5. `int MPI_Reduce(void *sendbuf, void *recvbuf, int count, MPI_Datatype datatype, MPI_Op op, int target, MPI_Comm comm)`
   6. `int MPI_Allreduce(void *sendbuf, void* recvbuf, int count, MPI_Datatype datatype, MPI_Op op, MPI_Comm comm)`
   7. `int MPI_Alltoall(void *sendbuf, int sendcount, MPI_Datatype senddatatype, void *recvbuf, int recvcount, MPI_Datatype recvdatatype, MPI_Comm comm)`

# Deadlock

This is somehow advanced for my current knowledge. Will come back to finish this part sometime.