## Abstract

We present an efficient implementation of 7-point and 27-point stencils on high-end Nvi- dia GPUs. A new method of reading the data from the global memory to the shared memory of thread blocks is developed. The method avoids conditional statements and requires only two coalesced instructions to load the tile data with the halo (ghost zone). Additional opti- mizations include storing only one XY tile of data at a time in the shared memory to lower shared memory requirements, common subexpression elimination to reduce the number of instructions, and software prefetching to overlap arithmetic and memory instructions, and enhance latency hiding. The efficiency of our implementation is analyzed using a simple stencil memory footprint model that takes into account the actual halo overhead due to the minimum memory transaction size on the GPUs. Through experiments we demonstrate that in our implementation the memory overhead due to the halos is largely eliminated by good reuse of the halo data in the memory caches, and that our method of reading the data is close to optimal in terms of memory bandwidth usage. Detailed performance analysis for single precision stencil computations, and performance results for single and double pre- cision arithmetic on two Tesla cards are presented. Our stencil implementations are more efficient than any other implementation described in the literature to date. On Tesla C2050 with single and double precision arithmetic our 7-point stencil achieves an average throughput of 12.3 and 6.5 Gpts/s, respectively (98 GFLOP/s and 52 GFLOP/s, respectively). The symmetric 27-point stencil sustains a throughput of 10.9 and 5.8 Gpts/s, respectively

## Basics

pdf - [[efficient_3d_stencil.pdf]]
published - 2013
## Notes

### General concepts

Paper tries to speed up [[Stencil]]s. There are three types of them tested - general 27-stencil, symmetric 27-stencil () and 7-stencil.

[[Halo (or ghost) zones]] are defined and with respect to it the **optimal memory foot print** (one read and one write operation per point).

### Algorithm

There several main principles that are being tested. Authors used previously known concept of [[2.5D Blocking]], which serves as the base stone for basically all of the principles discussed.

#### Loading XY tile

They defined a block size of particular size so that it is possible to perform only **2 coalesced reads from global memory to shared memory**.

![[efficient_block_XY_tile_in_memory.png]]

This approach has a total memory footprint of 12 bytes per point (BpP).

#### Intermediate outputs 

In contrast to traditional algorithms where there resides 3 XY planes in the shared memory (k-1, k, k + 1 where the k is current Z dimension) authors decided to store only one tile at the time in the memory. They used two extra variables to store intermediate results. Only after the stencil kernel is completed the result is written to the output memory (in each iteration the k-1 is completed).

![[z-loop.png]]

#### Software prefetching

Authors used an **explicit prefetching** of the data into registers (line 30 in previous picture) and then - **before** using these values to compute - they issued the loading operation of the next tile from the global to shared memory (line 31). This ensures math operation and data fetch interleaving explicitly thus helping the compiler. 

### Results

Authors 

## Concepts
