#linux #unix #tools #package-manager 

<img alt="mamba logo" height=32 src="https://mamba.readthedocs.io/en/latest/_static/logo.png" /> **Micromamba** is a fully statically-linked, self-contained executable, compared to [[Miniconda]]. This means that the `base` environment is completely empty.

- [https://mamba.readthedocs.io/en/latest/installation/micromamba-installation.html](https://mamba.readthedocs.io/en/latest/installation/micromamba-installation.html)
- You can install conda along with micromamba, just install conda in base environment.

Activate/Run:

- `micromamba activate env_name` (using conda env, base env by default)
- `micromamba deactivate` (stop using current conda env)
- `micromamba run -n env_name command` (run certain command in specific environment)

Packages:

- `micromamba install package[=version] [-c channel]` (version and channel is optional)
- `micromamba update [-a/package]` (update certain packages or all `-a`)
- `micromamba remove package` (remote certain packages)

Environments:

- `micromamba create -n env_name package[=version]` (create environment with name)
- `micromamba env list` (List available environments)
- `micromamba env remove -n env_name` (remove environment)

Cleaning Cache and Unused packages: `micromamba clean -a`

## Install Micromamba

```bash
"${SHELL}" <(curl -L micro.mamba.pm/install.sh)

# micromamba shell init --shell bash --root-prefix /where/you/want/to/install
```

## Bioconda

- [https://bioconda.github.io/](https://bioconda.github.io/)

Add bioconda and conda-forge channels:

```bash
micromamba config prepend channels bioconda
micromamba config prepend channels conda-forge
micromamba config set channel_priority strict
# micromamba config set auto_activate_base false
```

## Typical `.condarc`

Conda Runtime Config file. After you add channels, it will be like this.

Personally, I prefer to set `auto_activate_base` to `false` to save bash initiation time.

```yaml
channels:
  - conda-forge
  - bioconda
channel_priority: strict
auto_activate_base: false
```

## Usage

### Create new environment

```bash
micromamba create -n env_name package=version
# e.g.
micromamba create -n seq sra-tools bowtie bowtie2 star samtools
```

### Activate environment

```bash
micromamba activate env_name
# e.g.
micromamba activate seq # then you can run commands in seq env
fasterq SRRXXXXXXXX
```

### Install package in certain environment

```bash
micromamba install package[=version] [-n env_name] [-c channel]

# e.g.
micromamba install kallisto bustools kb-python # do this when seq env is activated
micromamba install sra-tools=3     # it will install version 3.x.x
micromamba install salmon -n seq   # install salmon in seq env
# Install pytorch using conda
micromamba install pytorch torchvision torchaudio pytorch-cuda=12.4 -c pytorch -c nvidia
```

### Run command in certain environment

```bash
micromamba run -n env_name command

# e.g.
micromamba run -n base nvim
micromamba run -n python2.7 python python2_script.py
```