#hpc #job-manager

[Slurm](https://slurm.schedmd.com/overview.html) is used to request computing resources on cluster.

Common Commands:

- `sbatch script.sh`
- `srun --time=4:00:00 --cpus-per-task=4 --mem=16GB command`
- `squeue -u $USER` check your jobs
- `scancel <job_id>` cancel your job
- `seff <job_id>` efficiency of that job (CPU and memory usage)

## SBATCH vs SRUN

- `sbatch` is for **background** jobs. It will **not** be killed if you **close** the terminal.
- `srun` is for **foreground** jobs. It will be **killed** if terminal is **closed**.

# SBATCH Template

- Options after double hashtag will not be applied.
- Delete singularity, micromamba, or module if you don’t use them.

```bash
#!/usr/bin/bash
#SBATCH --cpus-per-task=4
#SBATCH --time=0-6:00:00
#SBATCH --mem=8GB
##SBATCH --gres=gpu:1
#SBATCH --job-name=bash
#SBATCH --output=slurm_%j.out
##SBATCH --mail-type=END

################################################################
# Multi-core Template for SLURM but can used for normal bash script
#
# Special variables:
#   $cpus: number of CPUs (set by #SBATCH --cpus-per-task)
#   $mem: memory in GB    (set by #SBATCH --mem)
#   $job_name: job name   (set by #SBATCH --job-name)
# 
# NOTE:
#  - Settings start with ##SBATCH will not be applied to SLURM
#  - If not in SLURM, it will extract the SLURM settings in this script
#  - If in TMUX, it will rename the window when running and done
#  - Start your code at line 64 (region YOUR CODE)
#
################################################################

#region PARSING SLURM SETTINGS and FUNCTIONS
START_TIME=`date +%s`
if [ -n "$SLURM_JOB_ID" ]; then
    ncpu=$SLURM_CPUS_PER_TASK
    mem=$(($SLURM_MEM_PER_NODE / 1024)) # convert to GB
    prompt_message="SLURM Job ID: $SLURM_JOB_ID"
else
    # Extract the SLURM SETTINGS in this script if not running in SLURM
    script_file="${BASH_SOURCE[0]:-$0}"
    ncpu=$(sed -n 's/^#SBATCH --cpus-per-task=//p' "$script_file")
    mem=$(sed -n 's/^#SBATCH --mem=//p' "$script_file")
    mem=${mem%*GB} # remove GB
    prompt_message="PID: $$"
fi

job_name=$(sed -n 's/^#SBATCH --job-name=//p' "$script_file")

## function to rename tmux window
rename_tmux_window() {
    if [ -n "$TMUX" ]; then
        tmux rename-window "$1"
    fi
}

## function to check NVIDIA GPU and set nv
if [[ -n `ls /dev | grep nvidia` ]]; then
    echo "has an NVIDIA GPU"
    nv="--nv"
fi
#endregion

echo [`date +"%Y-%m-%d %T"`] START $prompt_message, NCPU: $ncpu MEM: ${mem}GB
rename_tmux_window "$job_name `date +'%Y-%m-%d %H:%M'`"

# Decrease mem before applying it to program. Most of time, you will not use $mem
mem=$((mem - 5))

#region YOUR CODE
## micromamba
# eval "$(micromamba shell hook --shell bash)"
## conda
eval "$(conda shell.bash hook)"

## module
module purge

## singularity container
singularity exec $nv \
    $SIF bash -c '
        #source /ext3/env.sh
        #conda activate base
        exit
    '
#endregion

## Print running time
END_TIME=`date +%s`
RUN_TIME=$((END_TIME - START_TIME))
RUN_TIME_DAYS=$(($RUN_TIME / 86400))
RUN_TIME_HOURS=$(($RUN_TIME / 3600 % 24))
RUN_TIME_MINUTES=$(($RUN_TIME / 60 % 60))
RUN_TIME_SECONDS=$(($RUN_TIME % 60))

echo [`date +"%Y-%m-%d %T"`] END $prompt_message, Elapsed: $RUN_TIME_DAYS days, $RUN_TIME_HOURS hours, $RUN_TIME_MINUTES minutes, $RUN_TIME_SECONDS seconds
rename_tmux_window "DONE $job_name Elapsed $RUN_TIME_DAYS-$RUN_TIME_HOURS:$RUN_TIME_MINUTES:$RUN_TIME_SECONDS"

```

# Build Pipeline with SBATCH Dependency:

`sbatch-pipeline.sh`

```bash
#!/bin/bash

ID=$(sbatch --parsable $1) # --parsable to get SLURM_JOB_ID
shift 
for script in "$@"; do
  # +5 means it will start five minutes after the previous job
  ID=$(sbatch --parsable --dependency=after:${ID}:+5 $script)
done
```

Usage: `sbatch-pipeline.sh script1.slurm script2.slurm script3.slurm`

# References

- [https://slurm.schedmd.com/overview.html](https://slurm.schedmd.com/overview.html)
- [https://slurm.schedmd.com/sbatch.html](https://slurm.schedmd.com/sbatch.html)
- [https://docs.hpc.shef.ac.uk/en/latest/referenceinfo/scheduler/SLURM/SLURM-environment-variables.html](https://docs.hpc.shef.ac.uk/en/latest/referenceinfo/scheduler/SLURM/SLURM-environment-variables.html)