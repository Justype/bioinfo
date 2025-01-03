#linux #unix #tools #package-manager

<img alt="anaconda logo" height=32 src="https://docs.anaconda.com/_static/Anaconda_Icon.png" /> [Miniconda](https://docs.anaconda.com/miniconda/install/#quick-command-line-install) includes minimum number of packages, including `conda` and `Python`.

- Because of [bioconda channel](https://bioconda.github.io/), you can easily manage bioinfo packages.
- Saving disk space using links (hard links in Linux).

Activate/Run:

- `conda activate env_name` (using conda env, base env by default)
- `conda deactivate` (stop using current conda env)
- `conda run -n env_name command` (run certain command in specific environment)

Packages:

- `conda install package[=version] [-c channel]` (channel is optional)
- `conda update [-a/package]` (update certain packages or all `-a`)
- `conda remove package` (remote certain packages)

Environments:

- `conda create -n env_name package[=version]` (create environment with name)
- `conda env list` (List available environments)
- `conda env remove -n env_name` (remove environment)

Cleaning Cache and Unused packages: `conda clean -a`

## Install Conda

```bash
wget <https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh>
bash Miniconda3-latest-Linux-x86_64.sh

# Install without asking configs
# bash Miniconda3-latest-Linux-x86_64.sh -b -p /where_to_install
	# -b agree without ask
	# -p prefix

# Init the shell if you missed it
/where_to_install/bin/conda init
```

## Bioconda

- [https://bioconda.github.io/](https://bioconda.github.io/)

Add bioconda and conda-forge channels:

```bash
conda config --add channels bioconda
conda config --add channels conda-forge
conda config --set channel_priority strict
# conda config --set auto_activate_base false
```

## Typical `.condarc`

Conda Runtime Config file. After you add channels, it will be like this.

Personally, I prefer to set `auto_activate_base` to `false` to save bash initiation time.

```yaml
channels:
  - conda-forge
  - bioconda
  - default
channel_priority: strict
auto_activate_base: false
```

## Usage

### Create new environment

```bash
conda create -n env_name package=version
# e.g.
conda create -n seq sra-tools bowtie bowtie2 star samtools
```

### Activate environment

```bash
conda activate env_name
# e.g.
conda activate seq  # then you can run commands in seq env
fasterq SRRXXXXXXXX
```

### Install package in certain environment

```bash
conda install package[=version] [-n env_name] [-c channel]

# e.g.
conda install kallisto bustools kb-python # do this when seq env is activated
conda install sra-tools=3     # it will install version 3.x.x
conda install salmon -n seq   # install salmon in seq env
# Install pytorch using conda
conda install pytorch torchvision torchaudio pytorch-cuda=12.4 -c pytorch -c nvidia
```

### Run command in certain environment

```bash
conda run -n env_name command

# e.g.
conda run -n base nvim
conda run -n python2.7 python python2_script.py
```