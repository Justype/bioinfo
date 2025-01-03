#linux #permission

```
$ ls -l file.sh
-rwxr-x--x 1 ubuntu ubuntu 0 Nov 24 04:51 file.sh
│ │  │  │  │    │      │   │       │       └─ Name
│ │  │  │  │    │      │   │       └─ Last modified datetime
│ │  │  │  │    │      │   └─ File size (bytes)
│ │  │  │  │    │      └─ Group name
│ │  │  │  │    └─ Owner name
│ │  │  │  └─ Hard link count
│ │  │  └─ Permissions for others (Others can only execute it)
│ │  └─ Permissions for group     (Group and read and execute it)
│ └─ Permissions for user/owner   (User can read, write, and execute it)
└─ File type (- file, d directory, l symlink)

Permissions
File
  r - Read    – *view or copy* its contents
  w - Write   – *modify or remove* it
  x - eXecute – *run it
Directory
  r - Read    – *list and copy* files from it (needs x permission as well)
                If no x permission, only names can be detected, no status.
  w - Write   – *edit or delete* files into it  (needs x permission as well)
  x - eXecute – enter it
```

## Change Permissions

- `chown` – change ownership `owner` or `owner:group` or `:group`
- `chgrp` – change group
- `chmod` – change read, write, and execute permissions

### `chmod`

By Text

- `u` user, `g` group, `o` others, `a` all
- `r` read, `w` write, `x` execute

e.g.

```bash
chmod a+r test.sh # adds read permission for all classes
chmod o-x test.sh # removes execute permission for others
chmod u=rw,g=r,o= plan.txt # sets read and write permission for User
                           # sets read for Group
                           # denies all permissions to Others
```

By Number

```bash
4210
rwx-
0 : --- none
1 : --x
2 : -w-
3 : -wx write and execute
4 : r--
5 : r-x read and execute
6 : rw- read and write
7 : rwx all the permissions

Example
chmod 751 test.sh
# for me:    7 rwx, I can read it, edit it and execute it
# for group: 5 r-x, they can read it and execute it
# for others 1 --x, they can only execute it
```

## Special Permissions

1. **SetUID (`s`)**: `chmod u+/-s` set user id, file only
    - If applied to an **executable file**, it allows users to **run the file with the permissions of the file's owner**.
    - Represented as `rwsr-xr-x`.
    - Example: `/bin/passwd` others can use it to reset password.
2. **SetGID (`s`)**: `chmod g+/-s` set group id, both file and group
    - If applied to **directories**, new files created within **inherit the directory’s group ownership**.
    - Represented as `rwxr-sr-x`.
    - Example: group shared folder
3. **Sticky Bit (`t`)**: `chmod +/-t`
    - Applied to **directories** to allow **only the owner** to delete or modify their files.
    - Commonly used in shared directories like `/tmp`.
    - Represented as `drwxrwxrwt`.

### `setuid` Example

```bash
$ ls -l /bin/passwd
-rwsr-xr-x. 1 root root 33K Feb  7  2022 /bin/passwd*

$ passwd
Changing password for user xxxx1.
Current password:_
New password:_
Retype new password:_
passwd: all authentication tokens updated successfully.
```

### `setgid` Example

```bash
$ ls -ld group_dir
drwxr-xr-x 2 user group 4096 Nov 24 10:00 group
$ touch group_dir/test_file_user
$ ls -l group_dir     # file's group is the default group of the user
drwxr-xr-x 2 user user 4096 Nov 24 10:01 test_file_user

$ chmod g+s group_dir # setgid
$ ls -ld group_dir
drwxr-sr-x 2 user group 4096 Nov 24 10:03 group
$ touch group_dir/test_file_group # now file's group is set to group
$ ls -l group_dir     
drwxr-xr-x 2 user user  4096 Nov 24 10:01 test_file_user
drwxr-xr-x 2 user group 4096 Nov 24 10:04 test_file_group
```

## References

- [https://linuxize.com/post/how-to-list-files-in-linux-using-the-ls-command/](https://linuxize.com/post/how-to-list-files-in-linux-using-the-ls-command/)
- [https://www.redhat.com/en/blog/suid-sgid-sticky-bit](https://www.redhat.com/en/blog/suid-sgid-sticky-bit)
