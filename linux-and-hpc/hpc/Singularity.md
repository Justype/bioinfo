#linux #hpc #container

<img alt="singularity logo" height=32 src="https://docs.sylabs.io/guides/3.0/user-guide/_static/logo.png" /> Singularity is container system like Docker but can run without `sudo` privilege.

### Why Use Singularity in HPC:

- **Compatibility**: Singularity works well in HPC environments because it doesn’t require root access to run (unlike Docker).
- **Performance**: Singularity containers can run applications with near-native performance, crucial for computational tasks in research.
- **Security**: Singularity allows users to run containers without compromising the security policies of the host system.

## Common Commands

**Run a Container**: To execute a command inside the container:

```bash
singularity exec my_container.sif <command>
```

**Shell into a Container**: To get an interactive shell inside the container:

```bash
singularity shell my_container.sif
```

## Overlay

**overlay** allows users to create **temporary**, **writable** layers on top of a container's existing filesystem.

```bash
# Create Overlay
singularity overlay create --size 1024 --sparse overlay.img

# Use Overlay with -o parameter
singularity shell -o shell.img my_container.sif
# ro means read-only
singularity exec -o overlay.img:ro my_container.sif <command>
```

## Build Your Own Image

To create a container from a definition file (e.g., `my_container.def`)

```bash
sudo singularity build my_container.sif my_container.def
```

My `def` on GitHub: [https://github.com/Justype/HPC-tips/blob/main/singularity-def/cuda-nvim-code-rstudio.def](https://github.com/Justype/HPC-tips/blob/main/singularity-def/cuda-nvim-code-rstudio.def)

# References

- [https://sylabs.io/singularity/](https://sylabs.io/singularity/)
- [https://github.com/sylabs/singularity](https://github.com/sylabs/singularity)

