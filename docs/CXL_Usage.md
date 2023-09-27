
![CXL-usage](https://github.com/Tom-CaoZH/notes-pictures/blob/main/img/CXL-usage.png)

After conducting a thorough investigation, we have determined that the CXL software ecosystem is compatible with established PMem concepts and libraries. As depicted in the figure above, we can observe that its usage closely resembles that of PMem. In general, there are three ways to utilize CXL-based memory, with two options for volatile memory and one for non-volatile memory:

1. For volitile memory: 
   1. the character device(like /dax/dax0.0)
   2. the character device can be transferred into headless NUMA node, in this way, it can be used as NUMA node memory.
2. For non-volitile memory: 
   1. it can be used by PMem-enabled file system. 

There are also several libraries which can directly make use of CXL-based memory like [memkind](https://pmem.io/memkind/) and etc.

Refferrence:

[EXPLORING THE SOFTWARE ECOSYSTEM FOR COMPUTE EXPRESS LINK (CXL) MEMORY](https://pmem.io/blog/2023/05/exploring-the-software-ecosystem-for-compute-express-link-cxl-memory/)