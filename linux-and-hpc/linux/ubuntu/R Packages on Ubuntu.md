#linux #ubuntu #R 


> [!bug] Failed to build the R package
> - **Cause**: Ubuntu packages not installed
> - **Solution**: `sudo apt install`

How to know which package you want to install?

2 conditions:

1. [[R]] will tell you which packages you need to install, like: `apt install <package name>`
2. Find it by yourself
    1. Find which step you got the error
    2. Search for the missing library
    3. Find the name and the version of the package
    4. Install it

## Example 1 which specific packages

```
Error: Error installing package 'curl':
================================

* installing *source* package ‘curl’ ...
** package ‘curl’ successfully unpacked and MD5 sums checked
** using staged installation
Found pkg-config cflags and libs!
Using PKG_CFLAGS=-I/usr/include/x86_64-linux-gnu
Using PKG_LIBS=-lcurl
--------------------------- [ANTICONF] --------------------------------
Configuration failed because **libcurl was not found**. Try installing:
 * deb: libcurl4-openssl-dev (Debian, Ubuntu, etc)
 * rpm: libcurl-devel (Fedora, CentOS, RHEL)
If libcurl is already installed, check that 'pkg-config' is in your
PATH and PKG_CONFIG_PATH contains a libcurl.pc file. If pkg-config
is unavailable you can set INCLUDE_DIR and LIB_DIR manually via:
R CMD INSTALL --configure-vars='INCLUDE_DIR=... LIB_DIR=...'
-------------------------- [ERROR MESSAGE] ---------------------------
In file included from /workspace/lab/hanlab/software/bin/conda/envs/r44/x86_64-conda-linux-gnu/sysroot/usr/include/features.h:375,
                 from /usr/include/x86_64-linux-gnu/sys/types.h:25,
                 from /usr/include/x86_64-linux-gnu/curl/system.h:430,
                 from /usr/include/x86_64-linux-gnu/curl/curl.h:35,
                 from <stdin>:1:
/usr/include/x86_64-linux-gnu/sys/cdefs.h:146:55: error: missing binary operator before token "("
  146 | #if __USE_FORTIFY_LEVEL == 3 && (__glibc_clang_prereq (9, 0)                  
      |                                                       ^
/usr/include/x86_64-linux-gnu/sys/cdefs.h:634:49: error: missing binary operator before token "("
  634 | #if __GNUC_PREREQ (4,8) || __glibc_clang_prereq (3,5)
      |                                                 ^
In file included from /workspace/lab/hanlab/software/bin/conda/envs/r44/x86_64-conda-linux-gnu/sysroot/usr/include/stdio.h:937,
                 from /usr/include/x86_64-linux-gnu/curl/curl.h:46:
/usr/include/x86_64-linux-gnu/bits/stdio2.h:233:17: error: missing binary operator before token "("
  233 | #if __GLIBC_USE (DEPRECATED_GETS)
      |                 ^
--------------------------------------------------------------------
ERROR: configuration failed for package ‘curl’
* removing ‘/workspace/lab/hanlab/chengz63/scPipeline/renv/library/linux-ubuntu-jammy/R-4.4/x86_64-conda-linux-gnu/.renv/1/curl’
install of package 'curl' failed [error code 1]
```

Simply install `libcurl`: `apt install -y libcurl4-openssl-dev`

## Example 2 no specific packages

- OS Version: Ubuntu 24.04
- Package to install: reshape2

### Error

When you get an error, 2 ways:

1. Search it in AI tools (ChatGPT), they will tell you which part goes run.
2. Find the missing packages by yourself.

