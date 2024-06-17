# Day 1

*17/06/2024*

## A RISC-V vector CPU for HPC

*Filippo Mantoni from Barcelona Computing Center*

[https://github.com/riscv]

### Barcelona Computing Center

Mare Nostrum Cluster, now they are building the new MareNostrum 5 cluster.

What they do?

- Cmputer Sciences
- Earth Sciences
- Life Sciences

Mantovani's team works on Mobile and Embedded based HPC lab

The tech in our phone is ARM (different from Intel), exploded in 2010 till now.
Fugaku cluster is powered by ARM technology (it was the most powerful cluster on the world).

They take mobile systems, builds cluster with them (removing android and installing linux)
They use TRL to build their systems.
TRL is how to build an idea, prototype and create stuff (from TR1 to TR9)

*See NotOnlyFLOPs team and competition*

### Why RISC-V?

RISC-V is a free standard instructions set (it doesn't means that is a free CPU).
Using it you can develop your own CPU. (With Intel and ARM is illegal, RISC-V no).

You can build and sell your own implementation of a CPU with RISC-V.

Istruction Set Architecture (ISA) is the most important interface because it regulates how software interacts with hardware.

#### Short history of ISAs

1. Intel x86-64 (AMD64)
2. SPARC (dead)
3. MIPS (dead)
4. IBM Power (dead)
5. Arm

The real value behind RISC-V is to try to agree on a standard to have a system that is going to be maintained avoiding death.

#### SoC fragmentation

Nowdays in a system chip there are many modules: CPU, GPU, MODEM, NPU, CAMERA and others
Every module has its ISA.

Now we ave three important comptitors:
- ARM (closed ISA)
- INTEL (closed ISA)
- RISC-V (open ISA, no fees for adopting it)

#### Linux is not like RISC-V

Develop a linux kernel costs 0

Develop a RISC-V cpu costs when you want to do the chip (build the actual material chip, milion dollars)

So why RISC-V

- It defines an open, free and standard ISA
- It allows to homogenize SoC designs
- It defines a new business model
- It is not dependent on mark fluctuations 

*Krste Asanovic talk*

### How different is RISC-V compared to other ISAs?

With RISC-V you have a basic version with the basic instructions, and then you can take the extensions. 
We are going to see the V extension.

Smaller documentation (because is young), faster and less code then ARM and Intel.

*See Compiler explorer that allows to write code in various programming language*

## Data Parallelism

**WE ARE ASSUMING SINGLE-CORE**

Starting from the Flynn's taxonomy: SISD, SIMD, MIMD, MISD

Today we are going to work with SIMD istructions, i.e. apply a single istruction to multiple data. I take a multiple variables and I apply **add** to all of them.

Three variations of SIMD:
- Vector architectures
- SIMD extensions: Arm Neon, VSX, TSUBASA
- GPUs

Example: x86 (Intel) SIMD

*axpy function avx512 (x86 intrinsics) code example*

Using this technique has some problems:
- Lose portability (portability its very important, often is more important than efficiency)
- frequency of the CPU have to decrease to avoid hotspot (melting risk)
- Cache pollution risk (contigue memory blocks benefits on that, but it usually isn't so)
- Smart enough compiler is able to vectorize sequential code, so sometimes you don't have to do that mainting portability (autovectorization).

There is a way to avoid sequential tails (when you don't have power of two sizes). Using gvl istruction, you can take the maximum vector lenght available.

In RISC-V we have the support of mask (using padding to reach power of 2 and then trash not needed data) and vector length (gvl).

#### Vector memory access and Vector chaining

**Indexed access and strided access:** 

- Indexed gather and scather data building vector using the address of a data cell, the strided one take data from cell jumping with a fix length.
- there is a complexity difference, strided is way more efficient.

**Vector chaining:**

- Load vector -> multiply -> add: you have to wait that all the vector is finished to start the operations
- With **vector chaining** we can start doing multiply operations during the vector loading, but we need a method to access subset of the vector.
- We need a compiler that emit vector instructions (GCC do that).


## Performance Analysis Tools


We need tools to measure things.

In computer science which tools we have:
- printf :D (don't do that)

### perf: linux basic tool for performance analysis

- Performance counters
- Event monitoring
- profiling code
- others

perf can't work with parallel systems, so we need to use other tools for further analysis

### Extrae: going parallel

Also **Paraver:** graphical interface and graphs for analysis

Tools for HPC performance analysis

**scheme here**

see [https://pop-coe.eu/partners/tools]


## European Processor Initiative

Europe is looking for technological independece creating a RISC-V core. They started from ARM and then turned on RISC-V.
There are two projects, one on ARM for general porpouse cpu and RISC-V for accelerator.

He is presenting a new architecture that use a Vector Processing Unit that try to compete with GPUs.
The architecture presented can be used only with #pragma you don't have to switch between host and device, you have a better portability.

### EPAC: RISC-V accelerator

One chip with 3 chips inside: 
- VEC: Scalar CPU + Vector processing unit 
- STX: 
- VRP:

### Focus on VEC: Avispado and Vitruvius

They use FPGA to test their RTL implementation, without the necessity to build the actual silicon for performance analysis


## How to program with SIMD

- Assembly :(

- C/C++ intrinsics :(

- #pragma omp simd

- autovectorization (compiler optimization)

# Take home message:

To improve efficiency do vectorization, verify that vectorization has been done correctly using analyzers.

We need to improve vectorization on compilers and introduce that on existing multi-disciplinaries systems (like QuantumExpresso)

# Afternoon

## Supercomputers

What is a supercomputer? Today from 10 PFlops/s computer to exaflops. (also bandwidth)

What makes a computer an HPC node? 
- Multiple socket (larger sockets)
- Multiple ram modules
- motherboard build to cool directly
- BMC (allows a connection also when the node is broken)
- NVLink capability

CPU Speed: DOuble precision flops (DPFlops) = #cores * turbo_frequency * (#flop/cycle) 


Intel Xeon Platinum 8470Q = 52 cores * 3.8 GHz * 32(8*2*2)

Intel Core i3-1220P = 2 * 4.4 * 8(2*256/64* Intel AVX2) + 8 * 3.3 * ...


## How do we measure performance?

Finding theoretical limits in your HPC compute node: GFLOP/s and DRAM GB/s

*See Grace Superchip, NVIDIA*



## STREAM Benchmark

The stream benchmark basically loads an array of numbers in memory and performs some basic operations on it.

COPY: measure the trasnfer rate with no arithmetic
SALE: adds a scalar operation
SUM: sums tow operands
TRIAD: defines a multiply-add

# HPL Benchmark
