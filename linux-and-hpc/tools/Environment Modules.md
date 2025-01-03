#linux #tools #env-manager

[Environment Modules](https://modules.sourceforge.net/) is a tool that simplify shell initialization and lets users easily modify their environment during the session with **modulefiles**.

This is very handy if you want to keep the different versions of the same package.

Go to check my GitHub repository: [Justype/modules](https://github.com/Justype/modules)

![modules logo](https://modules.sourceforge.net/modules_red.svg)

## Quick Examples

- `module avail` (list available modules)
- `module load software` (load certain modules)
- `module unload software` (unload certain modules)
- `module purge` (unload all)

### module avail

```
----------------- /workspace/lab/modules/modulefiles -----------------
cellranger/7.2.0 cellranger/8.0.1  spaceranger/3.1.1

------------------- /usr/share/Modules/modulefiles -------------------
dot  module-git  module-info  modules  null  use.own

---------------------- /usr/share/modulefiles ------------------------
mpi/mpich-x86_64  mpi/openmpi-x86_64  pmi/pmix-x86_64
```

### module load

```bash
$ module load cellranger/8.0.1 # then you can use cellranger
$ cellranger
cellranger cellranger-8.0.1

Process 10x Genomics Gene Expression, Feature Barcode, and Immune Profiling data

Usage: cellranger <COMMAND>
...
```

## Custom Module Files

Important functions

- `set variable_name value` (local variable, in this script)
- `setenv` (environmental variable, in the machine)
- `prepend-path PATH location_of_bin` (add location_of_bin to the beginning of `$PATH`)

My `template` module file.

- It is version independent, make sure you have the right file name (version).
- It is Tcl script not shell script.

```tcl
#%Module1.0
# Almost all of them are automated, you can modify whatis, ModulesHelp, and APP_HOME as needed.
# Search TOCHANGE to find the targets.
# You can also load other modules in this modulefile.
#   module load java/17.0.12

# abs path of this file, like /somewhere/modules/modulefiles/app/x.x.x
set abs_path [file normalize [info script]]
# full name of this module, like app/x.x.x
set app_full_name [module-info name]
# name of this module, like app
set app_name [file dirname $app_full_name]
# version of this app, like x.x.x
set app_version [file tail $app_full_name]
# abs path of modules, like /somewhere/modules
set module_root [file dirname [file dirname [file dirname $abs_path]]]
# abs path of the apps/app/x.x.x , like /somewhere/modules/apps/app/x.x.x
set app_root [file join $module_root apps $app_full_name]
# record current date time
set current_datetime [clock format [clock seconds] -format "%Y-%m-%d %T"]

# TOCHANGE: A brief description using module-whatis
module-whatis "Loads $app_name version $app_version"

# TOCHANGE: Detailed Help Section
proc ModulesHelp { } {
    # cannot use variables here
    puts stderr "Placeholder for the usage of this module."
}

# Display a help message when loading this module
if { [module-info mode] == "load" } {
    puts stderr "\[$current_datetime\] Loading module $app_full_name"
    # only display message if not in SLURM
    # if {![info exists ::env(SLURM_JOB_ID)]} {
    # }
} elseif { [module-info mode] == "unload" } {
    # puts stderr "\[$current_datetime\] Unloading module $app_full_name"
}

# Modify environment variables if the folders exist
if {[file isdirectory "$app_root/bin"]} {
    prepend-path PATH $app_root/bin
} elseif { [module-info mode] == "load" && ![info exists ::env(SLURM_JOB_ID)] } {
    puts stderr "WARNING: No bin directory found in $app_full_name"
}
if {[file isdirectory "$app_root/lib"]} {
    prepend-path LD_LIBRARY_PATH $app_root/lib
}
if {[file isdirectory "$app_root/share/man"]} {
    prepend-path MANPATH $app_root/share/man
}

# TOCHANGE: Set custom environment variables and aliases
# setenv APP_HOME $app_root
# setalias run_app "$app_root/bin/app"

# Handle conflicts with other versions
conflict $app_name

```

## References

- Official: [https://modules.readthedocs.io/en/stable/index.html](https://modules.readthedocs.io/en/stable/index.html)
- Tutorial: [https://adamdjellouli.com/articles/linux_notes/enviroment_modules](https://adamdjellouli.com/articles/linux_notes/enviroment_modules)