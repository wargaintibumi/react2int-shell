# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Proof-of-concept for CVE-2025-55182: a React Server Components (Next.js) Remote Code Execution vulnerability. Single-file Python exploit (`exploit.py`) targeting vulnerable Next.js server actions via crafted multipart form payloads that abuse prototype pollution in the RSC flight protocol.

**This tool is intended for authorized security testing and controlled lab environments only.**

## Running the Exploit

Requires Python 3 with `requests` installed (`pip install requests`).

```bash
python exploit.py -t <target-url> -c "<command>"
```

- `-t` / `--target`: Target URL or domain (auto-prepends `https://` if no scheme given)
- `-c` / `--command`: Shell command to execute on the target (default: `id`)

## Architecture

Everything lives in `exploit.py` with four classes:

- **ExploitConfig**: Target URL, command, timeout, URL normalization
- **BannerDisplay**: Terminal UI output (banner, config display, success/failure messages)
- **PayloadGenerator**: Builds the malicious multipart body exploiting Next.js server action deserialization — injects into `__proto__.then` to achieve `child_process.execSync` via `process.mainModule.require`
- **ExploitEngine**: Sends the crafted POST request with `Next-Action` headers, parses command output from the `X-Action-Redirect` response header

The exploit chain: crafted RSC payload → prototype pollution during deserialization → RCE via `child_process.execSync` → output exfiltrated through redirect header (`/login?a=<output>`).
