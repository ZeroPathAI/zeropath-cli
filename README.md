<p align="center">
  <a href="https://zeropath.com">
    <img src="https://sfo-zp-fe-assets.sfo3.cdn.digitaloceanspaces.com/ZeroPathPatch.svg" />
  </a>
</p>

# ZeroPath CLI

ZeroPath CLI provides command-line access to ZeroPath's AI-powered security scanning platform. Our scanning detects:
- Authentication and authorization vulnerabilities
- Application logic flaws
- Dependency issues and outdated packages (with SCA reachability)
- Security misconfigurations
- Command injection vulnerabilities
- File inclusion and path traversal attacks
- Secrets / hardcoded credentials
- **And more**

## Installation

### macOS Intel
```bash
wget https://github.com/ZeroPathAI/zeropath-cli/releases/latest/download/zeropath-macos -O zeropath
chmod +x zeropath
sudo mv zeropath /usr/local/bin/
```

### macOS ARM (Apple Silicon)
```bash
wget https://github.com/ZeroPathAI/zeropath-cli/releases/latest/download/zeropath-macos-arm64 -O zeropath
chmod +x zeropath
sudo mv zeropath /usr/local/bin/
```

### Linux x64
```bash
wget https://github.com/ZeroPathAI/zeropath-cli/releases/latest/download/zeropath-linux -O zeropath
chmod +x zeropath
sudo mv zeropath /usr/local/bin/
```

### Windows x64
```powershell
# Download using PowerShell
Invoke-WebRequest -Uri https://github.com/ZeroPathAI/zeropath-cli/releases/latest/download/zeropath-windows.exe -OutFile zeropath.exe
# Add to PATH - run in Command Prompt as Administrator
move zeropath.exe "C:\Windows\System32\"
```

## Usage

### Authentication
Authenticate with your ZeroPath API credentials:

```bash
zeropath auth <clientId> <clientSecret>
```

This stores credentials locally at `~/.config/zeropath/credentials.json`.

For CI/CD or other automated workflows, you can also authenticate with
environment variables:

```bash
export ZEROPATH_API_TOKEN_ID=<clientId>
export ZEROPATH_API_TOKEN_SECRET=<clientSecret>
```

When both environment variables are set, the CLI uses them automatically without
requiring `zeropath auth`.

> **Note:**
> For single-tenant environments, set the environment variable `ZEROPATH_ENVIRONMENT` before authentication:
>
> ```bash
> export ZEROPATH_ENVIRONMENT=https://<tenant>.branch.zeropath.com
> ```
>
> Example:
>
> ```bash
> export ZEROPATH_ENVIRONMENT=https://acme.branch.zeropath.com
> ```


### Scanning

#### Local Directory Scan (with SARIF output)
Scan a local directory and generate a SARIF report:
```bash
zeropath scan <directory> <outputFile.sarif>
```

#### Repository Scan (using existing repository)
Scan an already-configured repository by ID:
```bash
zeropath scan --repository-id <repositoryId>

# Scan a specific branch
zeropath scan --repository-id <repositoryId> --branch <branchName>
```

#### Repository Scan (by URL)
Scan a repository by its URL:
```bash
# GitHub repository
zeropath scan --repository-url https://github.com/owner/repo --vcs github

# GitLab repository
zeropath scan --repository-url https://gitlab.com/owner/repo --vcs gitlab

# Bitbucket repository
zeropath scan --repository-url https://bitbucket.org/owner/repo --vcs bitbucket

# Generic Git repository
zeropath scan --repository-url https://git.example.com/repo --vcs generic
```

#### On-Demand Code Scans Beta
Use `scan-code` to submit a diff, file, file set, or snippet for asynchronous
security review without starting a full repository scan.

```bash
# Scan the current Git working-tree diff
zeropath scan-code --diff

# Scan staged changes
zeropath scan-code --staged

# Scan one source file
zeropath scan-code --file src/api.ts

# Scan multiple source files
zeropath scan-code --files src/api.ts src/auth.ts

# Read a snippet from stdin
cat route.ts | zeropath scan-code --stdin --language typescript

# Force standalone mode when you do not want linked repository context
zeropath scan-code --diff --standalone
```

