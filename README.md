# Vela9 🌌

**邀请你试用 Vela9，并告诉我它哪里好用、哪里不好用。**

Vela9 是一个在你自己电脑上运行的轻量 coding agent。它能阅读和搜索你指定的代码目录、解释项目结构、协助定位报错、修改文件，并在你批准后运行命令。它同时提供终端命令和本地桌面窗口。现在是 **0.1.0 Beta 试用版**，欢迎开发者、学生和任何愿意给出真实反馈的朋友一起试。

> 当前下载包仅支持 **Windows 64 位 + CPython 3.12**。项目代码在本机运行；选用在线模型时，相关请求会发送给你配置的模型服务。请只在自己拥有或获准处理的代码目录中使用。

## 下载

在 [Releases 页面](https://github.com/Stamina9/vela9/releases/tag/v0.1.0-beta.1) 下载：

- [`vela9-0.1.0b1-cp312-cp312-win_amd64.whl`](https://github.com/Stamina9/vela9/releases/download/v0.1.0-beta.1/vela9-0.1.0b1-cp312-cp312-win_amd64.whl)：安装包。
- [`SHA256SUMS`](https://github.com/Stamina9/vela9/releases/download/v0.1.0-beta.1/SHA256SUMS)：安装包的完整性校验值。

`.whl` 是 Python 安装包，使用 `pip` 安装即可，**不必手动解压**。这个版本还没有上传 PyPI，所以不要运行 `pip install vela9` 来猜测同名包。PyPI 是 Python Package Index，即 Python 软件包索引；你可以把它理解成 Python 包的公开下载站。本次安装请使用上面的 GitHub Release 文件。

## Windows 安装：复制这几条命令

1. 安装 **Python 3.12 64 位版**。打开 PowerShell，运行 `py -3.12 --version` 确认能找到 Python。
2. 把下载的 wheel 放在“下载”文件夹，打开 PowerShell 并进入该目录。如果下载位置不同，请把下面的路径换成实际位置。
3. 逐行运行：

```powershell
cd "$HOME\Downloads"
py -3.12 -m venv "$HOME\vela9-env"
& "$HOME\vela9-env\Scripts\python.exe" -m pip install .\vela9-0.1.0b1-cp312-cp312-win_amd64.whl
& "$HOME\vela9-env\Scripts\vela9.exe" --version
```

看到 `Vela9 0.1.0b1` 就安装成功了。以上方式把 Vela9 安装在专门的虚拟环境里，无需管理员权限，也无需修改 PowerShell 执行策略。wheel 不包含 Python、模型权重、Docker 或在线模型额度。

## 先用桌面窗口体验

先创建一个练习目录，避免一开始就让 agent 修改重要项目：

```powershell
New-Item -ItemType Directory -Force "$HOME\vela9-demo"
& "$HOME\vela9-env\Scripts\vela9.exe" gui --cwd "$HOME\vela9-demo"
```

在窗口里选择模型服务，填写**你自己**的 API Key，然后试试：

> 先看看这个目录里有什么。请只解释，不要修改文件，也不要运行命令。

准备处理真实项目时，把 `--cwd` 后面的目录换成自己的代码仓库。建议先提交 Git 或备份文件，再阅读每次工具操作的审批提示和最终变更。窗口中的 Key 只用于当前运行进程；会话与运行记录保存在所选工作目录的 `.pico/` 中。

### 选择模型

- **在线模型**：窗口支持 DeepSeek、OpenAI 兼容服务和 Anthropic 兼容服务。各服务的 Key、模型名称、费用和可用网络由你自己的服务商决定。处理私有代码前，先确认你愿意把相关内容发送给该服务商。
- **本地 Ollama**：先自行安装、启动 Ollama 并下载模型，然后在窗口里选择 `ollama`。无需在线模型 API Key；效果取决于本地模型和电脑性能。

## 终端常用命令

下面用 `$Vela` 缩短命令；每次打开新的 PowerShell 窗口都需要重新设置它：

```powershell
$Vela = "$HOME\vela9-env\Scripts\vela9.exe"
& $Vela --help
& $Vela doctor --cwd "$HOME\vela9-demo"
& $Vela --cwd "$HOME\vela9-demo"
& $Vela --cwd "$HOME\vela9-demo" "请先列出文件并说明用途，不要修改"
```

进入交互模式后输入 `/help` 查看会话命令，输入 `/exit` 退出。命令行使用在线模型时，可以在自己的项目根目录放置 `.env`，例如 `PICO_PROVIDER=deepseek` 与 `PICO_DEEPSEEK_API_KEY=你自己的密钥`；**不要把 `.env` 提交到 GitHub**。程序会向上查找最近的 `.env`，也要留意父目录中的旧配置。桌面窗口可直接临时输入 Key，初次体验更方便。

安装了 Docker 的用户可以先执行 `docker pull python:3.12-slim`，再运行 `& $Vela --cwd "$HOME\vela9-demo" --sandbox docker`。该选项仅隔离 shell 命令，文件工具仍能修改所选目录；容器默认无网络。普通本地试用不需要 Docker。

## 核对下载文件

在 wheel 和 `SHA256SUMS` 所在目录执行：

```powershell
Get-Content .\SHA256SUMS
Get-FileHash .\vela9-0.1.0b1-cp312-cp312-win_amd64.whl -Algorithm SHA256
```

两个哈希值应完全相同。不一致时停止安装，重新从本仓库的 Release 页面下载。

## 特别想听到你的反馈 🙌

如果 Vela9 帮你理解了一个仓库、定位了错误，或者在哪一步卡住了，都欢迎告诉我。**小问题、体验建议和失败案例都很有用。**

- 在 [GitHub Issues](https://github.com/Stamina9/vela9/issues) 提交问题或建议。
- 邮箱：[1934687883@qq.com](mailto:1934687883@qq.com)。
- 微信：扫描下方二维码添加 **Stamina**，备注“Vela9”。

![作者微信二维码](wechat.jpg)

反馈时如果方便，请附上 Windows 版本、Python 版本、模型服务、你输入的任务、预期结果和实际结果。贴日志或截图前，请先删掉 API Key、个人文件内容和其他敏感信息。你最希望它下一版解决什么真实问题，也欢迎直接提出。

## 关于发布形式

本仓库放置使用说明、联系二维码、许可证和 Release 安装文件，**不放 Pico/Vela9 的实现源码**。Beta wheel 的实现模块已编译为 Windows 扩展；编译发布并不意味着绝对无法逆向分析。这个版本仍在试用阶段，欢迎一起发现问题、推动下一版改进。

许可：MIT，见 [LICENSE](LICENSE)。
