# CompArch

We are entering the multi-core era in computer science. All major high-performance processor manufacturers have in- tegrated at least two cores (processors) on the same chip — and it is predicted that chips with many more cores will be- come widespread in the near future. As cores on the same chip share the DRAM memory system, multiple programs execut- ing on different cores can interfere with each others’ memory access requests, thereby adversely affecting one another’s per- formance.
 we demonstrate that current multi-core proces- sors are vulnerable to a new class of Denial of Service (DoS) attacks because the memory system is “unfairly” shared among multiple cores. An application can maliciously destroy the memory-related performance of another application running on the same chip. We call such an application a memory perfor- mance hog (MPH). With the widespread deployment of multi- core systems in commodity desktop and laptop computers, we expect MPHs to become a prevalent security issue that could affect almost all computer users.
We show that an MPH can reduce the performance of an- other application by 2.9 times in an existing dual-core system, without being significantly slowed down itself; and this prob- lem will become more severe as more cores are integrated on the same chip. Our analysis identifies the root causes of unfair- ness in the design of the memory system that make multi-core processors vulnerable to MPHs. As a solution to mitigate the performance impact of MPHs, we propose a new memory sys- tem architecture that provides fairness to different applications running on the same chip. Our evaluations show that this mem- ory system architecture is able to effectively contain the neg- ative performance impact of MPHs in not only dual-core but also 4-core and 8-core systems.


## 1 Motivation: Examples of Denial of Mem- ory Service in Existing Multi-Cores 

In this section, we present measurements from real sys- tems to demonstrate that Denial of Memory Service at- tacks are possible in existing multi-core systems.

### 1.1 Applications 

We consider two applications to motivate the problem. One is a modified version of the popular stream bench- mark , an application that streams through memory and performs operations on two one-dimensional arrays. The arrays in stream are sized such that they are much larger than the L2 cache on a core. Each array consists of 2.5M 128-byte elements.5 Stream (Figure 3(a)) has very high row-buffer locality since consecutive cache misses almost always access the same row (limited only by the size of the row-buffer). Even though we cannot directly measure the row-buffer hit rate in our real experimental system (because hardware does not directly provide this information), our simulations show that 96% of all mem- ory requests in stream result in row-hits.
The other application, called rdarray, is almost the ex- act opposite of stream in terms of its row-buffer locality. Its pseudo-code is shown in Figure 3(b). Although it per- forms the same operations on two very large arrays (each consisting of 2.5M 128-byte elements), rdarray accesses the arrays in a pseudo-random fashion. The array indices accessed in each iteration of the benchmark’s main loop are determined using a pseudo-random number genera- tor. Consequently, this benchmark has very low row- buffer locality; the likelihood that any two outstanding L2 cache misses in the memory request buffer are to the same row in a bank is low due to the pseudo-random gen- eration of array indices. Our simulations show that 97% of all requests in rdarray result in row-conflicts.




