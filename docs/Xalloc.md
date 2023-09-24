*Still under construction.*

This lib is used to allocate normal DRAM-based memory and CXL-based memory using Rust.

Generally, for normal DRAM-based memory, we add a wrapper above [jemalloc](https://github.com/tikv/jemallocator). For CXL-based memory, because CXL-based memory can be transfered into cpuless numa-node memory, we enable specific numa node memory allocation. 

Repo is [here](https://github.com/Tom-CaoZH/xalloc).