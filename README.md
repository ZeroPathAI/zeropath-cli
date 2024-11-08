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
```bash
zeropath auth <clientId> <clientSecret>
```

### Scanning
```bash
zeropath scan <directory> <outputFile>
```

### Help
```bash
zeropath --help
```

## Support
- Email: support@zeropath.com
- [Discord Community](https://discord.gg/ZRqDvZ6qjJ)
- [Issue Tracker](https://github.com/ZeroPathAI/zeropath-cli/issues)

## License
Copyright © 2024 ZeroPath Corp. All rights reserved.
