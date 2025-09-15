# Warp Account Manager - Automated Account Creation & Management Tool

[English](#english) | [中文](#中文)

---

# English

> **Quick Summary**:
> - **Purpose**: Automatically create and manage multiple Warp.dev accounts for continuous usage without quota limitations
> - **Key Features**: Batch account creation, temporary email integration, proxy support, account health monitoring
> - **Components**: GUI Application + Batch Registration System + Temporary Email Manager + Chrome Extension + Proxy Management
> - **Main Workflow**: Configure proxies → Create accounts in batches → Monitor account health → Use Chrome extension to collect existing accounts
> - **Advanced**: Concurrent processing, smart retry logic, proxy rotation, automated email verification

## Language Settings & Detection

- Auto-detection rules:
  - Turkish/Azerbaijani language systems → Turkish interface (tr)
  - Chinese systems → Default English interface (en)  
  - Other systems → Default English (en)
- Check current detection results:
  - Language detection is now automatic based on system settings
  - The interface will automatically display in the appropriate language

## macOS Support

- macOS users can run the application using Python 3:
  ```bash
  python3 warp_account_manager.py
  ```
- Ensure you have the required dependencies installed:
  ```bash
  pip3 install -r requirements.txt
  ```
- First run auto-generates mitmproxy root certificate and attempts installation/trust; if auto-install fails, the app will guide you through manual trust setup.

## Recent Update Highlights

- **Cross-Platform Support**: Full Windows/macOS/Linux compatibility with automatic system detection
- **Certificate Automation**: Auto-create mitmproxy certificates on first startup and attempt auto-installation (Windows/macOS), with manual guidance dialog on failure
- **Enhanced Proxy Management**: Clear diagnostics and solution suggestions for port conflicts/missing dependencies/permission issues
- **Language Detection & Localization**: Automatic interface language selection based on system locale
- **Documentation & Assets**: Comprehensive multilingual README with interface screenshots
- **Performance Optimization**: Concurrent batch refresh (multithreading), reducing overall time by ~60-70% in 10-account scenarios
- **Code Optimization**: Streamlined codebase with focus on core functionality

---

## What It Is (Plain Explanation)

Warp Account Manager is a powerful automation tool for **creating and managing multiple Warp.dev accounts**.

**Core Functionality:**
- **Automated Account Creation**: Automatically creates Warp.dev accounts using temporary email services
- **Batch Processing**: Create dozens of accounts simultaneously with concurrent processing
- **Proxy Integration**: Uses proxy rotation to avoid IP-based restrictions during account creation
- **Email Verification**: Automatically handles email verification process for new accounts
- **Account Management**: Stores and manages created accounts with health status monitoring
- **Browser Integration**: Chrome extension to collect account data from existing browser sessions

**Why you need it:**
- Warp.dev has usage quotas per account - multiple accounts enable continuous usage
- Manual account creation is time-consuming - automation saves hours of work
- Proxy rotation helps avoid rate limiting and IP blocks during bulk operations
- Organized account management prevents losing track of multiple credentials

---

## What You Get

- **GUI Application** (Windows/macOS/Linux): Intuitive interface for batch account creation and management
- **Batch Registration System**: Create 10, 50, or 100+ accounts automatically with configurable concurrency
- **Temporary Email Integration**: Uses tmailor.com and other services for automated email verification
- **Proxy Management**: Load and rotate through proxy lists for distributed account creation
- **Chrome Extension**: "Add to Warp Manager" button on Warp.dev pages for collecting existing account data
- **Database Storage**: SQLite database for organized account storage and health tracking
- **Advanced Features**: Smart retry logic, progress monitoring, error handling, and performance optimization

### Application Interface Screenshots

#### English Interface
![English Interface](img/en.png)
*English interface: Shows account management, proxy control and status monitoring*

#### Turkish Interface  
![Turkish Interface](img/tr.jpg)
*Turkish interface: Multi-language support, adapted for different regional users*

---

## Installation Steps (Step by Step)

### 1. Install Dependencies

1) **Install Python** (recommended 3.8+)
2) **Install required packages:**
```bash
pip install -r requirements.txt
```

**Core Dependencies:**
- `PyQt5` - GUI framework for the desktop application
- `requests` - HTTP requests for API communications
- `curl_cffi` - Advanced HTTP client for bypassing anti-bot protection
- `mitmproxy` - HTTPS proxy for advanced account management
- `psutil` - System process monitoring
- `aiohttp` - Async HTTP client for concurrent operations

3) **Optional but recommended:**
```bash
pip install curl_cffi
```
For better success rates with temporary email services and anti-bot protection.

