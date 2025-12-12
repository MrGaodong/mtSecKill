# IntelliJ IDEA 集成指南

本文档详细说明如何在 IntelliJ IDEA 中导入、配置和运行 mtSecKill 项目。

## 前置要求

### 1. 安装 Go SDK
- 下载并安装 Go 1.14 或更高版本
- 官方下载地址：https://golang.org/dl/
- 安装后验证：在终端运行 `go version`

### 2. 安装 IntelliJ IDEA
- 下载 IntelliJ IDEA（Community 或 Ultimate 版本均可）
- 官方下载地址：https://www.jetbrains.com/idea/download/
- 推荐使用 Ultimate 版本，对 Go 支持更完善

### 3. 安装 Go 插件
- 打开 IntelliJ IDEA
- 进入 `File` -> `Settings` (Windows/Linux) 或 `IntelliJ IDEA` -> `Preferences` (macOS)
- 选择 `Plugins`
- 搜索 "Go"
- 点击 `Install` 安装 Go 插件
- 安装完成后重启 IDEA

### 4. 安装 Google Chrome 浏览器
- 本项目依赖 Chrome 浏览器进行自动化操作
- 下载地址：https://www.google.com/chrome/

## 导入项目

### 方法一：从 Version Control 导入

1. 打开 IntelliJ IDEA
2. 选择 `File` -> `New` -> `Project from Version Control`
3. 输入仓库地址：`https://github.com/zqjzqj/mtSecKill.git` 或您的 fork 地址
4. 选择本地保存路径
5. 点击 `Clone`

### 方法二：打开已克隆的项目

1. 如果已经克隆了项目，选择 `File` -> `Open`
2. 选择项目根目录（包含 `go.mod` 的目录）
3. 点击 `OK`

## 配置项目

### 1. 配置 Go SDK

1. 打开 `File` -> `Settings` -> `Languages & Frameworks` -> `Go` -> `GOROOT`
2. 点击 `Add SDK` 选择本地安装的 Go SDK 路径
3. 或者选择 `Download` 让 IDEA 自动下载 Go SDK

### 2. 配置 Go Modules

1. 确保 `File` -> `Settings` -> `Go` -> `Go Modules` 中启用了 `Enable Go modules integration`
2. 项目会自动识别 `go.mod` 文件
3. IDEA 会自动下载项目依赖

### 3. 下载依赖

在 IDEA 底部的终端（Terminal）中运行：

```bash
go mod download
```

或者等待 IDEA 自动同步依赖。

## 构建项目

### 在 IDEA 中构建

1. 打开 `cmd/main.go` 文件
2. 右键点击文件内容区域
3. 选择 `Run 'go build cmd/main.go'`

### 使用终端构建

在 IDEA 底部的终端中运行：

```bash
# 构建项目
go build -o mtSecKill cmd/main.go

# Windows 用户
go build -o mtSecKill.exe cmd/main.go
```

构建成功后，会在项目根目录生成可执行文件。

## 运行项目

### 方法一：使用 IDEA Run Configuration

1. 点击右上角 `Add Configuration...`
2. 点击 `+` 添加新配置
3. 选择 `Go Build`
4. 配置如下：
   - **Name**: mtSecKill
   - **Run kind**: Package
   - **Package path**: github.com/zqijzqj/mtSecKill/cmd
   - **Working directory**: 项目根目录
   - **Program arguments**: 添加运行参数（可选），例如：
     ```
     -sku=100012043978 -num=2 -works=6 -time=09:59:59
     ```
5. 点击 `OK` 保存
6. 点击右上角的绿色运行按钮运行项目

### 方法二：在终端中运行

在 IDEA 底部的终端中运行：

```bash
# 直接运行
go run cmd/main.go -sku=100012043978 -num=2 -works=6 -time=09:59:59

# 或运行已构建的可执行文件
./mtSecKill -sku=100012043978 -num=2 -works=6 -time=09:59:59

# Windows 用户
mtSecKill.exe -sku=100012043978 -num=2 -works=6 -time=09:59:59
```

## 调试项目

### 设置断点

1. 在代码行号左侧点击，添加断点（红色圆点）
2. 建议在 `cmd/main.go` 的 `main()` 函数中设置断点

### 启动调试

1. 使用上面配置的 Run Configuration
2. 点击右上角的调试按钮（绿色虫子图标）
3. 或右键点击 `cmd/main.go`，选择 `Debug 'go build cmd/main.go'`

### 调试操作

- **F8**: Step Over（单步跳过）
- **F7**: Step Into（单步进入）
- **Shift+F8**: Step Out（跳出）
- **F9**: Resume Program（继续执行）
- **Ctrl+F8**: Toggle Breakpoint（切换断点）

## 运行参数说明

在配置中可以添加以下参数：

