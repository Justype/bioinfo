#linux #unix #tools 

<img height=32 src="https://rclone.org/img/logo_on_light__horizontal_color.svg" /> [**Rclone**](https://rclone.org/) is a powerful command-line tool used for managing and synchronizing files between your local filesystem and various cloud storage providers (e.g., Google Drive, Amazon S3, Dropbox, etc.).

- `ls` list files, `lsd` list directories
- `copy`, `sync`,and `delete` folders, `copyto` a single file.
- `rclone mount mydrive: /path/to/mountpoint`
    - `fusermount -u /path/to/mountpoint` (unmount cleaning up)

## Configuring

```bash
rclone config
# Then follow the prompt:
# 1. n > create new remote
# 2. give a name
# 3. select the remote type
# Follow the prompts to authenticate and authorize
```

TIPS: You can first authorize on your local machine. Then copy config file to the remote machine.

```bash
rclone config show # show the path to rclone config file
```

## Common Usage

### List Files: `ls` `lsd`

To list the contents of a remote storage (e.g., `mydrive` for Google Drive):

```bash
rclone ls mydrive: # be careful, it will list files in subfolder
rclone lsd mydrive: # list directory only in under this path
```

### Copy Folders from/to/between Remotes: `copy` `copyto`

To copy a file or directory from your local system to a remote (e.g., Google Drive):

```bash
# local to remote
rclone copy path/to/directory mydrive:path/to/directory

# remote to local
rclone copy mydrive:path/to/directory path/to/local/directory

# between remotes
rclone copy mydrive:path/to/directory mydrive2:path/to/directory
```

`copy` vs `copyto`

- `copy` will automatically create directories on the destination if they don't exist.

If you `copy` a file, you will get: `path/file/file` on the remote machine.

```bash
$ rclone copy path/to/file mydrive:path/to/file
$ rclone ls mydrive:path/to/file
 0 file/file

$ rclone copyto path/to/file mydrive:path/to/file
$ rclone ls mydrive:path/to/file
 0 file
```

### Sync Folders from/to/between Remotes: `sync`

The `sync` command will delete files in the destination that no longer exist in the source, so be cautious.

NOTE: This `sync` is one direction NOT two-way.

```bash
# local to remote
rclone sync path/to/directory mydrive:path/to/directory
# remote to local
rclone sync mydrive:path/to/directory path/to/local/directory
# between remotes
rclone sync mydrive:path/to/directory mydrive2:path/to/directory
```

### Move Files: `move`

This will copy the files and then delete them from the local filesystem.

```bash
# local to remote
rclone move path/to/directory mydrive:path/to/directory
# remote to local
rclone move mydrive:path/to/directory path/to/local/directory
# between remotes
rclone move mydrive:path/to/directory mydrive2:path/to/directory
```

### Check Differences: `check`

To compare files between a local directory and a remote (to see if they’re identical or need syncing):

```bash
# local to remote
rclone check path/to/directory mydrive:path/to/directory
# remote to local
rclone check mydrive:path/to/directory path/to/local/directory
# between remotes
rclone check mydrive:path/to/directory mydrive2:path/to/directory
```

### Mount a Remote as a Filesystem:

You can mount a remote as a filesystem to interact with it like a local filesystem. This requires FUSE support and must be run with appropriate permissions.

```bash
rclone mount mydrive: /path/to/mountpoint
```

To unmount, use:

```bash
fusermount -u /path/to/mountpoint
```

### Delete Files: `delete`

```bash
rclone delete mydrive:/path/to/remote/file
```

### Useful Flags for Rclone

- **`--dry-run`**: Simulate the operation without actually making changes. This is useful to see what will happen before performing the actual action.
```bash
rclone copy /path/to/local/file mydrive:/path/to/remote/directory --dry-run
```

- **`--progress`**: Show progress while copying or syncing files.
```bash
rclone copy /path/to/local/file mydrive:/path/to/remote/directory --progress
```

- **`--exclude`**: Exclude files that match a pattern.
```bash
rclone copy /path/to/local/ mydrive:/path/to/remote/ --exclude "*.txt"
```

# References

- [https://rclone.org/](https://rclone.org/)