# Install Quorum

## Quick install (npm)

```bash
npm i -g @xudesheng/quorum
quorum --version
```

## Homebrew (macOS/Linux)

```bash
brew tap xudesheng/tap
brew install quorum
quorum --version
```

## winget (Windows)

```powershell
winget install --id Desheng.Quorum -e
quorum --version
```

Notes:

- `winget` availability can lag behind release publication due to upstream index sync/review.

## bunx (no global install)

```bash
bunx @xudesheng/quorum --version
```

## GitHub Release binaries

Use the latest release from:
https://github.com/xudesheng/quorum/releases/latest

Assets:
- `quorum-v<version>-windows-x64.zip`
- `quorum-v<version>-linux-x64-musl.tar.gz`
- `quorum-v<version>-macos-universal.zip`

Also download checksum file:
- `quorum-v<version>-SHA256SUMS.txt`

## Verify checksum (recommended)

Linux/macOS:

```bash
sha256sum -c quorum-v<version>-SHA256SUMS.txt
```

Windows (PowerShell):

```powershell
Get-FileHash .\quorum-v<version>-windows-x64.zip -Algorithm SHA256
```

Compare the output hash with the corresponding line in `quorum-v<version>-SHA256SUMS.txt`.

## Extract and run

After extraction, add `quorum` (`quorum.exe` on Windows) to `PATH` and run:

```bash
quorum --version
```
