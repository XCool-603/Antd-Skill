---
name: antdui
description: >-
  AntdUI WinForms UI 组件库的专家技能。当用户需要使用 AntdUI（基于 Ant Design 的 WinForms 界面库）开发桌面应用程序时激活此技能。
  包括：安装配置、控件使用、主题定制、表单布局、事件处理、SVG 图标、DPI 适配、AOT 发布等全部功能。
  Use this skill when the user asks about AntdUI controls, WinForms UI with Ant Design style,
  building desktop applications, configuring themes, using SVG icons, or publishing AOT with AntdUI library.
---

# AntdUI WinForms 界面库

基于 [Ant Design](https://ant-design.antgroup.com/components/overview-cn) 设计语言的 WinForm UI 界面库，采用纯 GDI 矢量绘图，不需要图片资源，全面支持 AOT，最低兼容 `.NET Framework 4.0`。

- **官方仓库**：https://github.com/AntdUI/AntdUI
- **NuGet 包**：`AntdUI`
- **协议**：Apache 2.0
- **QQ群**：328884096

---

## 🚀 快速开始

### 安装

```powershell
# NuGet PM 命令
Install-Package AntdUI

# dotnet CLI
dotnet add package AntdUI
```

### Program.cs 基础配置

```csharp
[STAThread]
static void Main()
{
    // ① DPI 适配（.NET 6+ 推荐）
    Application.SetHighDpiMode(HighDpiMode.PerMonitorV2);
    Application.EnableVisualStyles();
    Application.SetCompatibleTextRenderingDefault(false);

    // ② 颜色模式（可选，默认 Light）
    AntdUI.Config.Mode = AntdUI.TMode.Light;

    // ③ 全局主题色（可选）
    AntdUI.Config.Theme()
        .Light("#ffffff", "#000000")
        .Dark("#141414", "#ffffff")
        .Header("#f5f5f5", "#1f1f1f");

    Application.Run(new MainForm());
}
```

---

## 📐 控件分类总览

| 分类 | 控件 |
|:---|:---|
| **通用** (2) | Button, FloatButton |
| **布局** (5) | Divider, StackPanel, FlowPanel, GridPanel, Splitter |
| **导航** (7) | Breadcrumb, Dropdown, Menu, PageHeader, TabHeader, Pagination, Steps |
| **数据录入** (16) | Checkbox, ColorPicker, DatePicker, DatePickerRange, Input, InputNumber, Radio, Rate, Select, Slider, Switch, TimePicker, Upload, InputTag, Transfer, CheckboxGroup |
| **数据展示** (15) | Avatar, Badge, Calendar, Carousel, Cell, Label, Image, Progress, QRCode, Segmented, Table, Tag, Timeline, Tooltip, Tree |
| **反馈** (8) | Alert, Drawer, Message, Modal, Notification, Popover, Spin, Skeleton |
| **窗体/其他** | BaseForm, WindowBar, Panel, Tabs, SVGIcon |

详细属性参见 → [references/controls.md](./references/controls.md)

---

## 🎨 主题与全局配置

### 颜色模式切换

```csharp
// 设置模式
AntdUI.Config.Mode = AntdUI.TMode.Light;   // 浅色
AntdUI.Config.Mode = AntdUI.TMode.Dark;    // 深色
AntdUI.Config.Mode = AntdUI.TMode.Auto;    // 跟随系统

// 运行时读取
bool isLight = AntdUI.Config.IsLight;
bool isDark  = AntdUI.Config.IsDark;
```

### 主题色 / 颜色定制

```csharp
// 链式配置：继承 BaseForm 的窗口会自动响应切换
AntdUI.Config.Theme()
    .Light("#ffffff", "#000000")         // 浅色 背景 | 前景
    .Dark("#141414", "#ffffff")          // 深色 背景 | 前景
    .Header("#f5f5f5", "#1f1f1f");       // PageHeader/WindowBar 背景
```

### 动画控制

```csharp
AntdUI.Config.Animation = false;  // 全局关闭动画（默认开启）
```

### DPI 适配

```csharp
// 全局（推荐在 Main 之前）
Application.SetHighDpiMode(HighDpiMode.PerMonitorV2);

// 也可通过 BaseForm.AutoHandDpi = true（默认已开启）
```

---

## 🪟 窗体（Form）

### 继承 BaseForm（推荐）

```csharp
using AntdUI;

public partial class MainForm : BaseForm
{
    public MainForm()
    {
        InitializeComponent();
    }
}
```

`BaseForm` 提供：无边框原生窗口、自动 DPI 适配、主题自动切换、阴影效果。

| 属性 | 说明 | 默认值 |
|:---|:---|:---|
| `AutoHandDpi` | 自动处理 DPI | true |
| `Dark` | 深色模式 | false |
| `Mode` | 颜色模式 | Auto |
| `IsMax` | 是否最大化 | false |
| `IsFull` | 是否全屏 | false |
| `DisableTheme` | 是否禁用主题 | false |

### WindowBar 标题栏控件

在不继承 `BaseForm` 时，单独添加标题栏：

```csharp
var windowBar = new AntdUI.WindowBar
{
    Dock = DockStyle.Top,
    ShowIcon = true,
    CloseSize = 46,
};
this.Controls.Add(windowBar);
```

---

## 🧩 核心控件用法

### Button 按钮

```csharp
var btn = new AntdUI.Button
{
    Text = "提交",
    Type = AntdUI.TTypeMini.Primary,  // Default/Primary/Success/Warning/Error
    Shape = AntdUI.TShape.Default,    // Default/Round/Circle
    Radius = 6,
    WaveSize = 4,                     // 点击波纹
    Ghost = false,                    // 幽灵按钮（透明背景）
    Loading = false,                  // 加载状态
    ImageSvg = AntdUI.SvgDb.TagsFill, // SVG 图标
};
btn.Click += (s, e) => AntdUI.Message.success(this, "提交成功");
```

### Input 输入框

```csharp
// 普通输入框
var input = new AntdUI.Input
{
    PlaceholderText = "请输入用户名",
    AllowClear = true,
    Radius = 6,
    Status = AntdUI.TType.None,        // 验证状态：None/Success/Warn/Error
    Variant = AntdUI.TVariant.Outlined, // Outlined/Borderless/Filled/Underline
};

// 密码框
var pwd = new AntdUI.Input
{
    PlaceholderText = "请输入密码",
    UseSystemPasswordChar = true,      // 或 PasswordChar = '●'
    PasswordCopy = false,
};

// 多行文本
var memo = new AntdUI.Input
{
    Multiline = true,
    WordWrap = true,
    Height = 120,
};

input.TextChanged += (s, e) => Console.WriteLine(input.Text);
```

### Select 下拉选择

```csharp
var select = new AntdUI.Select
{
    PlaceholderText = "请选择",
    AllowClear = true,
    MaxCount = 6,                         // 下拉最多显示行数
    Placement = AntdUI.TAlignFrom.BL,     // 弹出位置
};
select.Items.Add(new AntdUI.SelectItem("选项一", "1"));
select.Items.Add(new AntdUI.SelectItem("选项二", "2"));
select.Items.Add(new AntdUI.SelectItem { Text = "分组", Sub = true }); // 分组标题
select.SelectedIndexChanged += (s, e) => Console.WriteLine(select.Value);
```

### Table 表格

```csharp
var table = new AntdUI.Table
{
    Dock = DockStyle.Fill,
    Bordered = true,
    FixedHeader = true,
    VirtualMode = false,  // 大数据时开启虚拟模式
    EditMode = AntdUI.TEditMode.None,
};

// 定义列
table.Columns = new AntdUI.ColumnCollection
{
    new AntdUI.Column("Id", "ID") { Width = 60 },
    new AntdUI.Column("Name", "姓名") { Width = 150 },
    new AntdUI.Column("Status", "状态") { Width = 100 },
    // 自定义渲染列
    new AntdUI.Column("Action", "操作")
    {
        Width = 120,
        Fixed = true, // 固定列
    },
};

// 绑定数据（支持 List<T>、DataTable、IEnumerable）
table.DataSource = myDataList;

// 事件
table.CellClick += (s, e) =>
{
    Console.WriteLine($"点击了第{e.RowIndex}行 第{e.ColumnIndex}列");
};
table.RowDoubleClick += (s, e) =>
{
    // e.Record 为当前行对象
};
```

### Modal 对话框

```csharp
// ① 简单提示
AntdUI.Modal.open(new AntdUI.Modal.Config(this, "提示", "操作已完成！"));

// ② 确认框（同步）
var result = AntdUI.Modal.open(new AntdUI.Modal.Config(this, "确认删除", "此操作不可恢复，确认吗？")
{
    OkType = AntdUI.TTypeMini.Error,
    OkText = "删除",
    CancelText = "取消",
    Keyboard = true,       // 支持 ESC 关闭
    MaskClosable = true,   // 点击蒙层关闭
    Width = 420,
    CloseIcon = true,
});
if (result == DialogResult.OK) DoDelete();

// ③ 异步等待
var ans = await AntdUI.Modal.openAsync(
    new AntdUI.Modal.Config(this, "标题", "内容")
);

// ④ 自定义控件内容
var panel = new Panel { /* 自定义布局 */ };
AntdUI.Modal.open(new AntdUI.Modal.Config(this, "自定义", panel)
{
    Width = 600,
    ContentPadding = new Size(0, 0),
});
```

### Message 全局消息

```csharp
AntdUI.Message.info(this, "普通提示");
AntdUI.Message.success(this, "操作成功");
AntdUI.Message.warn(this, "警告信息");
AntdUI.Message.error(this, "发生错误");

// 带 loading，返回 key 可手动关闭
string key = AntdUI.Message.loading(this, "加载中...");
// ... 异步操作完成后
AntdUI.Message.close(key);
```

### Notification 通知

```csharp
AntdUI.Notification.open(new AntdUI.Notification.Config(this, "上传成功", "文件已上传至服务器")
{
    Icon = AntdUI.TType.Success,
    Placement = AntdUI.TAlignFrom.TR,  // TR=右上 TL=左上 BR=右下 BL=左下
    Duration = 4500,                   // ms，0=不自动关闭
});
```

### Drawer 抽屉

```csharp
// 打开右侧抽屉
AntdUI.Drawer.open(new AntdUI.Drawer.Config(this, contentPanel)
{
    Align = AntdUI.TAlignMini.Right,  // Top/Bottom/Left/Right
    Mask = true,
    MaskClosable = true,
    Padding = 24,
    Dispose = true,  // 关闭后释放内容控件
});
```

### Menu 导航菜单

```csharp
var menu = new AntdUI.Menu
{
    Dock = DockStyle.Left,
    Width = 220,
    Collapsed = false,               // 折叠状态
    Unique = true,                   // 只保持一个子菜单展开
    Trigger = AntdUI.Trigger.Click,  // Click/Hover
    Indent = true,
};

// 添加菜单项
menu.Items.Add(new AntdUI.MenuItem("首页") { ImageSvg = AntdUI.SvgDb.HomeOutlined });
var subMenu = new AntdUI.MenuItem("系统管理") { ImageSvg = AntdUI.SvgDb.SettingOutlined };
subMenu.Sub.Add(new AntdUI.MenuItem("用户管理"));
subMenu.Sub.Add(new AntdUI.MenuItem("权限管理"));
menu.Items.Add(subMenu);

menu.SelectChanged += (s, e) => Console.WriteLine($"选中: {e.Item.Text}");
```

### Tree 树形控件

```csharp
var tree = new AntdUI.Tree
{
    Dock = DockStyle.Left,
    Width = 200,
    Checkable = true,       // 显示复选框
    Multiple = false,       // 多选
    Draggable = true,       // 可拖拽
    VirtualMode = false,    // 虚拟模式（大数据）
};

var root = new AntdUI.TreeItem("根节点") { Expand = true };
root.Add(new AntdUI.TreeItem("子节点1"));
var sub = new AntdUI.TreeItem("子节点2");
sub.Add(new AntdUI.TreeItem("孙节点"));
root.Add(sub);
tree.Items.Add(root);

tree.SelectChanged += (s, e) => Console.WriteLine(e.Item?.Text);
```

### Tabs 标签页

```csharp
var tabs = new AntdUI.Tabs
{
    Dock = DockStyle.Fill,
    Alignment = TabAlignment.Top,  // Top/Bottom/Left/Right
    Centered = false,
};

var page1 = new AntdUI.TabPage("首页");
page1.Controls.Add(new Label { Text = "首页内容" });

var page2 = new AntdUI.TabPage("设置");
page2.Controls.Add(new Label { Text = "设置内容" });

tabs.Pages.Add(page1);
tabs.Pages.Add(page2);

tabs.SelectedIndexChanged += (s, e) => Console.WriteLine($"切换到: {tabs.SelectedTab?.Text}");
```

---

## 🦜 SVG 图标

AntdUI 内置 Ant Design 图标集，通过 `AntdUI.SvgDb` 访问：

```csharp
// 按钮图标
button.ImageSvg = AntdUI.SvgDb.SearchOutlined;
button.ImageSvg = AntdUI.SvgDb.PlusOutlined;
button.ImageSvg = AntdUI.SvgDb.DeleteOutlined;
button.ImageSvg = AntdUI.SvgDb.EditOutlined;
button.ImageSvg = AntdUI.SvgDb.DownloadOutlined;

// MenuItem 图标
menuItem.ImageSvg = AntdUI.SvgDb.HomeOutlined;
menuItem.ImageSvg = AntdUI.SvgDb.SettingOutlined;
menuItem.ImageSvg = AntdUI.SvgDb.UserOutlined;

// 使用自定义 SVG 字符串
button.ImageSvg = "<svg>...</svg>";
```

常用图标速查（完整列表见 `AntdUI.SvgDb`）：

| 类别 | 图标名 |
|:---|:---|
| 方向 | `ArrowUpOutlined`, `ArrowDownOutlined`, `ArrowLeftOutlined`, `ArrowRightOutlined` |
| 操作 | `PlusOutlined`, `MinusOutlined`, `DeleteOutlined`, `EditOutlined`, `SearchOutlined` |
| 文件 | `FileOutlined`, `FolderOutlined`, `DownloadOutlined`, `UploadOutlined` |
| 用户 | `UserOutlined`, `TeamOutlined`, `LockOutlined`, `UnlockOutlined` |
| 系统 | `SettingOutlined`, `HomeOutlined`, `AppstoreOutlined`, `BellOutlined` |
| 状态 | `CheckCircleOutlined`, `CloseCircleOutlined`, `ExclamationCircleOutlined`, `InfoCircleOutlined` |

---

## 📋 枚举类型完整速查

| 枚举 | 可选值 | 说明 |
|:---|:---|:---|
| `TMode` | `Light`, `Dark`, `Auto` | 颜色模式 |
| `TTypeMini` | `Default`, `Primary`, `Success`, `Warning`, `Error`, `Info` | 控件类型 |
| `TType` | `None`, `Success`, `Info`, `Warn`, `Error` | 状态类型 |
| `TShape` | `Default`, `Round`, `Circle` | 形状 |
| `TShapeProgress` | `Default`, `Circle`, `Round`, `Mini`, `Steps` | Progress 形状 |
| `TAlign` | `Left`, `Center`, `Right` | 水平对齐 |
| `TAlignMini` | `Top`, `Bottom`, `Left`, `Right` | 四方向对齐 |
| `TAlignFrom` | `TL`, `TR`, `BL`, `BR` | 弹出位置（左上/右上/左下/右下）|
| `TFit` | `Fill`, `None`, `Contain`, `Cover` | 图片填充 |
| `TAutoSize` | `None`, `Auto`, `Width`, `Height` | 自动尺寸 |
| `TVariant` | `Outlined`, `Borderless`, `Filled`, `Underline` | Input 样式变体 |
| `TEditMode` | `None`, `Click`, `DoubleClick` | Table 编辑模式 |
| `TAMode` | `Auto`, `Light`, `Dark` | 颜色方案（Modal/Notification） |
| `Trigger` | `Click`, `Hover` | 触发方式 |
| `TMenuMode` | `Inline`, `Horizontal`, `Vertical` | Menu 模式 |
| `TFocusMode` | `None`, `Line`, `Dot` | Menu 焦点模式 |
| `ColumnsMode` | `Auto`, `None`, `Fill`, `Header` | Table 列宽模式 |

---

## 🌍 全球化（国际化）

```csharp
// 设置中文
AntdUI.Localization.Provider = new AntdUI.DefaultLocalizationProvider(
    new System.Globalization.CultureInfo("zh-CN")
);

// 控件支持 🌏 前缀的 Localization 属性：
input.LocalizationPlaceholderText = "input.placeholder";
label.LocalizationText = "label.title";
```

---

## 🦺 AOT 发布

```xml
<!-- .csproj 中添加 -->
<PropertyGroup>
  <PublishAot>true</PublishAot>
</PropertyGroup>
```

注意事项：
1. AntdUI 内部已全面支持 AOT，无需额外配置
2. `Table` 使用反射绑定时，实体类需标注 `[DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicProperties)]`
3. 或使用 `Column` 的 `GetValue` / `SetValue` 委托手动绑定，完全避免反射

---

## 💡 常见模式

### 异步加载数据到 Table

```csharp
private async void LoadData()
{
    // 显示 Spin
    AntdUI.Spin.open(this);
    try
    {
        var data = await FetchDataAsync();
        table.DataSource = data;
    }
    finally
    {
        AntdUI.Spin.close(this);
    }
}
```

### 表单验证

```csharp
bool Validate()
{
    bool ok = true;
    if (string.IsNullOrWhiteSpace(inputName.Text))
    {
        inputName.Status = AntdUI.TType.Error;
        ok = false;
    }
    else
    {
        inputName.Status = AntdUI.TType.None;
    }
    return ok;
}

btnSubmit.Click += (s, e) =>
{
    if (!Validate()) { AntdUI.Message.warn(this, "请完善表单信息"); return; }
    // 提交逻辑...
};
```

### 跨线程更新 UI

```csharp
// AntdUI 控件的属性赋值需要在 UI 线程
this.Invoke(() =>
{
    progress.Value = 80;
    label.Text = "处理完成";
});
```

---

## 📚 参考资料

- [控件完整属性参考](./references/controls.md) — 所有控件属性表格 + 代码示例
- [常用场景代码模板](./references/patterns.md) — 登录窗口、主布局、CRUD表格、表单验证等完整模板
- [官方英文文档](https://github.com/AntdUI/AntdUI/blob/main/doc/wiki/en/Home.md)
- [官方中文文档](https://github.com/AntdUI/AntdUI/blob/main/doc/wiki/zh/Home.md)
- [枚举类型完整参考](https://github.com/AntdUI/AntdUI/blob/main/doc/wiki/en/Control/Enum.md)
- [更新日志](https://github.com/AntdUI/AntdUI/blob/main/doc/wiki/en/UpdateLog.md)
- [演示项目](https://github.com/AntdUI/AntdUI-Demo)