```
Error: Error installing package 'reshape2':
====================================

* installing *source* package ‘reshape2’ ...
** package ‘reshape2’ successfully unpacked and MD5 sums checked
** using staged installation
** libs
using C++ compiler: ‘g++ (Ubuntu 13.2.0-23ubuntu4) 13.2.0’
g++ -std=gnu++17 -I"/usr/share/R/include" -DNDEBUG  -I'/home/ubuntu/lab/seurat5/renv/library/R-4.3/aarch64-unknown-linux-gnu/.renv/1/Rcpp/include'     -fPIC  -g -O2 -fno-omit-frame-pointer -mno-omit-leaf-frame-pointer -ffile-prefix-map=/build/r-base-Io6aw0/r-base-4.3.3=. -fstack-protector-strong -fstack-clash-protection -Wformat -Werror=format-security -mbranch-protection=standard -fdebug-prefix-map=/build/r-base-Io6aw0/r-base-4.3.3=/usr/src/r-base-4.3.3-2build2 -Wdate-time -D_FORTIFY_SOURCE=3  -c RcppExports.cpp -o RcppExports.o
g++ -std=gnu++17 -I"/usr/share/R/include" -DNDEBUG  -I'/home/ubuntu/lab/seurat5/renv/library/R-4.3/aarch64-unknown-linux-gnu/.renv/1/Rcpp/include'     -fPIC  -g -O2 -fno-omit-frame-pointer -mno-omit-leaf-frame-pointer -ffile-prefix-map=/build/r-base-Io6aw0/r-base-4.3.3=. -fstack-protector-strong -fstack-clash-protection -Wformat -Werror=format-security -mbranch-protection=standard -fdebug-prefix-map=/build/r-base-Io6aw0/r-base-4.3.3=/usr/src/r-base-4.3.3-2build2 -Wdate-time -D_FORTIFY_SOURCE=3  -c melt.cpp -o melt.o
g++ -std=gnu++17 -shared -L/usr/lib/R/lib -Wl,-Bsymbolic-functions -flto=auto -ffat-lto-objects -Wl,-z,relro -o reshape2.so RcppExports.o melt.o -L/usr/lib/R/lib -lR
installing to /home/ubuntu/lab/seurat5/renv/library/R-4.3/aarch64-unknown-linux-gnu/.renv/1/00LOCK-reshape2/00new/reshape2/libs
** R
** data
*** moving datasets to lazyload DB
** inst
** byte-compile and prepare package for lazy loading
Error in dyn.load(file, DLLpath = DLLpath, ...) :
  unable to load shared object '/home/ubuntu/.cache/R/renv/cache/v5/R-4.3/aarch64-unknown-linux-gnu/stringi/1.8.4/39e1144fd75428983dc3f63aa53dfa91/stringi/libs/stringi.so':  **libicui18n.so.70: cannot open shared object file: No such file or directory**
Calls: <Anonymous> ... namespaceImport -> loadNamespace -> library.dynam -> dyn.load
Execution halted
ERROR: lazy loading failed for package ‘reshape2’
* removing ‘/home/ubuntu/lab/seurat5/renv/library/R-4.3/aarch64-unknown-linux-gnu/.renv/1/reshape2’
install of package 'reshape2' failed [error code 1]
```

- Look for which so file is missing. (No such file or directory), `libicui18n.so.70` in this case
    - `libicui18n.so.70: cannot open shared object file: No such file or directory`
- Search `libicui18n.so.70` ubuntu.
- So install the `libicu` (version 70)

## Get the right one

- In Ubuntu 24, the version of `libicu` is 74 not 70.
    - Result of apt search `libicu`: `libicu-dev`, `libicu74`
- But we need `libicu70`

1. Visit [https://launchpad.net/ubuntu](https://launchpad.net/ubuntu)
2. Search libicu70
3. Click `icu`, you will go to [https://launchpad.net/ubuntu/+source/icu](https://launchpad.net/ubuntu/+source/icu)
4. Click `libicu70`
	![launchpad-1](https://i.imgur.com/6ZydVU4.png)
5. In build, find the architecture fits your computer (most of the time: amd64 but My machine is arm64)
	![launchpad-2](https://i.imgur.com/xaJP0IB.png)
6. Down load the deb package in binaries or built files. Copy the link and `wget`.
    ![launchpad-3](https://i.imgur.com/aDGrqQT.png)
7. Install it with `dpkg`: `sudo dpkg -i <>.deb`