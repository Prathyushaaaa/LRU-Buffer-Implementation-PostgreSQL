# LRU-Buffer-Implementation-PostgreSQL

**Objective:**
The primary goal of this project is to study and modify the PostgreSQL source code. 
We will dive into one of the core modules of PostgreSQL: the buffer manager. 
Specifically, we will implement the Least Recently Used (LRU) buffer replacement policy by comprehensively understanding and modifying the existing PostgreSQL source code, which employs the clock buffer replacement policy.

**Installation:**
PostgreSQL is installed from the source code. The original files are replaced with the modified version that implements LRU buffer replacement policy and recompiled and installed. The test implementation has been done with 100 buffer pages. The given input file with the queries has been used for testing purpose.

**Description:**
Since, LRU does not use the *from_ring variable as it uses a queue for implementation purpose unlike the clock sweep algorithm which uses circular queue, the parameter is removed in the StrategyGetBuffer function in the freelist file. In the modified code, I removed the logic related to tracking whether the buffer was selected from a ring. It acquires a buffer from the freelist (if available) without checking for a strategy or a ring which makes it a straightforward buffer selection process. Added a function FreeListSpinLockRelease to the freelist file. This function is called in the bufmgr file in the BufferAlloc function. The code acquires a spin lock and checks if the buffer is already in the freelist, if not, it adds the buffer to the freelist. Added elog statements to StrategyGetBuffer and StrategyFreeBuffer functions in the freelist file to get an output whenever a free buffer is added to the queue. It acquires a spin lock and checks if the buffer is already in the freelist. If not, it adds the buffer to the freelist. Modified code in bufmgr file in the UnpinBuffer function to check the reference count of a buffer, and if the reference count is zero, it calls the StrategyFreeBuffer function to free the buffer.

**Files to Modify:**
~/src/backend/storage/buffer/buf_init.c
~/src/backend/storage/buffer/bufmgr.c
~/src/backend/storage/buffer/freelist.c
~/src/include/storage/buf_internals.h
