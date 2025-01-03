#hpc 

HPC (High-Performance Computing) are usually a cluster, which consists of interconnected server nodes.

- Use [[SSH]] to login to Login Nodes
- You can split large tasks into different Compute Nodes using [[Slurm]].

![HPC structure](https://i.imgur.com/kE4kUmW.png)

### Key Features of HPC Clusters:

- **Parallel Processing**: Clusters allow tasks to be split into smaller pieces, processed simultaneously across many nodes, speeding up computations.
- **Distributed Memory**: Each node in a cluster has its own memory, and data is shared between nodes via a network.
- **Scalability**: Clusters can easily grow by adding more nodes to increase computational power.

### How Clusters are Used:

- **Large Simulations**: For example, weather forecasting or molecular modeling.
- **Big Data Analysis**: Handling large datasets in fields like genomics or finance.
- **Machine Learning**: Training complex AI models across many nodes.

Clusters are managed with job schedulers (like [[Slurm]]) that distribute computational tasks efficiently across the nodes, ensuring optimal resource utilization.

### Slurm components

- Controller: `slurmctld`
- Database: `slurmdbd`
- Compute Nodes: `slurmd`

![Slurm Arch](https://slurm.schedmd.com/arch.gif)

## References

- [https://cl.indiana.edu/Supercomputing_at_IU.pdf](https://cl.indiana.edu/Supercomputing_at_IU.pdf)
- [https://www.ibm.com/topics/hpc](https://www.ibm.com/topics/hpc)
- [https://slurm.schedmd.com/overview.html](https://slurm.schedmd.com/overview.html)
