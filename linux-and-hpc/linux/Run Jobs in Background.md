#linux #unix #job-manager 

### TL;DR

- `nohup command &`
    - view running jobs and their pid `ps wf` (Be careful if the cluster has multiple login nodes)
    - kill job `kill pid`
- run command in [[Tmux]], then detach the session
- `sbatch your_script.sh`, see [[Slurm]]

### SIGHUP and `nohup`

- If your connection to remote machine is closed, `SIGHUP` signal will be sent to the remote machine to terminate running jobs.
- `nohup` is the middle layer, which catches the `SIGHUP` signal but does not pass it so that the process will not be terminated.

## HUP and SIGHUP

If something happened that broke the connection between your terminal and the computer, the computer detected the line drop and sent a or hang up signal to the programs you'd been running. The programs ceased execution when they received the signal `HUP`.

Today, a terminal window is an emulation of a physical terminal. If you have processes running that were launched from that terminal window and you close that window the `SIGHUP` signal is sent to the programs so that they're informed of the HUP and know they should terminate.

## Nohup

If you want to have a process continue even if the terminal window it was launched from is closed, you need a way to intercept the `SIGHUP` so that the program never receives it. (Actually, the terminal window doesn't launch processes, they're launched by the shell session inside the terminal window.) The simple and elegant solution to that problem is to place another process between the shell session and the program, and have that middle-layer program never pass on the `SIGHUP` signal.

That's what `nohup` does. It launches programs for you so that they are a child process of `nohup`, not a child process of the shell. Because they're not a child process of the shell, they won't directly receive a SIGHUP from the shell. And if `nohup` doesn't pass on the SIGHUP to its children, the program won't receive SIGHUP at all.

## Using nohup

Downloading large file

```bash
user@host:/somewhere/data$ nohup download-data.sh &
[1] **847252** # pid process ID
nohup: ignoring input and appending output to 'nohup.out'

user@host:/somewhere/data$ ls
download-data.sh nohup.out data.tar.gz

# You can also view the process using ps
user@host:/somewhere/data$ ps wf
    PID TTY      STAT   TIME COMMAND
 807293 pts/1646 Ss     0:00 -bash
 847252 pts/1646 S      0:00  \_ /usr/bin/bash download-data.sh
 847254 pts/1646 S      0:05  |   \_ wget -N --no-verbose 
 847893 pts/1646 R+     0:00  \_ ps wf

# pgrep will find all users' processes, please use specific name
user@host:/somewhere/data$ pgrep download-data.sh
847252

# pwdx check working directory
user@host:/somewhere/data$ pwdx 847252
847252: /somewhere/data
```

## Strategy to find `nohup` jobs

- Print `pid` in script by `echo $$`
- `pgrep progress-name`
- `ps wf` by COMMAND and TTY

You can print TTY in script, then you can find the process by `ps -t`

## References

- [https://www.howtogeek.com/804823/nohup-command-linux/](https://www.howtogeek.com/804823/nohup-command-linux/)
- [https://www.baeldung.com/linux/nohup-process-get-pid](https://www.baeldung.com/linux/nohup-process-get-pid)