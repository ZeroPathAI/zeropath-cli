<p align="center">
  <a href="https://zeropath.com">
    <img src="https://sfo-zp-fe-assets.sfo3.cdn.digitaloceanspaces.com/ZeroPathPatch.svg" />
  </a>
</p>

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
First, authenticate with your ZeroPath API credentials:
```bash
zeropath auth <clientId> <clientSecret>
```

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

#### CI/CD Integration
For continuous integration pipelines, use the `--ci` flag to run PR/merge request scans:
```bash
# Basic CI scan
zeropath scan --repository-id <repositoryId> --ci

# CI scan with explicit PR branches
zeropath scan --repository-id <repositoryId> --ci \
  --pr-branch feature/new-feature \
  --pr-target main

# CI scan by repository URL
zeropath scan --repository-url https://github.com/owner/repo --vcs github --ci \
  --pr-branch feature/new-feature \
  --pr-target main
```

**CI Mode Exit Codes:**
- **0**: No vulnerabilities found
- **1**: Vulnerabilities detected (fails CI pipeline)

### Command Options

#### `zeropath scan` Options

| Option | Description |
|--------|-------------|
| `--repository-id <id>` | Scan an existing repository by ID |
| `--repository-url <url>` | Scan a repository by URL (requires `--vcs`) |
| `--vcs <provider>` | VCS provider: `github`, `gitlab`, `bitbucket`, or `generic` |
| `--branch <name>` | Branch to scan (for regular scans) |
| `--ci` | Run a CI/PR scan instead of a full scan |
| `--pr-branch <name>` | Source/feature branch for CI scans |
| `--pr-target <name>` | Target/base branch for CI scans |

### Examples

```bash
# Scan local code and generate SARIF report
zeropath scan ./my-project report.sarif

# Scan main branch of a repository
zeropath scan --repository-id abc-123-def --branch main

# CI pipeline scanning a pull request
zeropath scan --repository-id abc-123-def --ci \
  --pr-branch feature/security-fix \
  --pr-target main

# Scan a GitHub repository directly
zeropath scan --repository-url https://github.com/myorg/myapp --vcs github

# Scan a specific branch of a GitLab repository
zeropath scan --repository-url https://gitlab.com/myorg/myapp --vcs gitlab --branch develop
```

### Features

- **Real-time scanning**: All scans wait for completion and show progress
- **CI/CD ready**: Exit codes for pipeline integration (1 for vulnerabilities, 0 for clean)
- **Multiple VCS support**: GitHub, GitLab, Bitbucket, and generic Git repositories
- **Branch-aware**: Scan specific branches or PR/merge requests
- **SARIF output**: Industry-standard format for local scans
- **Vulnerability reporting**: Detailed breakdown by severity in CI mode

### Help
```bash
zeropath --help
zeropath scan --help
```

## Support
- Email: support@zeropath.com
- [Discord Community](https://discord.gg/ZRqDvZ6qjJ)
- [Issue Tracker](https://github.com/ZeroPathAI/zeropath-cli/issues)

## License
Copyright © 2025 ZeroPath Corp. All rights reserved.
