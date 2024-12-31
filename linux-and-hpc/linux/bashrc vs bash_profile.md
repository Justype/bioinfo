#linux #unix #bash 

Both `~/.bashrc` and `~/.bash_profile` are configuration files for **Bash**. They are used to set environment variables, aliases, functions, and customize the user shell environment.

- `.bashrc` is for **non-login shells (interactive or run a script)**
- `.bash_profile` is for **login shells (SSH login)**.

OR you just put all the settings in `.bashrc`

## `~/.bashrc`

- **Purpose**: Executed for **interactive non-login shells** (run a script or open a new terminal).
- **Common Usage**:
    - Set **aliases**, **functions**, and **custom shell behavior**.
    - Modify environment variables that are relevant to the interactive shell session.
    - Example: Setting up color support or shell prompt customization.
```bash
# ~/.bashrc
alias ll='ls -la'
export PATH=$PATH:/home/user/bin
```

## `~/.bash_profile`

- **Purpose**: Executed for **login shells** (when you log in via the terminal, SSH, or console).
- **Common Usage**:
    - Set environment variables that should apply to **all shell sessions** (interactive and non-interactive).
    - Typically, this file is used to configure **user environment settings**, such as `PATH`, `USER`, `LANG`, etc.
    - It often **sources** `~/.bashrc` to apply those configurations to login shells as well.

**Example content**:

```bash
# ~/.bash_profile
export PATH=$PATH:/usr/local/bin
export EDITOR=vim

# Source ~/.bashrc for login shells
if [ -f ~/.bashrc ]; then
    source ~/.bashrc
fi
```

### Best Practice

- **Source `~/.bashrc` in `~/.bash_profile`** to ensure your interactive shell settings apply to login shells as well:

```bash
# In ~/.bash_profile
if [ -f ~/.bashrc ]; then
    source ~/.bashrc
fi
```