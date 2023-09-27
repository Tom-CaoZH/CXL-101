To enable Kubernetes with CXL-based memory, we need to verify two things:

1. whether the CXL-based memory is visible to the container like the host. 
2. whether container can control CXL-based memory well. 


## CXL-based Memory Visibility

For kubernetes, it relys on kubelet to control the containers in a machine. then for the container in kubernetes, there is a CRI(Container Runtime Interface). It can utilize containerd which is the container in docker. So in order to verify the CXL-based memory visibility, we can verify docker's. For verifying it, we only need to see whether the CXL-based memory of the host can be seen in the docker. After we setup the simulation evirtonment and install docker in the simulated machine, we find that it's visibile which means we can see the CXL-based memory in docker just like the host. 

## CXL-based Memory Control

Kubernetes Replys on Cgroup to control the resource like cpu, memory and etc. So in order to enable Kubernetes control CXL-based memory, one method is to enable Cgroup aware of CXL-based memory. For more details, you can refer to the information in the referrnece. 



Referrence:

[Container Memory Interface](https://www.youtube.com/watch?v=ZArmCVN4uF0)

[Design of per cgroup memory disaggregation](https://asplos.dev/wordpress/2023/07/03/design-of-per-cgroup-memory-disaggregation/) from [Yiwei Yang](https://asplos.dev/)