### 2. Setup Proxy List (Recommended)

Create a `proxy.txt` file in the project directory with your proxy list:
```
http://user:pass@proxy1.example.com:8080
https://user:pass@proxy2.example.com:3128
socks5://user:pass@proxy3.example.com:1080
proxy4.example.com:8080
```

**Proxy formats supported:**
- `http://user:pass@host:port`
- `https://user:pass@host:port`
- `socks5://user:pass@host:port`
- `host:port` (no auth, assumes HTTP)

### 3. Start Application

**Windows:**
```bash
python warp_account_manager.py
```

**macOS:**
```bash
python3 warp_account_manager.py
```

**Linux:**
```bash
python3 warp_account_manager.py
```

### 3.3 Install and Trust mitmproxy Certificate (Critical)
- On first startup, the app will prompt to install mitmproxy root certificate.
- Purpose: Let system trust proxy's "man-in-the-middle" decryption of HTTPS, used only for local machine and Warp domains.
- Operation:
  1. Click "Open Certificate File" button, locate `mitmproxy-ca-cert.cer`.
  2. Follow in-app step prompts to import it into "Trusted Root Certification Authorities".
  3. After completion, app status will show certificate ready.

Note: Without certificate installation, proxy cannot properly decrypt and inject tokens, functionality will be abnormal.

### 3.4 Install Browser Extension

#### Chrome Installation:
1) Open Chrome and enter `chrome://extensions/` in address bar
2) Enable "Developer mode" in top right corner
3) Click "Load unpacked extension", select `chrome-extension` directory in project

#### Microsoft Edge Installation:
1) Open Edge and enter `edge://extensions/` in address bar  
2) Enable "Developer mode" in bottom left corner
3) Click "Load unpacked", select `chrome-extension` directory in project

📝 **Note**: No need to set permissions after extension installation, it will work automatically on Warp.dev pages

---

## Usage Workflow

### Method 1: Automated Batch Creation (Recommended)

1) **Setup Proxies**: Add proxy list to `proxy.txt` file
2) **Launch Application**: Run `python warp_account_manager.py`
3) **Configure Batch Settings**:
   - Choose number of accounts to create (e.g., 50)
   - Select fast mode for quicker processing
   - Set concurrent workers (default: 15)
4) **Start Batch Creation**: Click "Start Batch Registration"
5) **Monitor Progress**: Watch real-time progress and success/failure stats
6) **Review Results**: Check created accounts in the main interface

### Method 2: Chrome Extension Collection

1) **Install Extension**: Load `chrome-extension` folder in Chrome Developer Mode
2) **Visit Warp.dev**: Go to `https://app.warp.dev/logged_in/remote`
3) **Collect Account**: Click "Add to Warp Manager" button that appears
4) **Auto-Import**: Account data is automatically saved to your local database

### Account Management Features

- **Health Monitoring**: Track which accounts are active/banned/expired
- **Batch Operations**: Delete, export, or refresh multiple accounts
- **Usage Statistics**: Monitor account creation success rates
- **Database Management**: SQLite database with full CRUD operations

---

## Performance Optimization Overview (Concurrent Batch Refresh)

- Default "refresh limits" operation has concurrent processing enabled (2-5 threads, auto-adjusted by account count).
- Individual account failure won't affect other accounts, with 60s timeout protection and lightweight rate control (0.1-0.5s random delay).
- Optional customization: Create user_config.json in root directory to override concurrency and delay parameters, for example:

```json
{
  "concurrent": {
    "max_workers_small": 2,
    "max_workers_medium": 3, 
    "max_workers_large": 5,
    "api_delay_min": 0.1,
    "api_delay_max": 0.5
  }
}
```

> Tip: Too high concurrency may trigger server rate limiting or local resource bottlenecks, recommend gradually increasing while observing.


---

## Contact & Support

### 📞 Contact Methods