| 参数 | 说明 | 默认值 | 示例 |
|------|------|--------|------|
| `-sku` | 商品 SKU ID | 100012043978 | `-sku=100012043978` |
| `-num` | 购买数量 | 2 | `-num=2` |
| `-works` | 并发数 | 7 | `-works=6` |
| `-time` | 开始时间（不带日期） | 09:59:59 | `-time=09:59:59` |
| `-execPath` | 浏览器执行路径 | 自动检测 | `-execPath=/path/to/chrome` |
| `-eid` | 京东 EID（可选） | 空 | `-eid=YOUR_EID` |
| `-fp` | 京东 FP（可选） | 空 | `-fp=YOUR_FP` |

## 常见问题

### 1. Go SDK 未识别

**问题**: IDEA 提示找不到 Go SDK

**解决方案**:
- 检查 `File` -> `Settings` -> `Go` -> `GOROOT` 是否正确配置
- 重新下载或手动指定 Go SDK 路径

### 2. 依赖下载失败

**问题**: 无法下载 Go modules 依赖

**解决方案**:
```bash
# 设置 Go 代理（中国大陆用户）
go env -w GOPROXY=https://goproxy.cn,direct
go env -w GOSUMDB=sum.golang.org

# 或者使用其他代理
go env -w GOPROXY=https://goproxy.io,direct
```

### 3. 浏览器路径未找到

**问题**: 运行时提示找不到浏览器执行路径

**解决方案**:
- 使用 `-execPath` 参数手动指定 Chrome 浏览器路径
- Windows: `C:\Program Files\Google\Chrome\Application\chrome.exe`
- macOS: `/Applications/Google Chrome.app/Contents/MacOS/Google Chrome`
- Linux: `/usr/bin/google-chrome` 或 `/usr/bin/chromium-browser`

### 4. 权限问题（macOS/Linux）

**问题**: 可执行文件没有执行权限

**解决方案**:
```bash
chmod +x mtSecKill
```

### 5. 构建失败

**问题**: 编译时出现错误

**解决方案**:
```bash
# 清理缓存并重新构建
go clean -cache
go build -o mtSecKill cmd/main.go
```

## 项目结构说明

```
mtSecKill/
├── cmd/                    # 主程序入口
│   └── main.go            # 主函数
├── chromedpEngine/        # Chrome 浏览器自动化引擎
│   └── allocator.go       # 浏览器分配器
├── global/                # 全局配置和工具
│   ├── constant.go        # 常量定义
│   └── helper.go          # 辅助函数
├── logs/                  # 日志模块
│   └── logs.go            # 日志功能
├── secKill/               # 秒杀核心逻辑
│   └── jdSecKill.go       # 京东秒杀实现
├── go.mod                 # Go modules 依赖管理
├── go.sum                 # 依赖校验和
└── README.md              # 项目说明
```

## 代码导航技巧

### 快捷键

- **Ctrl+N** (Cmd+O on Mac): 搜索类/结构体
- **Ctrl+Shift+N** (Cmd+Shift+O on Mac): 搜索文件
- **Ctrl+B** (Cmd+B on Mac): 跳转到定义
- **Ctrl+Alt+B** (Cmd+Alt+B on Mac): 跳转到实现
- **Alt+F7**: 查找使用处
- **Ctrl+F**: 当前文件查找
- **Ctrl+Shift+F**: 全局查找

### 代码重构

- **Shift+F6**: 重命名
- **Ctrl+Alt+M**: 提取方法
- **Ctrl+Alt+V**: 提取变量
- **Ctrl+Alt+L**: 格式化代码

## 开发建议

1. **代码格式化**: 使用 `gofmt` 保持代码格式统一
   - IDEA 可以自动在保存时格式化：`Settings` -> `Tools` -> `File Watchers`

2. **代码检查**: 启用 Go 静态检查
   - `Settings` -> `Editor` -> `Inspections` -> `Go`

3. **版本控制**: 使用 IDEA 内置的 Git 工具
   - `VCS` 菜单提供了完整的 Git 操作

4. **测试**: 如果编写测试文件（*_test.go）
   - 右键测试文件或测试函数，选择 `Run` 或 `Debug`

## 更多资源

- [Go 官方文档](https://golang.org/doc/)
- [IntelliJ IDEA Go 插件文档](https://www.jetbrains.com/help/go/quick-start-guide-goland.html)
- [项目原始 README](README.md)

## 注意事项

⚠️ **重要提示**：
- 本项目仅供学习和研究使用
- 请遵守相关法律法规和网站使用条款
- 使用前请仔细阅读 README.md 中的特别声明
- 不得用于商业用途

---

如有问题，请参考项目 [Issues](https://github.com/zqjzqj/mtSecKill/issues) 或提交新的 Issue。
