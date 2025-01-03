#linux #ubuntu #package-manager 

NOTE: Always `update` before installing/upgrading packages

| Command                  | Description                                    |
| ------------------------ | ---------------------------------------------- |
| `sudo apt update`        | Updates the package database.                  |
| `sudo apt upgrade`       | Upgrades installed packages.                   |
| `sudo apt install <pkg>` | Installs a specific package.                   |
| `sudo apt remove <pkg>`  | Removes a package (keeps configuration files). |
| `sudo apt purge <pkg>`   | Completely removes a package.                  |
| `sudo apt autoremove`    | Removes unnecessary packages.                  |
| `sudo apt clean`         | Clears the package cache.                      |
| `apt search <pkg>`       | Searches for a package.                        |
| `apt show/info <pkg>`    | Displays details about a package.              |
| `apt list --upgradable`  | Lists outdated packages.                       |

## Add/Remove PPA (Ubuntu Personal Package Archive)

```bash
# add PPA
sudo add-apt-repository ppa:<ppa-name>
# e.g. NeoVim
sudo add-apt-repository ppa:neovim-ppa/unstable

# remove PPA
sudo add-apt-repository --remove ppa:<ppa-name>
```

## Hold a Package

```bash
# Hold a package
sudo apt-mark hold <package-name>

# Check the hold status
apt-mark showhold

# Unhold
sudo apt-mark unhold <package-name>
```