| Contact Method | Link/Info | Purpose |
|---|---|---|
| 🌐 **GitHub Repository** | [warp.dev_account_manager](https://github.com/hj01857655/warp.dev_account_manager) | Source code, releases, bug reports |
| 📱 **Telegram Channel** | [@warp5215](https://t.me/warp5215) | Real-time chat, tips, feedback |
| 💬 **QQ Group** | [WARP Tools Community](https://qm.qq.com/q/2zXp6ziTnC) | Chinese user community |
| 🐞 **Bug Reports** | [GitHub Issues](https://github.com/hj01857655/warp.dev_account_manager/issues) | Report issues, feature requests |
| 📚 **Documentation** | [Wiki Pages](https://github.com/hj01857655/warp.dev_account_manager/wiki) | Detailed tutorials, FAQ |

### 🆘 Getting Help

**When you encounter issues, please follow this order:**

1. 📖 **Read Documentation** - First check this README's troubleshooting section
2. 🔍 **Search History** - Look for similar issues in GitHub Issues
3. 💭 **Community Discussion** - Ask in Telegram channel or QQ group
4. 🐛 **Submit Issue** - Create new GitHub Issue with detailed problem description
5. 📝 **Provide Information** - Include detailed error logs and reproduction steps

### 🌟 Community Contribution

We welcome contributions to the project!

- **Code Contribution**: Submit Pull Request to improve features or fix bugs
- **Documentation Translation**: Help translate documentation to other languages
- **Testing Feedback**: Test on different system environments and provide feedback
- **Sharing**: Recommend this project to friends who need it

---

## Disclaimer

This project is provided for learning research and convenient management of Warp.dev accounts only. Use at your own risk.

---

## System Requirements & Compatibility

### Supported Operating Systems
- **Windows** 10/11 (x64)
  - Supports automatic certificate installation
  - Supports automatic system proxy configuration
  - Supports Debug mode independent console
- **macOS** 10.15+ (Catalina and newer versions)
  - Supports automatic certificate installation and trust detection
  - Provides start-macos.sh one-click startup script
  - Supports Homebrew dependency installation
- **Linux** (Ubuntu/Debian/CentOS etc.)
  - Basic functionality support (requires manual system proxy configuration)
  - Certificate installation requires manual completion

### Minimum Requirements
- **Python Version**: 3.8 or higher (recommended 3.9+)
- **RAM**: 512 MB free memory
- **Storage Space**: 50 MB for program files + dependencies
- **Network**: Stable internet connection
- **Browser**:
  - **Full Functionality**: Chrome 88+, Edge 88+, Brave, Opera and other Chromium-based browsers
  - **Proxy Only**: Any browser (Firefox, Safari, etc.)

### Python Dependencies
```
PyQt5>=5.15.0        # GUI framework
requests>=2.25.0     # HTTP request library
mitmproxy>=8.0.0     # HTTPS proxy and interception
psutil>=5.8.0        # System process management
```

---

## License

This project is open source under MIT License. See LICENSE file for details.

---

# 中文

> **快速概览**:
> - **用途**: 自动创建和管理多个Warp.dev账户，突破单账户使用限制
> - **核心功能**: 批量账户创建、临时邮箱集成、代理支持、账户健康监控
> - **组件构成**: 图形界面应用 + 批量注册系统 + 临时邮箱管理器 + Chrome扩展 + 代理管理
> - **主要工作流**: 配置代理 → 批量创建账户 → 监控账户健康 → 使用Chrome扩展收集现有账户
> - **高级特性**: 并发处理、智能重试逻辑、代理轮换、自动邮箱验证

## 语言设置与检测

- 自动检测规则:
  - 土耳其/阿塞拜疆语系统 → 土耳其语界面 (tr)
  - 中文系统 → 默认英文界面 (en)  
  - 其他系统 → 默认英文 (en)
- 查看当前检测结果:
  - 语言检测现在基于系统设置自动完成
  - 界面将自动显示为相应的语言

## macOS 支持

- macOS用户可以使用Python 3运行应用程序:
  ```bash
  python3 warp_account_manager.py
  ```
- 确保安装了所需的依赖项:
  ```bash
  pip3 install -r requirements.txt
  ```
- 首次运行时会自动生成mitmproxy根证书并尝试安装/信任；如果自动安装失败，应用程序将引导您进行手动信任设置。

## 最近更新亮点

- **跨平台支持**: 完全兼容Windows/macOS/Linux，自动系统检测
- **证书自动化**: 首次启动时自动创建mitmproxy证书并尝试自动安装(Windows/macOS)，失败时提供手动指导对话框
- **增强代理管理**: 清晰的诊断和针对端口冲突/缺少依赖/权限问题的解决方案建议
- **语言检测与本地化**: 基于系统区域设置自动选择界面语言
- **文档与资源**: 全面的多语言README，包含界面截图
- **性能优化**: 并发批量刷新(多线程)，在10个账户场景下减少总体时间约60-70%
- **代码优化**: 精简代码库，专注于核心功能

---

## 这是什么（简明解释）

Warp账户管理器是一个强大的自动化工具，用于**创建和管理多个Warp.dev账户**。

**核心功能:**
- **自动账户创建**: 使用临时邮箱服务自动创建Warp.dev账户
- **批量处理**: 使用并发处理同时创建数十个账户
- **代理集成**: 使用代理轮换避免账户创建过程中的IP限制
- **邮箱验证**: 自动处理新账户的邮箱验证过程
- **账户管理**: 存储和管理已创建的账户，监控健康状态
- **浏览器集成**: Chrome扩展，从现有浏览器会话中收集账户数据

**为什么需要它:**
- Warp.dev对每个账户有使用配额限制 - 多个账户可以实现持续使用
- 手动创建账户耗时 - 自动化节省大量工作时间
- 代理轮换有助于在批量操作期间避免速率限制和IP封锁
- 有组织的账户管理防止丢失多个凭证

---

## 你将获得什么

- **图形界面应用程序** (Windows/macOS/Linux): 直观的批量账户创建和管理界面
- **批量注册系统**: 自动创建10、50或100+个账户，可配置并发数
- **临时邮箱集成**: 使用tmailor.com等服务实现自动邮箱验证
- **代理管理**: 加载并轮换代理列表，实现分布式账户创建
- **Chrome扩展**: 在Warp.dev页面上提供"添加到Warp管理器"按钮，用于收集现有账户数据
- **数据库存储**: SQLite数据库，用于有组织的账户存储和健康跟踪
- **高级功能**: 智能重试逻辑、进度监控、错误处理和性能优化

### 应用程序界面截图

#### 英文界面
![英文界面](img/en.png)
*英文界面: 显示账户管理、代理控制和状态监控*

#### 土耳其语界面  
![土耳其语界面](img/tr.jpg)
*土耳其语界面: 多语言支持，适应不同地区用户*

---

## 安装步骤（逐步指导）

### 1. 安装依赖项

1) **安装Python** (推荐3.8+)
2) **安装所需软件包:**
```bash
pip install -r requirements.txt
```

**核心依赖项:**
- `PyQt5` - 桌面应用程序的GUI框架
- `requests` - 用于API通信的HTTP请求
- `curl_cffi` - 用于绕过反机器人保护的高级HTTP客户端
- `mitmproxy` - 用于高级账户管理的HTTPS代理
- `psutil` - 系统进程监控
- `aiohttp` - 用于并发操作的异步HTTP客户端

3) **可选但推荐:**
```bash
pip install curl_cffi
```
提高临时邮箱服务和反机器人保护的成功率。

### 2. 设置代理列表（推荐）

在项目目录中创建一个`proxy.txt`文件，包含您的代理列表:
```
http://user:pass@proxy1.example.com:8080
https://user:pass@proxy2.example.com:3128
socks5://user:pass@proxy3.example.com:1080
proxy4.example.com:8080
```

**支持的代理格式:**
- `http://user:pass@host:port`
- `https://user:pass@host:port`
- `socks5://user:pass@host:port`
- `host:port` (无认证，假定为HTTP)

### 3. 启动应用程序

**Windows:**
```bash
python warp_account_manager.py
```

**macOS:**
```bash
python3 warp_account_manager.py
```

**Linux:**
```bash
python3 warp_account_manager.py
```

### 3.3 安装并信任mitmproxy证书（关键）
- 首次启动时，应用程序将提示安装mitmproxy根证书。
- 目的: 让系统信任代理的"中间人"HTTPS解密，仅用于本地机器和Warp域。
- 操作:
  1. 点击"打开证书文件"按钮，找到`mitmproxy-ca-cert.cer`。
  2. 按照应用程序中的步骤提示将其导入到"受信任的根证书颁发机构"。
  3. 完成后，应用程序状态将显示证书已就绪。

注意: 如果没有安装证书，代理将无法正确解密和注入令牌，功能将异常。

### 3.4 安装浏览器扩展

#### Chrome安装:
1) 打开Chrome并在地址栏输入`chrome://extensions/`
2) 在右上角启用"开发者模式"
3) 点击"加载已解压的扩展程序"，选择项目中的`chrome-extension`目录

