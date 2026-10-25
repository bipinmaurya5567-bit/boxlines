# CLAUDE.md

Project notes for Claude. Read this at the start of every session in this repo.

## About the owner

Bipin. Prefers direct, actionable guidance.

## Reverse engineering toolkit: REA + Ghidra

Bipin uses [REA](https://github.com/morluto/rea) with Ghidra to reverse engineer apps, binaries and websites. Nothing installed in one chat carries over to the next, so at the start of any reverse-engineering task, check first and install only if missing.

### 1. Check

```bash
which rea && rea --version
export GHIDRA_INSTALL_DIR=/home/claude/tools/ghidra_12.1.4_PUBLIC
export JAVA_HOME=$(dirname $(dirname $(readlink -f $(which javac))))
rea doctor --json   # the ghidra-* checks should all be healthy
```

If `rea` is found and the Ghidra checks are healthy, skip the install. Hopper, IDA and JADX showing as missing is fine.

### 2. Install (only if missing)

If the `setup-rea-ghidra` skill is available, use it. Otherwise, on a Linux x64 workspace:

1. Needs Node 22.19+ or 24.11+, a 64-bit full JDK 21 (`javac -version`), and procps `ps`.
2. `npm install --global rea-agents`
3. Download Ghidra 12.1.4 (REA's Linux provider needs exactly this version) into a new empty directory:
   `https://github.com/NationalSecurityAgency/ghidra/releases/download/Ghidra_12.1.4_build/ghidra_12.1.4_PUBLIC_20260921.zip`
   Verify SHA-256 `ddac49f903da9d5bac833e5cc79395098b9c33cfd3279be5f31bd00387d2d4db`, then extract to `/home/claude/tools/ghidra_12.1.4_PUBLIC` and delete the zip.
4. Export `GHIDRA_INSTALL_DIR`, `JAVA_HOME`, and add `~/.npm-global/bin` to `PATH` (also append them to `~/.bashrc`).
5. Run `rea doctor --json` and confirm the ghidra-* checks pass.

### 3. Use

```bash
rea analyze /path/to/binary --provider ghidra --json
```

### Known limits

- In the cloud workspace, the shell can only reach allowlisted hosts and there is no Chrome, so REA's website analysis cannot run there. Websites like excalidraw.com are blocked. For open-source targets, read the public source with WebSearch and WebFetch instead.
- `gh release view` fails (GraphQL is blocked). Use plain `curl` on release asset URLs.
- On Bipin's own Windows laptop, Ghidra support is experimental (native x86-64 PE only) and `rea setup` does not register the MCP server, so add it by hand with the command `rea mcp`.
- Only analyze software Bipin owns or is authorized to examine (see REA's DISCLAIMER.md).

## This repo: Boxlines

A simple Excalidraw-style whiteboard: boxes, arrow connectors, text inside boxes, stroke and fill editing, save and open as JSON.

- `index.html` page structure, `css/style.css` styles, `js/app.js` editor logic. No build step.
- Rendering is SVG, with hand-drawn lines from rough.js loaded from a CDN.
- Saved files use Boxlines' own JSON format (`"type": "boxlines"`), not `.excalidraw`.
- Keep it dependency-free. Test changes by opening `index.html` in a browser.
