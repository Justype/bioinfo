#linux #unix 

Usage:

- **stdout**: data output from a command or file
- **stderr**: Err info or prompt from a command

Sometimes you will write code like this:

```bash
$ bowtie2 -x index -q input.fastq.gz -S aligned.sam > test.out

906352 reads; of these:
  906352 (100.00%) were unpaired; of these:
    464827 (51.29%) aligned 0 times
    439894 (48.53%) aligned exactly 1 time
    1631 (0.18%) aligned >1 times
48.71% overall alignment rate
```

You want to record the output, but the program still print things out. It is stderr not stdout. `>` only write stdout.

## Redirection

- `>`: Redirect stdout to a file (overwrite).
- `>>`: Redirect stdout to a file (append).
- `<`: Redirect stdin from a file.
- `2>`: Redirect stderr to a file (overwrite).
- `2>>`: Redirect stderr to a file (append).
- `&>`: Redirect both stdout and stderr to a file.

|Commands|Description|
|---|---|
|`command > file.out`|write **stdout** to a file|
|`command 1> file.out`|write **stdout** to a file|
|`command 2> file.err`|write **stderr** to a file|
|`2>&1`|redirect **stderr** to **stdout**|
|`>&1`|redirect to **stderr**|

## Number code

|0|stdin|
|---|---|
|1|stdout|
|2|stderr|

## Detect stdin in a script

```bash
if [ ! -t 0 ]; then
  echo file and stdin
  read input
  echo $input
else
  echo terminal
fi
```

```
$ ./test.sh 123
terminal

$ ./test.sh < test.in
file and stdin
123
```

## Output: stdout, stderr

```bash
#!/bin/bash

echo stdout
# redirect to stderr
echo stderr >&2
```

```
$ ./test.sh
stdout
stderr
$ ./test.sh > test.out
stderr
$ ./test.sh 2> test.err
stdout
$ ./test.sh &> test.both
```

## References

- [https://www.howtogeek.com/435903/what-are-stdin-stdout-and-stderr-on-linux/](https://www.howtogeek.com/435903/what-are-stdin-stdout-and-stderr-on-linux/)

