# CXL-101
Contain the materials about CXL. 


1. [**CXL Introduction**](docs/CXL_Introduction.md): why cxl and cxl1.1, 2.0, 3.0 sepecification. 
2. [**CXL Simulation and Setup**](docs/CXL_CXL_Sim_Setup.md): introduce how to simulate CXL and also give a sepecific way to set the environment. 
3. [**CXL Benchmark**](docs/CXL_Benchmark.md): Run serveral benchmark under cxl-based memory, hybrid DRAM-CXL-based memory. 
4. [**Build a Easy-to-Use CXL-memory Lib in Rust**](docs/Xalloc.md): This is a wrap of mmap and the similar APIs to memkind lib. 
5. [**Enable k8s with CXL-based Memory**](docs/k8s_with_cxl.md): point out it's visible to use cxl-based memory like host in the container, so the challenge lays on cxl-based memory specific control, this need to change the cgroup and related resource management utils. 

