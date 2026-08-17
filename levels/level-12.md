# Bandit Level 12 → Level 13

## Level Objective

Find the password for the next level stored in `data.txt`, which is a hex dump of a file that has been repeatedly compressed using different archive formats.

## Approach

This level was a chain of different compression and archive formats.

I first created an isolated workspace in `/tmp` so I could safely modify and extract the files without affecting the original `data.txt`.

I then:

1. Reverted the hex dump back into its original binary form.
2. Used `file` to identify the format of the resulting file.
3. Decompressed or extracted the file according to its identified format.
4. Repeated this process until reaching the final ASCII text file.
5. Used `cat` to read the password.

## Commands Used

### 1. Create an Isolated Workspace

```bash
mkdir /tmp/myworkspace
cp data.txt /tmp/myworkspace/
cd /tmp/myworkspace/
```

### 2. Revert the Hex Dump

```bash
xxd -r data.txt > data
```

### 3. Identify and Decompress Each Layer

```bash
file data
bzip2 -d data
```

The resulting file was identified as gzip-compressed data.

```bash
file data.out
mv data.out data.gz
gzip -d data.gz
```

The resulting file was a POSIX tar archive.

```bash
file data
tar -xf data
```

This extracted `data5.bin`, which was another tar archive.

```bash
file data5.bin
tar -xf data5.bin
```

This extracted `data6.bin`, which was bzip2-compressed.

```bash
file data6.bin
bzip2 -d data6.bin
```

The resulting file was another tar archive.

```bash
file data6.bin.out
tar -xf data6.bin.out
```

This extracted `data8.bin`, which was gzip-compressed.

```bash
file data8.bin
mv data8.bin data8.gz
gzip -d data8.gz
```

Finally, the resulting file was identified as ASCII text.

```bash
file data8
cat data8
```

## Explanation

### Reverting a Hex Dump with `xxd`

The `xxd` command can create a hexadecimal representation of binary data.

The `-r` option reverses this process and converts a hexadecimal dump back into its original binary form.

```bash
xxd -r data.txt > data
```

### Identifying File Types with `file`

The `file` command examines the contents of a file and identifies its format based on its data and file signatures.

This was especially important in this level because the files did not necessarily have extensions that matched their actual formats.

```bash
file data
```

The output told me which decompression or extraction tool to use next.

### Decompressing and Extracting Files

Different formats require different tools:

- `bzip2 -d` → decompresses bzip2 data
- `gzip -d` → decompresses gzip data
- `tar -xf` → extracts tar archives

The key to solving the level was repeatedly identifying the current file type and applying the appropriate tool.

### File Extensions

Some utilities, such as `gzip`, expect the file to have an appropriate extension.

For example, when a file was identified as gzip-compressed but did not have a `.gz` extension, I used `mv` to rename it before decompressing it.

```bash
mv data.out data.gz
gzip -d data.gz
```

## What I Learned

- How to reverse a hex dump using `xxd -r`
- How to identify unknown file formats using `file`
- How to work with gzip and bzip2 compression
- How to extract tar archives
- Why file extensions and actual file formats are not necessarily the same thing
- How to solve a multi-layered problem by repeatedly identifying the current state and choosing the appropriate next command

---

**[← Previous Level](./level-11.md) | [Next Level →](./level-13.md)**