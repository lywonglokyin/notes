## Type of parallelization of hardware

1. Single instruction stream, single data stream (**SISD**)

    For each cycle, the processing unit fetch one instruction and one data to produce one result.

    Basically no parallelism.

2. Single instruction stream, multiple data stream (**SIMD**)

    For each cycle, the processing unit fetch one instruction and multiple data to produce multiple results.

    - MMX: MultiMedia eXtension, a single instruction can be applied to two 32-bit integer, four 16-bit integers, or eight 8-bit integers at once.
    - SSE: Streaming SIMD Extension, a SSE instruction can perform 4 single-precision or 2 double-precision operations
    - AVX: Advanced vector extension, a AVX-256 instruction can perform 8 single-precision or 4 double-precision operations, and a AVX-512 instruction can perform 16 single-precision or 8 double-precision operations.
    - GPU: Graphics processing unit

3. Multiple instruction stream, single data stream (**MISD**)

    Uncommon. Might be used for verification of computation results.

4. Multiple instruction stream, multiple data stream (**MIMD**)

    Example: multi-core system, multi-processor system, cluster

## Memory systems

There are mainly two types: Shared-memory system, and distributed memory system.

### Uniform memory access (UMA)
- Shared-memory system
- all processor can directly access main memory
- each processor can equal memory accesing time and speed.

### Non-uniform memory access (NUMA)
- Shared-memory system
- Each cpu can an assigned RAM
- The processor can access other's memory with special hardware like QPI or UPI.
- Access speed of memory relies on the distance where the processor is placed.

