#linux #ubuntu 

TL;DR:

| Headless | `sudo apt autoremove --purge snapd`<br/>`sudo apt-mark hold snapd`                      |
| -------- | --------------------------------------------------------------------------------------- |
| Desktop  | 1. Remove Ubuntu Firefox<br/>2. Install Firefox from Mozilla Team<br/>3. Remove `snapd` |

## Remove `snapd`

```bash
sudo rm -rf /var/cache/snapd/
sudo apt -y autoremove --purge snapd gnome-software-plugin-snap
rm -fr ~/snap
sudo apt-mark hold snapd
```

### Desktop

Remove Ubuntu Firefox (snap version)

```bash
sudo snap remove firefox

# If failed, run this: disable gnome extensions and hunspell service and remove firefox
sudo rm /etc/apparmor.d/usr.bin.firefox 
sudo rm /etc/apparmor.d/local/usr.bin.firefox
sudo systemctl stop var-snap-firefox-common-host\\x2dhunspell.mount
sudo systemctl disable var-snap-firefox-common-host\\x2dhunspell.mount
sudo snap remove firefox
```

Add repository and set priority

```bash
sudo add-apt-repository ppa:mozillateam/ppa

echo '
Package: *
Pin: release o=LP-PPA-mozillateam
Pin-Priority: 1001

Package: firefox
Pin: version 1:1snap*
Pin-Priority: -1
' | sudo tee /etc/apt/preferences.d/mozilla-firefox
```

Update list and install Firefox

```bash
sudo apt update
sudo apt install firefox
```

## References

- [https://askubuntu.com/questions/1035915/how-to-remove-snap-from-ubuntu](https://askubuntu.com/questions/1035915/how-to-remove-snap-from-ubuntu)
- [https://askubuntu.com/questions/1399383/how-to-install-firefox-as-a-traditional-deb-package-without-snap-in-ubuntu-22/1404401#1404401](https://askubuntu.com/questions/1399383/how-to-install-firefox-as-a-traditional-deb-package-without-snap-in-ubuntu-22/1404401#1404401)