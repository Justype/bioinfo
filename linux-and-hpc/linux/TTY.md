#linux #unix 

**TTY** stands for **Teletypewriter**, was a machine sending and receiving machine in 1800s and 1900s. Now it stands for a terminal device/interface, allowing messages to be typed, encoded, sent, received, decoded, and printed.

![Teletype teleprinters in use in England during World War II](https://upload.wikimedia.org/wikipedia/commons/8/89/WACsOperateTeletype.jpg)
Teletype teleprinters in use in England during World War II

# TTY on Linux

TTY on [[Linux]] is simulated and can be found under `/dev/pts`

The `tty` command will print the name of the device file that your pseudo teletypewriter is using to interface to the machine.

```bash
$ tty
/dev/pts/365

$ tty -s # -s silent

$ tty && echo "In a tty"
/dev/pts/365
In a tty

$ tty -s && echo "In a tty" # only print the message
In a tty
```

The `who` command will list information for all logged in users, including yourself.

```bash
$ who
xxxx     pts/365      2024-10-14 05:43 (10.xxx.xxx.14)
xxxx67   pts/415      2024-11-05 14:48 (10.xxx.xxx.254)
xxxx89   pts/734      2024-11-23 20:17 (10.xxx.xxx.13)
xxxx10   pts/856      2024-11-23 22:27 (10.xxx.xxx.237)
```

## Images

![telegraph alphabet](https://upload.wikimedia.org/wikipedia/commons/thumb/1/12/International_Telegraph_Alphabet_2_brightened.jpg/820px-International_Telegraph_Alphabet_2_brightened.jpg)

# References

---

- [https://www.howtogeek.com/428174/what-is-a-tty-on-linux-and-how-to-use-the-tty-command/](https://www.howtogeek.com/428174/what-is-a-tty-on-linux-and-how-to-use-the-tty-command/)
- DEC PDP-1: [https://www.youtube.com/watch?v=1EWQYAfuMYw](https://www.youtube.com/watch?v=1EWQYAfuMYw)