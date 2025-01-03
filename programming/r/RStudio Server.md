#R #IDE

If you want to use **RStudio**. You can use [RStudio Desktop](https://posit.co/download/rstudio-desktop/). But since we are using [[HPC]], which is headless, we can only use [RStudio Server](https://posit.co/download/rstudio-server/).

Most of the time the server has [Open OnDemand](https://openondemand.org/) with **RStudio Server** Configured. Sometimes due to SSL issue, some content cannot be displayed properly. 

![](https://i.imgur.com/5iPBnbG.png)

To solve this you can use [[Singularity]] to run **RStudio Server** and use [[SSH#Port Forwarding|SSH Port Forwarding]] to forward port to local machine which allows us to access the **RStudio Server** on [[HPC]].

See my note here [no-ood.md](https://github.com/Justype/HPC-tips/blob/main/no-ood.md). And my Singularity definition file [cuda-nvim-code-rstudio.def](https://github.com/Justype/HPC-tips/blob/main/singularity-def/cuda-nvim-code-rstudio.def).

## GitHub Copilot on RStudio Server

[GitHub Copilot](https://github.com/features/copilot) is now available on RStudio Server.

- If you are a student or educator, you can join [GitHub Education](https://github.com/education) for free GitHub Pro and Copilot.
- You need to change **RStudio Server** config file to enable it.
	- In `/etc/rstudio/rsession.conf`
	- set `copilot-enabled=1`
