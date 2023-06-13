# Discussions

## 06-14-23 ##

### TODO ##

- [ ] NR log SPSC on different NUMA node with varying batch size.
- [ ] Request batching in client/server. Result for varying batch sizes.
- [ ] Bloomfilter size for 20M keys to track hot set.

### Notes ###

* Representing hot set (i) Skew (ii) Stability of hot set.
* Bloomfilters can be used to implement client assisted routing.
Bloomfilters will be used to track hot set and send that information to client.
* Alternative approach might be installing eSwitch to implement client assisted routing.
* What is the relative cost to cross core communication vs hashtable access?
* Win for shared memory if cost of cross core communication isn't high.
* Comparison between lock free hashtable and EREW.

## 06-05-23 ##

* Combination of EREW and CREW can be used to reduce gap between them.
* Why write only workload is scaling more that read only workload in case of EREW?
* When bucket becomes a problem? Find out the optimum number of buckets.
* Implement lock free hash table to reduce gap between EREW and CREW.
* Implement OCC for reads
* Implement request batching and test end to end. Start with end-to-end noop table.
* Graph for all the ycsb workloads.

## 05-22-23 ##

- Ankit Q: Trees might make more sense since data deps amp differences in memory latency
- TODOs for CI/experiment runner
 - Rework CI to put runner in /proj so new runners work even after more than 1 hour
 - Script to run client/server and collect results
- Need to decide how toml files are structured
- Ashfaq: trying to run it, but not getting responses back from server
 - Ankit: probably because not using the client hw mac address
- TODOs on running experiments
 - Swap TOML args on client/server so that can run/configure them from commandline
 - Create run.py that can run client/server (across SSH where needed)

## 05-17-23 ##

### Baselines ###

#### Client assisted request routing (EREW) ####

* Keys will be partitioned statically and optimally.
* Clients will be responsible for sending request to the correct server/core.
* Cores only access local memory.
* Needs reconfiguration of partitions and migration of data if skew changes.
* Clients need to be informed on reconfiguration.
* Approximate processing time per request: 200 ns for hash table access on local memory + 1400 cycles for DPDK packet processing.

#### Server assisted request routing (shared only) ####

* Full Hashtable lives in shared memory.
* Client oblivious routing. Clients can send requests to any server.
* Assumes no key partitioning.
* Skew resistance. No reconfiguration and data migration.
* Approximate processing time per request: 400ns for hash table access on remote memory + 1400 cycles for DPDP packet processing.

#### Discussion ####

* Client assisted routing is only marginally better than server assisted shared only model.
* In both cases assumes no memory latency hiding techniques. (e.g: Pipelined memory access)

## 05-15-23 ##

- dpdk noop is running, both client and server
  - 8 mpps per core
  - ycsb already in client but hasn't been verified
  - todo verify ycsb distribution, etc
- todo need to automate running of client/server
  - start at `scripts/ci.bash` and fill in the 'run benchmarks' part
- [github self-hosted runner docs](https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners/about-self-hosted-runners)
- using cloudlab c6420s, have a "large" number of cores
- primary/secondary processes for dpdk
  - need multiple dpdk apps on same machine
  - https://doc.dpdk.org/guides/prog_guide/multi_proc_support.html
- `trait table`: can we get rid of need for per table instance handlers?
- multi threaded client/server

## 01-09-23 ##

### Benchmarks ###

* All ycsb benchmarks including the latest one.

### Design ###

* Hash table design can be in two parts: Shared hash map and thead local hash map
* Or One big hash table interface which will include both shared hashmap and local hashmap
* Experiment  with different memory placement policies (CREW, EREW, CRCW).
Details of placement policies can be found in MICA paper.
* One queue pair per NUMA node for shared memory.
* Allocation: NIC receives the packets, DMA to local memory and then copied to shared memory.
* New writes will always be in local memory.
* After first write when the key will be read it will be moved to shared memory.
* Look up will be in parallel in both shared and local memory.

### Questions ###
* What will be the design of shared memory allocator?
* How core X will deallocate core Y’s memory?

