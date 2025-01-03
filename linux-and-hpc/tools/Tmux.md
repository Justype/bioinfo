#linux #unix #tools 

**Tmux** (Terminal Multiplexer) allows you to manage multiple terminal sessions from a single window. It enables you to

1. Split your terminal (different pane)
2. [[Run Jobs in Background]]
3. Reconnect to sessions even after disconnecting from SSH.

## Key Components

| Session               | Window                 | Pane                      |
| --------------------- | ---------------------- | ------------------------- |
| Top-level container   | Workspace in a session | Subdivision of a window   |
| Group related tasks   | Manage separate tasks  | Multitask within a window |
| `tmux switch-session` | `Ctrl+b`, `n/p`        | `Ctrl+b`, arrow keys      |

![tmux pane](https://i.imgur.com/cO8SnPv.png)

![tmux sessions](https://i.imgur.com/s8CcceO.png)

## Usage

### Sessions

- New session: `tmux`
- List sessions: `tmux ls`
- Detach session: `tmux detach`
- Attach session: `tmux attach -t name`

### General Navigation

| Action              | Shortcut              |
| ------------------- | --------------------- |
| Detach session      | `Ctrl+b`, `d`         |
| List sessions       | `tmux list-sessions`  |
| Attach to a session | `tmux attach -t name` |
| Switch session      | `tmux switch -t name` |

### Window Management

| Action               | Shortcut      |
| -------------------- | ------------- |
| Create new window    | `Ctrl+b`, `c` |
| Next window          | `Ctrl+b`, `n` |
| Previous window      | `Ctrl+b`, `p` |
| List windows         | `Ctrl+b`, `w` |
| Rename window        | `Ctrl+b`, `,` |
| Close current window | `Ctrl+b`, `&` |

### Pane Management

| Action             | Shortcut             |
| ------------------ | -------------------- |
| Split vertically   | `Ctrl+b`, `"`        |
| Split horizontally | `Ctrl+b`, `%`        |
| Switch panes       | `Ctrl+b`, then arrow |
| Resize pane        | `Ctrl+b`, hold arrow |
| Close current      | `Ctrl+b`, `x`        |

### Copy Mode

| Action            | Shortcut              |
| ----------------- | --------------------- |
| Enter copy mode   | `Ctrl+b`, `[`         |
| Scroll            | Arrow keys            |
| Search            | `/`                   |
| Copy text         | `Space`, then `Enter` |
| Paste copied text | `Ctrl+b`, `]`         |

## Useful Configs in `.tmux.conf`

```bash
# enable mouse mode
set -g mouse on
# enable clipboard OSC52
set -s set-clipboard on
```

## References

- [https://tmuxcheatsheet.com/](https://tmuxcheatsheet.com/)