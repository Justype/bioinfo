#linux #unix #tools 

`gzip` is a command-line tool used for compressing and decompressing files.

### Compressing a file:

To compress a file using `gzip`, use the following command:

```bash
gzip filename
```

This will compress the file and replace the original file with a `.gz` compressed version (`filename.gz`).

### Decompressing a file:

To decompress a `.gz` file, you can use:

```bash
gzip -d filename.gz
gunzip filename.gz
```

### Compressing multiple files:

You can compress multiple files by listing them:

```bash
gzip file1 file2 file3
gzip *.fastq
```

This will compress each file individually and create `.gz` files for each.

### Keep original files when compressing:

By default, `gzip` deletes the original file after compression. To keep the original file, use the `-k` option:

```bash
gzip -k filename
```

### Compressing directories:

`gzip` cannot compress directories directly. However, you can use `tar` in combination with `gzip` to compress a directory:

```bash
tar cfz archive.tar.gz directory/
```

- `c`: Creates a new archive.
- `f`: Specifies the name of the archive file.
- `z`: Compresses the archive using `gzip`.
- `directory/`: The directory you want to compress.

### Viewing compressed file contents:

To view the contents of a `.gz` file without decompressing it fully, you can use the `zcat` command:

```bash
zcat filename.gz
```

Or, to view it page-by-page with `less`:

```bash
zless filename.gz
```