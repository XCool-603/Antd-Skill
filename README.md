# AntdUI Skill for Antigravity / AI Agent

这是一个为 **Google Antigravity / AI Agent** 定制打造的 **[AntdUI](https://github.com/AntdUI/AntdUI)** 专业技能（Skill）知识库。

> **AntdUI** 是一款基于 Ant Design 5.0 设计语言的现代 WinForms 界面库，使用纯 GDI 矢量绘图、无需图片资源、支持暗黑模式、高 DPI 自适应以及原生 AOT 发布。

---

## 📖 目录
- [🎯 这个 Skill 能帮你做什么？](#-这个-skill-能帮你做什么)
- [🚀 怎么在项目中使用该 Skill？](#-怎么在项目中使用该-skill)
  - [方式一：作为工作区技能使用（当前项目）](#方式一作为工作区技能使用当前项目推荐)
  - [方式二：复制到全局技能（所有项目通用）](#方式二复制到全局技能所有项目通用)
  - [方式三：安装到你自己的 WinForms 项目中](#方式三安装到你自己的-winforms-项目中)
- [💡 提示词（Prompt）提问示例](#-提示词prompt提问示例)
- [📂 技能仓库文件结构](#-技能仓库文件结构)
- [📚 文档导航](#-文档导航)

---

## 🎯 这个 Skill 能帮你做什么？

当你让 AI 编写 WinForms 代码时，AI 默认会输出传统 WinForms 控件（灰底、老旧样式、难以维护的绘图代码）。

安装或激活此 Skill 后，AI 会自动化身为 **AntdUI 专家**，为你生成：
1. **纯正 Ant Design 风格**的现代化 WinForms 界面代码（`BaseForm`、圆角、阴影、波纹动效）。
2. **完整参数化的 AntdUI 控件**（`Button`、`Input`、`Table`、`Menu`、`Tabs`、`Modal`、`Spin` 等）。
3. **内置 SVG 图标与暗黑模式切换**代码（无需引入外部图标资源）。
4. **企业级实战模板**（CRUD 页面、带验证表单、异步防抖搜索、文件上传进度条等）。
5. **AOT 编译兼容写法**（避免因反射引发的 AOT 裁剪异常）。

---

## 🚀 怎么在项目中使用该 Skill？

### 方式一：作为工作区技能使用（当前项目推荐）

该仓库根目录下的结构为：
```text
.agents/
└── skills/
    └── antdui/
        ├── SKILL.md
        └── references/
```

只要你在 Antigravity 中打开了包含 `.agents/skills/antdui` 的文件夹作为工作区，AI 在回答与 **AntdUI / WinForms UI / Ant Design Winform** 相关的问题时，就会**自动激活**该技能。

### 方式二：复制到全局技能（所有项目通用）

如果你希望在任何工作区中，AI 都能随时使用该技能，可以将 `antdui` 文件夹复制到你的 Antigravity 全局技能目录：

- **Windows 目录**：
  ```powershell
  $target = "$env:USERPROFILE\.gemini\antigravity\skills\antdui"
  Copy-Item -Recurse -Force ".agents\skills\antdui" $target
  ```
- 复制完成后，无论你在哪个目录与 AI 对话，AI 都能调用此技能。

### 方式三：安装到你自己的 WinForms 项目中

如果你要在新的或已有的 WinForms 工程里引入该 Skill：
1. 把本仓库的 `.agents` 文件夹直接复制到你的 WinForms 解决方案根目录下。
2. 提交到你的 Git 仓库即可与团队成员共享。

---

## 💡 提示词（Prompt）提问示例

你可以直接对 AI 提出如下需求，AI 会自动依据 Skill 知识库生成标准规范的 AntdUI 代码：

### 示例 1：创建现代化登录窗口
> *"帮我用 AntdUI 编写一个现代风格的登录窗口，要求继承 BaseForm，包含账号、密码输入框（带前缀图标）、记住密码、圆角主色登录按钮，并加入简单的空值验证和 Message 提示。"*

### 示例 2：实现数据表格与分页管理
> *"用 AntdUI 帮我写一个用户管理的 CRUD 界面，包含顶部搜索栏（带防抖）、操作按钮、Table 控件（定义列、自定义操作列按钮）以及底部的 Pagination 分页。"*

### 示例 3：切换深色模式（Dark Mode）
> *"如何在 AntdUI 中实现一键切换浅色/深色主题？请给我全局配置及 Switch 控件联动的示例代码。"*

### 示例 4：AOT 编译安全的数据绑定
> *"我想把基于 AntdUI 的 WinForms 项目发布为 AOT 单文件程序，Table 控件该怎么绑定数据才不会被剪裁？"*

---

## 📂 技能仓库文件结构

```text
Antd-Skill/
├── README.md                                  # 🏠 仓库首页与使用教程（本文档）
└── .agents/
    └── skills/
        └── antdui/
            ├── SKILL.md                       # ⚡ 技能主入口（快速参考、核心概念、速查表）
            └── references/
                ├── controls.md                # 📖 全控件属性清单（参数说明、类型、默认值与用例）
                └── patterns.md                # 🛠️ 10大实用场景完整可运行模板
```

---

## 📚 文档导航

- [查看技能主定义与速查 (SKILL.md)](.agents/skills/antdui/SKILL.md)
- [查看所有控件详细属性 (references/controls.md)](.agents/skills/antdui/references/controls.md)
- [查看企业级场景代码模板 (references/patterns.md)](.agents/skills/antdui/references/patterns.md)
- [AntdUI 官方源码仓库 (GitHub)](https://github.com/AntdUI/AntdUI)
