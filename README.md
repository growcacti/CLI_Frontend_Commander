[README.md](https://github.com/user-attachments/files/32236104/README.md)
# CLI_Frontend_Commander
A linux command linefrontend insprired from other software
# CommandLine Commander

CommandLine Commander is a graphical Linux command launcher and reusable command-template builder written in Python and Tkinter. It makes frequently used terminal commands easier to find, review, customize, run, and save without having to remember every option.

The application uses only the Python standard library. Administrator commands use `pkexec`, allowing PolicyKit to display the normal graphical password prompt.

## Main features

- Large searchable collection of Linux commands grouped by category
- Command names and full command text displayed in a sortable table
- Editable command field before execution
- Selectable working directory
- Normal or administrator execution modes
- Live combined command output and error display
- Stop a running command and its child processes
- Save command output as a text file
- Copy commands to the clipboard
- Recall the 50 most recent commands from the current session
- Build and save reusable command templates
- Source-file, source-folder, destination-file, and destination-folder selectors
- Automatic command preview
- Confirmation prompts for commands that may delete files or make major system changes
- Integrated Help & Safety tab
- Resizable Tkinter grid layout with colored buttons and high-contrast terminal output

## Requirements

- Linux
- Python 3.10 or newer
- Tkinter
- PolicyKit/`pkexec` for graphical administrator prompts

Some commands require separate Linux utilities such as `curl`, `rsync`, `nmap`, `smartmontools`, or `lm-sensors`. The application itself does not install these tools.

On MX Linux or Debian, Tkinter and PolicyKit can be installed with:

```bash
sudo apt update
sudo apt install python3-tk policykit-1
```

## Running the application

Open a terminal in the program folder and run:

```bash
python3 CLI_Frontend_Commander.py
```

You can also make it executable:

```bash
chmod +x CLI_Frontend_Commander.py
./CLI_Frontend_Commander.py
```

## Command Commander tab

1. Select a category from the left side.
2. Use **Search** to filter commands by name or command text.
3. Select a command from the table. Double-clicking runs it immediately.
4. Review and edit the command in the **Command** field.
5. Select a working folder when the command uses relative paths.
6. Enable **Run as administrator** only when required.
7. Select **Run** and watch the results in the Output panel.

The **Recent Command** list remembers up to 50 commands during the current application session.

## Command Creator tab

The Command Creator builds reusable commands from templates. A template may contain these placeholders:

| Placeholder | Replaced with |
| --- | --- |
| `{source}` | Selected source file or folder |
| `{destination}` | Selected destination file or folder |
| `{username}` | Current Linux username |

Example template:

```bash
rsync -avh --progress "{source}" "{destination}"
```

Choose the source and destination, check the generated preview, and then run or save the command. Custom commands are stored in:

```text
~/.linux_command_frontend_commands.json
```

The program includes starter templates for copying, moving, synchronizing, creating symbolic links, checking disk usage, changing ownership, and other common jobs.

## Keyboard shortcuts

| Shortcut | Action |
| --- | --- |
| `Ctrl+L` | Focus the command search field |
| `Ctrl+Enter` | Run the selected or edited command |
| `Ctrl+Shift+C` | Copy the current command |
| `F1` | Open Help & Safety |

## Administrator mode

When administrator mode is enabled, the application runs the command through:

```bash
pkexec sh -c 'COMMAND'
```

PolicyKit should display a graphical authentication dialog. The program never stores your administrator password.

Commands already containing `sudo` may show a terminal-password error when administrator mode is disabled because the output panel is not an interactive terminal. For those commands, remove `sudo` from the editable command field and enable **Run as administrator**.

## Safety

Always read the complete command before running it. Linux commands can modify permissions, overwrite data, stop services, or remove files.

- Keep administrator mode off unless it is needed.
- Make backups before running commands that alter files or disks.
- Avoid recursive `chmod 777`; it grants access to every user and is rarely the correct permissions fix.
- Only use network, security, or testing commands on systems you own or have permission to test.
- A confirmation dialog is helpful but is not a substitute for understanding a command.

Lines beginning with `#` are examples or templates that normally require editing. The program removes the leading marker when a command is selected, so replace placeholders such as `TARGET`, `PORT`, `PACKAGE_NAME`, or `SERVICE_NAME` before running it.

## Troubleshooting

### Tkinter is missing

Install it on Debian-based systems with:

```bash
sudo apt install python3-tk
```

### Administrator prompt does not appear

Check whether `pkexec` is installed:

```bash
command -v pkexec
```

If no path is returned, install the PolicyKit package appropriate for your Linux distribution.

### A command reports “not found”

The command-line utility is not installed or is not in the current `PATH`. Install the relevant package using your distribution's package manager.

### A command waits forever or expects keyboard input

This application captures normal text output but is not a complete interactive terminal emulator. Commands requiring an interactive shell, full-screen interface, or repeated password input should be opened in a regular terminal.

### A command uses the wrong folder

Select the intended **Working folder** before running it, or use absolute file paths in the command.

## Files

```text
CLI_Frontend_Commander.py                Main application
README.md                                Documentation
~/.linux_command_frontend_commands.json  User-created command templates
```

## License

MIT License