#### Microsoft Edge安装:
1) 打开Edge并在地址栏输入`edge://extensions/`
2) 在左下角启用"开发者模式"
3) 点击"加载已解压的扩展"，选择项目中的`chrome-extension`目录

📝 **注意**: 安装扩展后无需设置权限，它将自动在Warp.dev页面上工作

---

## 使用工作流程

### 方法1: 自动批量创建（推荐）

1) **设置代理**: 将代理列表添加到`proxy.txt`文件
2) **启动应用程序**: 运行`python warp_account_manager.py`
3) **配置批量设置**:
   - 选择要创建的账户数量（例如，50个）
   - 选择快速模式以加快处理速度
   - 设置并发工作线程数（默认: 15）
4) **开始批量创建**: 点击"开始批量注册"
5) **监控进度**: 观察实时进度和成功/失败统计信息
6) **查看结果**: 在主界面中检查已创建的账户

### 方法2: Chrome扩展收集

1) **安装扩展**: 在Chrome开发者模式下加载`chrome-extension`文件夹
2) **访问Warp.dev**: 前往`https://app.warp.dev/logged_in/remote`
3) **收集账户**: 点击出现的"添加到Warp管理器"按钮
4) **自动导入**: 账户数据自动保存到本地数据库

### 账户管理功能

