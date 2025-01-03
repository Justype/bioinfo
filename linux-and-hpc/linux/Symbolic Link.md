#linux #unix

Symbolic link is like a shortcut, easy to access and saving space.

```bash
~ $ ln -s /workspace/lab/your_lab lab
~ $ ls -l lab
lrwxrwxrwx. 1 user group  22 Nov 21 00:00 lab -> /workspace/lab/your_lab
```

# Hard Link vs Symbolic Link (GPT)

### Comparison Table

| Feature              | Hard Link                           | Symbolic Link                                 |
| -------------------- | ----------------------------------- | --------------------------------------------- |
| Points to            | Inode (index node, actual file)     | File/directory path                           |
| Target moves/deletes | Hard link still valid               | Symbolic link breaks                          |
| Cross-filesystem     | Not supported                       | Supported                                     |
| For directories      | Rarely allowed (special cases only) | Fully supported                               |
| Storage use          | Minimal (entry in the filesystem)   | Minimal (stores link path)                    |
| File updates         | Visible across all hard links       | Only if the path points to the updated target |
| Practical use        | Duplicate main files (same content) | Shortcut to files/directories                 |

### Hard Link

- **Definition**: A hard link is a direct reference to the **same index node** as the original file. Multiple hard links to the same file share the same data and metadata (permissions, timestamps, etc.).
- **Key Characteristics**:
    - **File Content**: All hard links to a file point to the same data on the disk.
    - **File Deletion**: The file content is only deleted when all hard links to it are removed.
    - **Cross-Partition**: Hard links **cannot** span across different filesystems or partitions.
    - **Directories**: Typically, hard links **cannot** be created for directories (to avoid filesystem loops), except in special cases like with some low-level tools.
    - **Updating the File**: Changes to the content of the file are reflected in all hard links.
    - **Storage**: Creating a hard link doesn’t consume additional storage for the file's content, only a small amount for the directory entry.

### Symbolic Link

- **Definition**: A symbolic link is a special file that points to the **path** of another file or directory. It works as a shortcut.
- **Key Characteristics**:
    - **File Content**: A symbolic link points to a pathname, not the actual file content.
    - **File Deletion**: If the target file is deleted, the symbolic link becomes a **broken link**.
    - **Cross-Partition**: Symbolic link can span across different filesystems or partitions.
    - **Directories**: Symbolic link can be created for directories without restrictions.
    - **Updating the File**: If the target file is replaced or moved, the symbolic link must be updated; otherwise, it breaks.
    - **Storage**: Symbolic link require minimal storage for the link path itself.

### Use Cases

- **Hard link**:
    - Efficiently create multiple references to large files without duplicating content.
    - Used in backup systems to save space by pointing multiple snapshots to the same unchanged data.
- **Symbolic link**:
    - Create shortcuts to files or directories for easier navigation.
    - Link files or directories across filesystems or partitions.
    - Common in configuration management (e.g., `/etc/nginx/sites-enabled` linking to `/etc/nginx/sites-available`).