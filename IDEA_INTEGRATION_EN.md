# IntelliJ IDEA Integration Guide

This document provides detailed instructions on how to import, configure, and run the mtSecKill project in IntelliJ IDEA.

## Prerequisites

### 1. Install Go SDK
- Download and install Go 1.14 or higher
- Official download: https://golang.org/dl/
- Verify installation: Run `go version` in terminal

### 2. Install IntelliJ IDEA
- Download IntelliJ IDEA (Community or Ultimate edition)
- Official download: https://www.jetbrains.com/idea/download/
- Ultimate edition is recommended for better Go support

### 3. Install Go Plugin
- Open IntelliJ IDEA
- Go to `File` -> `Settings` (Windows/Linux) or `IntelliJ IDEA` -> `Preferences` (macOS)
- Select `Plugins`
- Search for "Go"
- Click `Install` to install the Go plugin
- Restart IDEA after installation

### 4. Install Google Chrome Browser
- This project requires Chrome for automation
- Download: https://www.google.com/chrome/

## Import Project

### Method 1: Import from Version Control

1. Open IntelliJ IDEA
2. Select `File` -> `New` -> `Project from Version Control`
3. Enter repository URL: `https://github.com/zqjzqj/mtSecKill.git` or your fork URL
4. Choose local path
5. Click `Clone`

### Method 2: Open Cloned Project

1. If you've already cloned the project, select `File` -> `Open`
2. Select the project root directory (containing `go.mod`)
3. Click `OK`

## Configure Project

### 1. Configure Go SDK

1. Open `File` -> `Settings` -> `Languages & Frameworks` -> `Go` -> `GOROOT`
2. Click `Add SDK` to select your local Go SDK installation path
3. Or select `Download` to let IDEA automatically download Go SDK

### 2. Configure Go Modules

1. Ensure `Enable Go modules integration` is enabled in `File` -> `Settings` -> `Go` -> `Go Modules`
2. The project will automatically recognize the `go.mod` file
3. IDEA will automatically download project dependencies

### 3. Download Dependencies

Run in IDEA's terminal (at the bottom):

```bash
go mod download
```

Or wait for IDEA to automatically sync dependencies.

## Build Project

### Build in IDEA

1. Open `cmd/main.go` file
2. Right-click in the editor area
3. Select `Run 'go build cmd/main.go'`

### Build Using Terminal

Run in IDEA's terminal:

```bash
# Build project
go build -o mtSecKill cmd/main.go

# Windows users
go build -o mtSecKill.exe cmd/main.go
```

After successful build, an executable file will be generated in the project root directory.

## Run Project

### Method 1: Using IDEA Run Configuration

1. Click `Add Configuration...` in the top-right corner
2. Click `+` to add new configuration
3. Select `Go Build`
4. Configure as follows:
   - **Name**: mtSecKill
   - **Run kind**: Package
   - **Package path**: github.com/zqijzqj/mtSecKill/cmd
   - **Working directory**: Project root directory
   - **Program arguments**: Add runtime parameters (optional), for example:
     ```
     -sku=100012043978 -num=2 -works=6 -time=09:59:59
     ```
5. Click `OK` to save
6. Click the green run button in the top-right corner to run the project

### Method 2: Run in Terminal

Run in IDEA's terminal:

```bash
# Run directly
go run cmd/main.go -sku=100012043978 -num=2 -works=6 -time=09:59:59

# Or run the built executable
./mtSecKill -sku=100012043978 -num=2 -works=6 -time=09:59:59

# Windows users
mtSecKill.exe -sku=100012043978 -num=2 -works=6 -time=09:59:59
```

## Debug Project

### Set Breakpoints

1. Click to the left of line numbers to add breakpoints (red dots)
2. Recommended to set breakpoints in the `main()` function of `cmd/main.go`

### Start Debugging

1. Use the Run Configuration configured above
2. Click the debug button in the top-right corner (green bug icon)
3. Or right-click `cmd/main.go` and select `Debug 'go build cmd/main.go'`

### Debug Operations

- **F8**: Step Over
- **F7**: Step Into
- **Shift+F8**: Step Out
- **F9**: Resume Program
- **Ctrl+F8**: Toggle Breakpoint

## Runtime Parameters

You can add the following parameters in configuration:

