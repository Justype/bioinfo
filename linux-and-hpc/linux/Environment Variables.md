#linux #unix 

**Environment variables** are key-value pairs that define the environment in which processes run.

- `env` view all env variables
- `export VAR` setting env variable
- `echo $VAR` printing env variable
- `unset VAR` unsetting variable

PATH: `export PATH=/abs/path/to/directory:$PATH`

## Usage:

- **System Configuration**: Control system behavior (e.g., paths, locale, user settings).
- **User Customization**: Allow users to modify their environment, like setting default editor.
- **Process Settings**: Provide runtime parameters to applications and scripts.

## Common Environment Variables

| Variable | Description                                | Example                   |
| -------- | ------------------------------------------ | ------------------------- |
| `PATH`   | Directories to search for executable files | `/usr/local/bin:/usr/bin` |
| `HOME`   | Path to the current user's home directory  | `/home/user`              |
| `USER`   | The name of the current logged-in user     | `user`                    |
| `PWD`    | Current working directory                  | `/home/user/projects`     |

## Setting Environment Variables

- **Temporary (Session-only)**:

```bash
export MY_VAR="value"
```

This only lasts for the duration of the session or the current shell.

- Permanent (Across sessions): Add the export statement to configuration files like `~/.bashrc` or `~/.bash_profile`.

```bash
echo 'export MY_VAR="value"' >> ~/.bashrc
```

After editing `~/.bashrc`, reload it:

```bash
source ~/.bashrc
```

## Accessing Environment Variables

- Print value: `echo $VAR_NAME`
- List all environment variables `env` or `printenv`

## Unsetting Variables

- To remove a variable: `unset VAR_NAME`
