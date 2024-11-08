# ZeroPath CLI

ZeroPath CLI provides command-line access to ZeroPath's AI-powered security scanning platform. Our scanning detects:
- Authentication and authorization vulnerabilities
- Application logic flaws
- Dependency issues and outdated packages (with SCA reachability)
- Security misconfigurations
- Command injection vulnerabilities
- File inclusion and path traversal attacks
- Secrets / hardcoded credentials
- And more

## Installation

```bash
# macOS Intel
wget https://github.com/ZeroPathAI/zeropath-cli/releases/latest/download/zeropath-macos -O zeropath
chmod +x zeropath
sudo mv zeropath /usr/local/bin/

# macOS ARM (Apple Silicon)
wget https://github.com/ZeroPathAI/zeropath-cli/releases/latest/download/zeropath-macos-arm64 -O zeropath
chmod +x zeropath
sudo mv zeropath /usr/local/bin/

# Linux x64
wget https://github.com/ZeroPathAI/zeropath-cli/releases/latest/download/zeropath-linux -O zeropath
chmod +x zeropath
sudo mv zeropath /usr/local/bin/

# Linux ARM64
wget https://github.com/ZeroPathAI/zeropath-cli/releases/latest/download/zeropath-linux-arm64 -O zeropath
chmod +x zeropath
sudo mv zeropath /usr/local/bin/
```

## Usage

```bash
# Authenticate
zeropath auth <clientId> <clientSecret>

# Scan a directory
zeropath scan <directory> <outputFile>

# Show help
zeropath --help
```

## Support

- Email: support@zeropath.com
- [Discord Community](https://discord.gg/ZRqDvZ6qjJ)
- [Issue Tracker](https://github.com/ZeroPathAI/zeropath-cli/issues)

## License

Copyright © 2024 ZeroPath Corp. All rights reserved.
