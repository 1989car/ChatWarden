# CLAUDE.md

此文件为Claude Code (claude.ai/code) 提供在此代码库中工作的指导。

## 项目概述

这是一个 **World of Warcraft (WoW) 插件项目**，名为 **ChatWarden**（聊天守卫）。插件旨在保护聊天环境免受垃圾信息和骚扰。

- **技术栈**: Lua 5.1 + Ace3 框架
- **插件类型**: WoW 界面插件 (Interface Addon)
- **接口版本**: 38000 (对应WoW 3.8.0版本)
- **许可证**: MIT

## 项目结构

```
ChatWarden/
├── ChatWarden.toc          # 插件主描述文件 (必需)
├── LICENSE.txt            # MIT许可证
├── .gitignore             # 全面的Git忽略配置
├── .claude/               # Claude Code本地设置
└── Libs/                  # Ace3框架依赖库
    ├── Ace3.lua           # Ace3主文件
    ├── Ace3.toc           # Ace3描述文件
    ├── README.md          # Ace3文档
    └── 多个Ace3库子目录 (AceAddon-3.0, AceEvent-3.0等)
```

## 开发环境设置

### 1. WoW插件开发环境

- **WoW客户端**: 需要安装World of Warcraft客户端
- **插件目录**:
  - **Windows**: `C:\Program Files (x86)\World of Warcraft\classi_titan\Interface\AddOns\`
  - **Linux/macOS**: `~/Games/World of Warcraft/classi_titan/Interface/AddOns/`
- **开发流程**: 推荐使用硬链接方式（见下方构建流程），这样可以在开发目录修改代码时自动同步到游戏插件目录

### 2. Lua开发工具

- **编辑器推荐**:
  - **VS Code** + Lua扩展（推荐）
  - **Notepad++** + Lua插件（Windows环境）
  - **Sublime Text** + Lua插件
- **调试工具**:
  - 使用WoW内置的`/script`命令输出调试信息
  - **BugSack**插件捕获错误
  - **!BugGrabber**插件收集错误信息
- **代码检查**: 暂无配置LuaLint等工具，建议手动代码审查

### 3. 项目状态

当前项目处于**早期开发阶段**，只有基础框架文件：

- ✅ 已包含完整的Ace3框架
- ✅ 已配置插件元数据 (.toc文件)
- ❌ 缺少主插件逻辑代码 (ChatWarden.lua)
- ❌ 缺少配置界面和事件处理

## 构建和测试

### 构建流程

WoW插件无需传统构建过程，推荐使用**硬链接**方式，这样可以在开发目录修改代码时自动同步到游戏插件目录：

#### Linux/macOS 环境

```bash
# 创建硬链接到WoW插件目录
ln -s "$(pwd)" ~/Games/World\ of\ Warcraft/classi_titan/Interface/AddOns/ChatWarden
```

#### Windows 环境 (Windows 11)

```powershell
# 使用mklink创建目录连接（需要管理员权限）
mklink /J "C:\Program Files (x86)\World of Warcraft\classi_titan\Interface\AddOns\ChatWarden" "C:\path\to\your\ChatWarden\project"

# 或者使用符号链接（不需要管理员权限）
mklink /D "C:\Program Files (x86)\World of Warcraft\classi_titan\Interface\AddOns\ChatWarden" "C:\path\to\your\ChatWarden\project"
```

#### 传统复制方式（不推荐）

```bash
# 将插件复制到WoW插件目录（每次修改都需要重新复制）
cp -r ChatWarden/ ~/Games/World\ of\ Warcraft\classi_titan/Interface/AddOns/
```

### 测试方法

1. **启动WoW客户端**
2. **登录游戏**
3. **检查插件加载**:
   - 在角色选择界面点击"插件"按钮
   - 确保ChatWarden已启用
4. **游戏内测试**:
   - 使用`/chatwarden`命令测试插件功能
   - 检查保存变量是否正确存储

### 调试技巧

- 使用`/script print("调试信息")`输出调试信息
- 查看`ChatWardenDB.lua`文件检查保存变量
- 使用BugSack插件捕获错误

## Ace3框架使用指南

### 核心库

- **AceAddon-3.0**: 插件基础框架
- **AceEvent-3.0**: 事件处理系统
- **AceDB-3.0**: 数据存储管理
- **AceConsole-3.0**: 斜杠命令处理
- **AceConfig-3.0**: 配置界面生成
- **AceGUI-3.0**: 图形用户界面组件

### 基本插件结构

```lua
-- ChatWarden.lua 示例结构
local ChatWarden = LibStub("AceAddon-3.0"):NewAddon("ChatWarden", "AceConsole-3.0", "AceEvent-3.0")

