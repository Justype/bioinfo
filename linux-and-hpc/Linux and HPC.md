[[Linux|🐧Linux]] and High-Performance Computing ([[HPC]]) have become essential tools to analyze vast amounts of data.

### HPC Tools

| Tools      | Description                                             |
| ---------- | ------------------------------------------------------- |
| [[SSH]]    | Connect to a remote machine (HPC)                       |
| [[Slurm]]  | Request computing resources on the cluster              |
| [[Rclone]] | Transfer files from/to your drive (Google, OneDrive, …) |
| [[Vim]]    | Text editor. (or use `nano`)                            |
### Package Manager

> [!tip] Admin Privilege
> You don’t have `sudo` privilege on HPC. You need other ways to manage packages and environments.

- [[Miniconda]] or [[Micromamba]] with [bioconda](https://bioconda.github.io/)
- [[Environment Modules]] or [[Lmod]]
- To run container/image, use [[Singularity]]

### Other Tips:

1. [[Bash Script]]
2. [[Run Jobs in Background]]
3. [[Linux Permissions]]
