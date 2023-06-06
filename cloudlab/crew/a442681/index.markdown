---
layout: scalebench
title: "HashBench"
---

[Buckets](#throughput-for-crew-reads-with-varying-number-of-buckets)

# Throughput

## Read

<p float="left">
<img src="./hashbrown_read_plot.png" alt="Read" width="100%"/>
</p>

## YCSB-B

<p float="left">
<img src="./hashbrown_ycsbb_plot.png" alt="Write" width="100%"/>
</p>

## YCSB-A

<p float="left">
<img src="./hashbrown_ycsba_plot.png" alt="Write" width="100%"/>
</p>

## Write

<p float="left">
<img src="./hashbrown_write_plot.png" alt="Write" width="100%"/>
</p>

# Throughput for CREW Reads with varying number of buckets

<ul>
<li>Sequential Layout: Data is distributed across buckets sequentially (Ex: 1-10, 10-20, and so on.).</li>
<li>Interleaved Layout: Data is distributed across buckets in a round-robin fashion (Ex: 1, 2, 3,.., and so on).</li>
</ul>

## Sequential Layout

<p float="left">
<img src="./hashbrown_read_buckets_sequential.png" alt="Write" width="100%"/>
</p>

## Interleaved Layout

<p float="left">
<img src="./hashbrown_read_buckets_interleaved.png" alt="Write" width="100%"/>
</p>