function ChatWarden:OnInitialize()
    -- 初始化代码
    self.db = LibStub("AceDB-3.0"):New("ChatWardenDB", defaults)

    -- 注册斜杠命令
    self:RegisterChatCommand("chatwarden", "SlashCommand")
    self:RegisterChatCommand("cw", "SlashCommand")
end

function ChatWarden:SlashCommand(input)
    -- 处理斜杠命令
end
```

## 开发工作流程

### 1. 创建主插件文件

需要创建 `ChatWarden.lua` 文件，包含：

- 插件初始化逻辑
- 事件处理函数
- 配置管理
- 聊天监控逻辑

### 2. 添加配置文件

建议创建 `ChatWarden_Config.lua` 用于：

- 默认配置设置
- 用户选项管理
- 本地化字符串

### 3. 实现核心功能

根据插件描述，需要实现：

- 聊天消息监控
- 垃圾信息检测算法
- 用户屏蔽功能
- 配置界面

### 4. 测试和迭代

- 在游戏中测试每个功能
- 收集用户反馈
- 优化检测算法

## 文件命名规范

- **主文件**: `ChatWarden.lua`
- **配置**: `ChatWarden_Config.lua`
- **本地化**: `ChatWarden_Locale.lua` 或按语言分文件
- **库文件**: 放在 `Libs/` 目录下
- **XML界面**: `ChatWarden.xml` (如果需要自定义UI)

## Git工作流程

### 忽略的文件

根据 `.gitignore` 配置，以下文件不应提交：

- 用户数据文件 (`ChatWardenDB.lua`)
- 编辑器临时文件
- 操作系统特定文件
- 备份文件

### 提交规范

- 使用中文或英文提交信息
- 描述功能变更而非文件列表
- 保持提交的原子性

## 注意事项

1. **Lua版本**: WoW使用Lua 5.1，不支持5.2+的新特性
2. **性能考虑**: 插件代码应高效，避免影响游戏性能
3. **内存管理**: 注意内存泄漏，及时清理不需要的数据
4. **兼容性**: 确保与常见插件的兼容性
5. **本地化**: 支持多语言，使用AceLocale库
6. **Windows环境**:
   - 使用`mklink`创建链接时需要**管理员权限**
   - 路径中的空格需要使用引号或转义
   - 建议在PowerShell中执行链接创建命令
7. **文件编码**: 确保Lua文件使用UTF-8编码，避免中文乱码
8. **路径分隔符**: Windows使用反斜杠`\`，Linux/macOS使用正斜杠`/`

## 资源链接

- **Ace3文档**: [项目内的Libs/README.md](Libs/README.md)
- **WoW API文档**: <https://wowpedia.fandom.com/wiki/World_of_Warcraft_API>
- **Lua 5.1参考**: <https://www.lua.org/manual/5.1/>
- **WoW界面开发社区**: <https://www.wowinterface.com/>

## 下一步开发建议

1. 创建主插件文件 `ChatWarden.lua`
2. 实现基本的插件框架和事件处理
3. 添加配置选项和用户界面
4. 开发聊天监控核心逻辑
5. 进行游戏内测试和优化