- **健康监控**: 跟踪哪些账户处于活动/封禁/过期状态
- **批量操作**: 删除、导出或刷新多个账户
- **使用统计**: 监控账户创建成功率
- **数据库管理**: 具有完整CRUD操作的SQLite数据库

---

## 性能优化概述（并发批量刷新）

- 默认的"刷新限制"操作启用了并发处理（2-5个线程，根据账户数量自动调整）。
- 单个账户失败不会影响其他账户，有60秒超时保护和轻量级速率控制（0.1-0.5秒随机延迟）。
- 可选自定义: 在根目录创建user_config.json以覆盖并发和延迟参数，例如:

```json
{
  "concurrent": {
    "max_workers_small": 2,
    "max_workers_medium": 3, 
    "max_workers_large": 5,
    "api_delay_min": 0.1,
    "api_delay_max": 0.5
  }
}
```

> 提示: 过高的并发可能触发服务器速率限制或本地资源瓶颈，建议在观察的同时逐渐增加。


---

## 联系与支持

### 📞 联系方式

| 联系方式 | 链接/信息 | 用途 |
|---|---|---|
| 🌐 **GitHub仓库** | [warp.dev_account_manager](https://github.com/hj01857655/warp.dev_account_manager) | 源代码、发布版本、错误报告 |
| 📱 **Telegram频道** | [@warp5215](https://t.me/warp5215) | 实时聊天、提示、反馈 |
| 💬 **QQ群** | [WARP工具社区](https://qm.qq.com/q/2zXp6ziTnC) | 中文用户社区 |
| 🐞 **错误报告** | [GitHub Issues](https://github.com/hj01857655/warp.dev_account_manager/issues) | 报告问题、功能请求 |
| 📚 **文档** | [Wiki页面](https://github.com/hj01857655/warp.dev_account_manager/wiki) | 详细教程、常见问题 |

### 🆘 获取帮助

**当您遇到问题时，请按以下顺序操作:**

1. 📖 **阅读文档** - 首先查看本README的故障排除部分
2. 🔍 **搜索历史** - 在GitHub Issues中查找类似问题
3. 💭 **社区讨论** - 在Telegram频道或QQ群中询问
4. 🐛 **提交问题** - 创建带有详细问题描述的新GitHub Issue
5. 📝 **提供信息** - 包括详细的错误日志和复现步骤

### 🌟 社区贡献

我们欢迎对项目的贡献!

- **代码贡献**: 提交Pull Request改进功能或修复错误
- **文档翻译**: 帮助将文档翻译成其他语言
- **测试反馈**: 在不同的系统环境上测试并提供反馈
- **分享**: 将此项目推荐给需要它的朋友

---

## 免责声明

本项目仅供学习研究和方便管理Warp.dev账户使用。使用风险自负。

---

## 系统要求与兼容性

### 支持的操作系统
- **Windows** 10/11 (x64)
  - 支持自动证书安装
  - 支持自动系统代理配置
  - 支持Debug模式独立控制台
- **macOS** 10.15+ (Catalina及更新版本)
  - 支持自动证书安装和信任检测
  - 提供start-macos.sh一键启动脚本
  - 支持Homebrew依赖安装
- **Linux** (Ubuntu/Debian/CentOS等)
  - 基本功能支持（需要手动系统代理配置）
  - 证书安装需要手动完成

### 最低要求
- **Python版本**: 3.8或更高 (推荐3.9+)
- **内存**: 512 MB可用内存
- **存储空间**: 50 MB用于程序文件+依赖项
- **网络**: 稳定的互联网连接
- **浏览器**:
  - **完整功能**: Chrome 88+, Edge 88+, Brave, Opera和其他基于Chromium的浏览器
  - **仅代理**: 任何浏览器 (Firefox, Safari等)

### Python依赖项
```
PyQt5>=5.15.0        # GUI框架
requests>=2.25.0     # HTTP请求库
mitmproxy>=8.0.0     # HTTPS代理和拦截
psutil>=5.8.0        # 系统进程管理
```

---

## 许可证

本项目在MIT许可证下开源。详见LICENSE文件。