By default, `scan-code` uses your Git remote URL to automatically use linked
repository context when exactly one accessible ZeroPath repository matches. If
there is no match, it runs as a standalone scan. Use `--repository-id` to require
linked repository context, or `--standalone` to force a scan without repository
context.

You must choose exactly one input source per invocation: `--diff`, `--staged`,
`--file`, `--files`, `--snippet`, or `--stdin`.

#### CI/CD Integration
Repository scans and on-demand code scans exit with code 1 when vulnerabilities
are found, making them CI-ready out of the box:

```bash
# Scan a repository (exits 1 if issues found)
zeropath scan --repository-id <repositoryId>

# Scan a specific branch
zeropath scan --repository-id <repositoryId> --branch main

# Scan by repository URL
zeropath scan --repository-url https://github.com/owner/repo --vcs github

# Scan only the current Git diff and print JSON output
zeropath scan-code --diff --json
```

**Exit Codes:**
- **0**: No vulnerabilities found
- **1**: Vulnerabilities detected or command failed (fails CI pipeline)

### Command Options

#### `zeropath scan` Options

| Option | Description |
|--------|-------------|
| `--repository-id <id>` | Scan an existing repository by ID |
| `--repository-url <url>` | Scan a repository by URL (requires `--vcs`) |
| `--vcs <provider>` | VCS provider: `github`, `gitlab`, `bitbucket`, or `generic` |
| `--branch <name>` | Branch to scan |

#### `zeropath scan-code` Options

| Option | Description |
|--------|-------------|
| `--diff` | Scan the current Git working-tree diff |
| `--staged` | Scan the current staged Git diff |
| `--file <path>` | Scan one source file |
| `--files <path...>` | Scan one or more source files |
| `--snippet <text>` | Scan explicit snippet text; repeat for multiple snippets |
| `--stdin` | Read a snippet from stdin |
| `--language <language>` | Language hint for file or snippet input |
| `--label <label>` | Label for a `--snippet` or `--stdin` input |
| `--additional-context <text>` | Supplemental context for the scan |
| `--repository-id <id>` | Link the scan to a specific ZeroPath repository |
| `--remote-url <url>` | Git remote URL to auto-resolve to a linked repository |
| `--standalone` | Force a standalone scan without linked repository context |
| `--target-label <label>` | Label for the scanned local checkout or ad-hoc target |
| `--base-ref <ref>` | Base Git ref for metadata or diff generation |
| `--head-ref <ref>` | Head Git ref for metadata or diff generation |
| `--working-tree-ref <ref>` | Working tree label to include in metadata |
| `--json` | Print submit/status/results payloads as JSON |
| `--no-wait` | Submit the scan without waiting for results |
| `--timeout <seconds>` | Maximum seconds to wait for completion (default: 600) |

### Examples

```bash
# Scan local code and generate SARIF report
zeropath scan ./my-project report.sarif

# Scan main branch of a repository
zeropath scan --repository-id abc-123-def --branch main

# Scan a GitHub repository directly
zeropath scan --repository-url https://github.com/myorg/myapp --vcs github

# Scan a specific branch of a GitLab repository
zeropath scan --repository-url https://gitlab.com/myorg/myapp --vcs gitlab --branch develop

# Scan a local diff without starting a full repository scan
zeropath scan-code --diff
```

### Features

- **Real-time scanning**: Scans wait for completion and show progress by default
- **CI/CD ready**: Exit codes for pipeline integration (1 for vulnerabilities, 0 for clean)
- **Multiple VCS support**: GitHub, GitLab, Bitbucket, and generic Git repositories
- **Branch-aware**: Scan specific branches
- **On-demand code scans**: Submit diffs, files, file sets, or snippets without starting a full repository scan
- **SARIF output**: Industry-standard format for local scans
- **Vulnerability reporting**: Detailed breakdown by severity

### Help
```bash
zeropath --help
zeropath scan --help
```

## Support
- [CLI documentation](https://zeropath.com/docs/cli/introduction)
- [On-Demand Code Scans Beta guide](https://zeropath.com/docs/developer-tools/async-code-scans)
- Email: support@zeropath.com
- [Issue Tracker](https://github.com/ZeroPathAI/zeropath-cli/issues)

## License
Copyright © 2025 ZeroPath Corp. All rights reserved.