| Parameter | Description | Default | Example |
|-----------|-------------|---------|---------|
| `-sku` | Product SKU ID | 100012043978 | `-sku=100012043978` |
| `-num` | Purchase quantity | 2 | `-num=2` |
| `-works` | Concurrency count | 7 | `-works=6` |
| `-time` | Start time (without date) | 09:59:59 | `-time=09:59:59` |
| `-execPath` | Browser executable path | Auto-detect | `-execPath=/path/to/chrome` |
| `-eid` | JD EID (optional) | Empty | `-eid=YOUR_EID` |
| `-fp` | JD FP (optional) | Empty | `-fp=YOUR_FP` |

## Common Issues

### 1. Go SDK Not Recognized

**Problem**: IDEA prompts that Go SDK cannot be found

**Solution**:
- Check if `File` -> `Settings` -> `Go` -> `GOROOT` is configured correctly
- Re-download or manually specify Go SDK path

### 2. Dependency Download Failed

**Problem**: Cannot download Go modules dependencies

**Solution**:
```bash
# Set Go proxy (for users in mainland China)
go env -w GOPROXY=https://goproxy.cn,direct
go env -w GOSUMDB=sum.golang.org

# Or use other proxies
go env -w GOPROXY=https://goproxy.io,direct
```

### 3. Browser Path Not Found

**Problem**: Runtime error about browser executable path not found

**Solution**:
- Use `-execPath` parameter to manually specify Chrome browser path
- Windows: `C:\Program Files\Google\Chrome\Application\chrome.exe`
- macOS: `/Applications/Google Chrome.app/Contents/MacOS/Google Chrome`
- Linux: `/usr/bin/google-chrome` or `/usr/bin/chromium-browser`

### 4. Permission Issues (macOS/Linux)

**Problem**: Executable file lacks execute permission

**Solution**:
```bash
chmod +x mtSecKill
```

### 5. Build Failed

**Problem**: Compilation errors

**Solution**:
```bash
# Clean cache and rebuild
go clean -cache
go build -o mtSecKill cmd/main.go
```

## Project Structure

```
mtSecKill/
├── cmd/                    # Main program entry
│   └── main.go            # Main function
├── chromedpEngine/        # Chrome browser automation engine
│   └── allocator.go       # Browser allocator
├── global/                # Global configuration and utilities
│   ├── constant.go        # Constants
│   └── helper.go          # Helper functions
├── logs/                  # Logging module
│   └── logs.go            # Logging functionality
├── secKill/               # SecKill core logic
│   └── jdSecKill.go       # JD SecKill implementation
├── go.mod                 # Go modules dependency management
├── go.sum                 # Dependency checksums
└── README.md              # Project documentation
```

## Code Navigation Tips

### Shortcuts

- **Ctrl+N** (Cmd+O on Mac): Search class/struct
- **Ctrl+Shift+N** (Cmd+Shift+O on Mac): Search file
- **Ctrl+B** (Cmd+B on Mac): Go to definition
- **Ctrl+Alt+B** (Cmd+Alt+B on Mac): Go to implementation
- **Alt+F7**: Find usages
- **Ctrl+F**: Find in current file
- **Ctrl+Shift+F**: Global find

### Code Refactoring

- **Shift+F6**: Rename
- **Ctrl+Alt+M**: Extract method
- **Ctrl+Alt+V**: Extract variable
- **Ctrl+Alt+L**: Format code

## Development Recommendations

1. **Code Formatting**: Use `gofmt` to maintain consistent code format
   - IDEA can auto-format on save: `Settings` -> `Tools` -> `File Watchers`

2. **Code Inspection**: Enable Go static checks
   - `Settings` -> `Editor` -> `Inspections` -> `Go`

3. **Version Control**: Use IDEA's built-in Git tools
   - `VCS` menu provides complete Git operations

4. **Testing**: If writing test files (*_test.go)
   - Right-click test file or test function, select `Run` or `Debug`

## Additional Resources

- [Go Official Documentation](https://golang.org/doc/)
- [IntelliJ IDEA Go Plugin Documentation](https://www.jetbrains.com/help/go/quick-start-guide-goland.html)
- [Original Project README](README.md)

## Important Notes

⚠️ **Important**:
- This project is for learning and research purposes only
- Please comply with relevant laws, regulations, and website terms of use
- Read the special statement in README.md carefully before use
- Do not use for commercial purposes

---

If you have any questions, please refer to the project [Issues](https://github.com/zqjzqj/mtSecKill/issues) or submit a new issue.
