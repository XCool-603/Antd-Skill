# AntdUI 自适应与响应式布局完全指南

在 WinForms 开发中，“自适应”包含两大关键维度：
1. **DPI 硬件自适应**：高分屏（1080P/2K/4K）、系统缩放（125%、150%、200%）、多显示器不同 DPI 热插拔/拖拽不模糊、文字不发虚、组件不重叠。
2. **窗口与屏幕尺寸自适应**：用户随意拉伸窗口、最大化、最小化、分屏或在不同分辨率屏幕上运行时，界面元素自动伸缩、按比例分布、流式换行，杜绝空白溢出或内容被截断。

---

## 目录
- [一、强制执行的自适应设计铁律](#一强制执行的自适应设计铁律)
- [二、高 DPI 硬件级自适应配置](#二高-dpi-硬件级自适应配置)
- [三、窗口与分辨率自适应机制](#三窗口与分辨率自适应机制)
- [四、AntdUI 专用自适应容器选型与实践](#四antdui-专用自适应容器选型与实践)
- [五、Table 表格列宽与数据自适应](#五table-表格列宽与数据自适应)
- [六、响应式断点（Responsive Breakpoints）](#六响应式断点responsive-breakpoints)
- [七、弹窗与抽屉（Modal & Drawer）自适应](#七弹窗与抽屉modal--drawer自适应)
- [八、完整自适应响应式界面实战代码](#八完整自适应响应式界面实战代码)

---

## 一、强制执行的自适应设计铁律

在编写任何 AntdUI 界面代码时，必须遵守以下规范：

| 规范项 | ❌ 严禁做法 | ✅ 正确做法 |
| :--- | :--- | :--- |
| **窗体继承** | 继承原生 `System.Windows.Forms.Form` | **必须继承 `AntdUI.BaseForm`**（或 `Window`、`BorderlessForm`）|
| **控件定位** | 硬编码绝对坐标 `Location = new Point(120, 350)` | 使用 `Dock`、`Anchor`、`Margin`、`Padding` 及布局容器 |
| **控件尺寸** | 随意给内部控件定死绝对像素宽度 | 父级使用 `DockStyle.Fill`，宽度由容器自适应填充 |
| **内容超长** | 固定面板尺寸，超出部分被直接裁切截断 | 主工作区容器设置 `AutoScroll = true` 自动出平滑滚动条 |
| **表单排版** | 手工测量每个输入框的 X/Y 像素 | 使用 `GridPanel`（栅格等分）或 `FlowPanel`（流式换行）|
| **表格排版** | 所有列固定写死 Width，窗体放大后右侧大片空白 | 设置 `AutoSizeColumnsMode = ColumnsMode.Fill`，仅保留必要列 `Fixed = true` |
| **字体渲染** | 默认 GDI 渲染导致高 DPI 下边缘发虚 | 开启 `AntdUI.Config.TextRenderingHighQuality = true` |

---

## 二、高 DPI 硬件级自适应配置

### 1. 窗体基类开启 DPI 处理
AntdUI 内部封装了专门的 DPI 计算逻辑：
```csharp
public partial class MainForm : AntdUI.BaseForm
{
    public MainForm()
    {
        InitializeComponent();
        // 确保开启 DPI 自动缩放（BaseForm 默认即为 true）
        this.AutoHandDpi = true;
    }
}
```

### 2. 入口程序配置（Program.cs）

#### .NET Core / .NET 6 / 8 / 9 / 10 推荐配置：
```csharp
internal static class Program
{
    [STAThread]
    static void Main()
    {
        // 1. 声明每监视器 PerMonitorV2 DPI 感知（关键！）
        Application.SetHighDpiMode(HighDpiMode.PerMonitorV2);
        Application.EnableVisualStyles();
        Application.SetCompatibleTextRenderingDefault(false);

        // 2. 开启 AntdUI 高质量抗锯齿字体渲染
        AntdUI.Config.TextRenderingHighQuality = true;

        // 3. 微调常见字体（如微软雅黑）垂直对齐
        AntdUI.Config.SetCorrectionTextRendering("Microsoft YaHei UI", "中");

        Application.Run(new MainForm());
    }
}
```

#### .NET Framework (4.7 / 4.8) 配置：
必须在 `app.manifest` 中开启高 DPI 支持：
```xml
<application xmlns="urn:schemas-microsoft-com:asm.v3">
  <windowsSettings>
    <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true/pm</dpiAware>
    <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitorV2, PerMonitor</dpiAwareness>
  </windowsSettings>
</application>
```

### 3. VS 设计器高分屏不失真配置
在项目工程文件 `.csproj` 中加入：
```xml
<PropertyGroup>
    <!-- 防止 Visual Studio 高 DPI 缩放破坏窗体设计尺寸 -->
    <ForceDesignerDPIUnaware>true</ForceDesignerDPIUnaware>
</PropertyGroup>
```

### 4. 运行时 DPI 兼容模式与换算
当遇到个别特殊系统缩放异常时：
```csharp
// 切换为兼容 DPI 模式
AntdUI.Config.DpiMode = DpiMode.Compatible;

// 手动代码动态计算基于 DPI 的尺寸增量：
int dynamicPadding = AntdUI.Config.Dpi(16); // 100%下是16，150%下自动为24
```

---

## 三、窗口与分辨率自适应机制

### 1. 经典三段式 / 左右式自适应骨架（Dock 体系）

在根窗体或主页面中，使用 `DockStyle` 划分互不干扰的自适应区域：
```csharp
// 顶部标题栏 / 工具栏：Top（高度固定，宽度 100% 自适应跟随窗体）
var header = new AntdUI.WindowBar
{
    Dock = DockStyle.Top,
    Height = 48,
};

// 底部状态栏 / 分页器：Bottom（高度固定，宽度 100% 自适应跟随窗体）
var footer = new AntdUI.Pagination
{
    Dock = DockStyle.Bottom,
    Height = 44,
};

// 左侧导航栏：Left（宽度固定，高度 100% 自适应拉伸）
var leftNav = new AntdUI.Menu
{
    Dock = DockStyle.Left,
    Width = 220,
};

// 中间工作区：Fill（占据剩余全部空间，宽和高随窗口拉伸自动伸缩）
var contentPanel = new Panel
{
    Dock = DockStyle.Fill,
    AutoScroll = true, // 关键：小屏幕自动出滚动条
};

// 注意添加顺序：先 Add 中间 Fill，再 Add 边界 Top/Left/Bottom
this.Controls.Add(contentPanel);
this.Controls.Add(leftNav);
this.Controls.Add(footer);
this.Controls.Add(header);
```

### 2. 局部浮动组件（Anchor 体系）
当某些控件不需要填满整个区域，而是要**靠右对齐、靠底对齐或随宽度拉伸**时：
```csharp
// 靠右下角固定的操作按钮（如“确定”、“取消”）
btnSubmit.Anchor = AnchorStyles.Bottom | AnchorStyles.Right;

// 横向拉伸的进度条（高度固定，左右随窗口自适应拉伸）
progressBar.Anchor = AnchorStyles.Top | AnchorStyles.Left | AnchorStyles.Right;
```

---

## 四、AntdUI 专用自适应容器选型与实践

### 1. `GridPanel`：栅格等分布局（最适合表单、仪表盘卡片）
根据 `Column` 列数，容器会自动将每列宽度平分：
```csharp
var grid = new AntdUI.GridPanel
{
    Dock = DockStyle.Top,
    Column = 2,       // 2列等分
    Gap = 16,         // 左右间距
    GapRow = 16,      // 上下行间距
    Padding = new Padding(16),
};

// 向其中添加的控件不需要手动计算位置，GridPanel 自动按列排布并充满每格宽度
grid.Controls.Add(nameInput);
grid.Controls.Add(emailInput);
grid.Controls.Add(phoneInput);
grid.Controls.Add(deptSelect);
```

### 2. `FlowPanel`：自适应流式换行（最适合标签、筛选条件、操作按钮栏）
当窗口宽度变小时，放不下的子控件自动折行；当窗口变宽时自动排成一行：
```csharp
var flow = new AntdUI.FlowPanel
{
    Dock = DockStyle.Top,
    Wrap = true,   // 开启自动换行
    Gap = 8,
    GapRow = 8,
    Padding = new Padding(8),
};

// 添加若干筛选 Tag 或操作按钮
foreach (var filter in filters)
{
    flow.Controls.Add(new AntdUI.Tag { Text = filter.Name });
}
```

### 3. `StackPanel`：线性弹性布局
按行或按列紧密堆叠，支持间距自适应：
```csharp
var stack = new AntdUI.StackPanel
{
    Dock = DockStyle.Top,
    Vertical = false, // 水平排列
    Gap = 12,
};
stack.Controls.Add(searchInput);
stack.Controls.Add(btnSearch);
stack.Controls.Add(btnReset);
```

### 4. `Splitter`：自适应双栏分割（可拖拽）
```csharp
var split = new AntdUI.Splitter
{
    Dock = DockStyle.Fill,
    Vertical = false, // 左右分隔
    SplitPosition = 260, // 初始位置
    MinSize = 180,       // 最小限制，避免拉没
};
```

---

## 五、Table 表格列宽与数据自适应

WinForms 表格最容易出现的 bug 是窗口放大后，右侧留有一大块刺眼的空白；或者小屏下内容挤爆被截断。

### 1. 全宽自适应列配置（`ColumnsMode.Fill`）
```csharp
var table = new AntdUI.Table
{
    Dock = DockStyle.Fill,
    Bordered = true,
    FixedHeader = true,
    AutoSizeColumnsMode = AntdUI.ColumnsMode.Fill, // 关键：所有列等比或填充整张表
};

table.Columns = new AntdUI.ColumnCollection
{
    // 固定宽度的列（如序号、勾选框、操作列）
    new AntdUI.Column("Id", "ID") { Width = 60, Fixed = true },

    // 自适应伸缩列（名称、描述等根据剩余宽度自动拉伸）
    new AntdUI.Column("Title", "任务标题") { Align = AntdUI.TAlign.Left },
    new AntdUI.Column("Remark", "备注描述") { Align = AntdUI.TAlign.Left },

    // 固定宽度的操作列
    new AntdUI.Column("Action", "操作") { Width = 140, Fixed = true },
};
```

### 2. 文本溢出省略提示
开启 `ShowTip = true`，当某列文字自适应缩短无法完全显示时，鼠标悬停自动弹出 Tooltip：
```csharp
table.ShowTip = true;
```

---

## 六、响应式断点（Responsive Breakpoints）

在桌面端，用户经常会执行**分屏（半屏操作）**或在**平板/小笔记本**上使用，此时需要动态调整布局：

```csharp
public partial class ResponsiveMainForm : AntdUI.BaseForm
{
    private AntdUI.Menu sideMenu;
    private AntdUI.GridPanel cardGrid;

    public ResponsiveMainForm()
    {
        InitializeComponent();
        this.SizeChanged += ResponsiveMainForm_SizeChanged;
    }

    private void ResponsiveMainForm_SizeChanged(object? sender, EventArgs e)
    {
        int w = this.ClientSize.Width;

        // 断点 1：宽度 < 800px（小屏/半屏）
        if (w < 800)
        {
            // 自动收缩侧边菜单为极简图标栏
            if (!sideMenu.Collapsed) sideMenu.Collapsed = true;
            // 表单或卡片流由多列变为单列
            if (cardGrid.Column != 1) cardGrid.Column = 1;
        }
        // 断点 2：800px <= 宽度 < 1200px（标准屏）
        else if (w < 1200)
        {
            if (sideMenu.Collapsed) sideMenu.Collapsed = false;
            if (cardGrid.Column != 2) cardGrid.Column = 2;
        }
        // 断点 3：宽度 >= 1200px（宽屏 / 4K）
        else
        {
            if (sideMenu.Collapsed) sideMenu.Collapsed = false;
            if (cardGrid.Column != 4) cardGrid.Column = 4;
        }
    }
}
```

---

## 七、弹窗与抽屉（Modal & Drawer）自适应

### 1. Modal 弹窗自适应父窗体
弹窗不应写死超过父窗体的像素，应设置相对合理的宽度，并在小屏下自适应：
```csharp
int parentWidth = this.ClientSize.Width;
int modalWidth = Math.Min(560, (int)(parentWidth * 0.9)); // 最大560，最小保持90%宽度

AntdUI.Modal.open(new AntdUI.Modal.Config(this, "自适应弹窗", myEditControl)
{
    Width = modalWidth,
    MaskClosable = true,
});
```

### 2. Drawer 抽屉宽度自适应
```csharp
int drawerWidth = Math.Min(400, (int)(this.ClientSize.Width * 0.8));
AntdUI.Drawer.open(new AntdUI.Drawer.Config(this, myDetailPanel)
{
    Align = AntdUI.TAlignMini.Right,
    Width = drawerWidth,
});
```

---

## 八、完整自适应响应式界面实战代码

以下是一个包含**多端 DPI 适配、自动响应式断点、自适应表格列宽、自动出滚动条**的完整界面：

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using AntdUI;

public class FullyAdaptiveForm : AntdUI.BaseForm
{
    private AntdUI.WindowBar windowBar;
    private AntdUI.Menu menu;
    private Panel bodyPanel;
    private AntdUI.GridPanel summaryGrid;
    private AntdUI.Table dataTable;
    private AntdUI.Pagination pagination;

    public FullyAdaptiveForm()
    {
        this.AutoHandDpi = true;
        this.Size = new Size(1100, 720);
        this.MinimumSize = new Size(640, 480);
        this.Text = "自适应业务管理中心";

        BuildResponsiveUI();
        this.SizeChanged += (s, e) => HandleResponsiveBreakpoints();
    }

    private void BuildResponsiveUI()
    {
        // 1. 顶部标题栏自适应拉伸
        windowBar = new AntdUI.WindowBar
        {
            Dock = DockStyle.Top,
            Height = 44,
            ShowIcon = true,
            Text = this.Text,
        };

        // 2. 左侧菜单
        menu = new AntdUI.Menu
        {
            Dock = DockStyle.Left,
            Width = 200,
            Unique = true,
        };
        menu.Items.Add(new AntdUI.MenuItem("数据概览") { ImageSvg = AntdUI.SvgDb.DashboardOutlined });
        menu.Items.Add(new AntdUI.MenuItem("用户管理") { ImageSvg = AntdUI.SvgDb.UserOutlined });
        menu.Items.Add(new AntdUI.MenuItem("配置中心") { ImageSvg = AntdUI.SvgDb.SettingOutlined });

        // 3. 主体工作区（必须 Fill，开启 AutoScroll 防小屏内容溢出）
        bodyPanel = new Panel
        {
            Dock = DockStyle.Fill,
            AutoScroll = true,
            Padding = new Padding(16),
        };

        // 4. 统计卡片栅格（动态自适应列数）
        summaryGrid = new AntdUI.GridPanel
        {
            Dock = DockStyle.Top,
            Height = 110,
            Column = 3,
            Gap = 16,
            Margin = new Padding(0, 0, 0, 16),
        };
        summaryGrid.Controls.Add(CreateStatCard("今日活跃", "12,840", AntdUI.TType.Primary));
        summaryGrid.Controls.Add(CreateStatCard("订单成交", "￥89,400", AntdUI.TType.Success));
        summaryGrid.Controls.Add(CreateStatCard("待处理异常", "3 件", AntdUI.TType.Warn));

        // 5. 分页器（贴底拉伸）
        pagination = new AntdUI.Pagination
        {
            Dock = DockStyle.Bottom,
            Height = 44,
            Total = 300,
            PageSize = 10,
            ShowSizeChanger = true,
            ShowTotal = true,
        };

        // 6. 表格（Fill 铺满剩余所有空间，列宽全宽自适应）
        dataTable = new AntdUI.Table
        {
            Dock = DockStyle.Fill,
            Bordered = true,
            FixedHeader = true,
            AutoSizeColumnsMode = AntdUI.ColumnsMode.Fill,
            ShowTip = true,
        };
        dataTable.Columns = new AntdUI.ColumnCollection
        {
            new AntdUI.Column("Id", "ID") { Width = 60, Fixed = true },
            new AntdUI.Column("Title", "业务标题") { Align = AntdUI.TAlign.Left },
            new AntdUI.Column("User", "经办人") { Width = 120 },
            new AntdUI.Column("Time", "更新时间") { Width = 160 },
            new AntdUI.Column("Status", "状态") { Width = 90, Fixed = true },
        };

        // 容器装载层级
        bodyPanel.Controls.Add(dataTable);
        bodyPanel.Controls.Add(pagination);
        bodyPanel.Controls.Add(summaryGrid);

        this.Controls.Add(bodyPanel);
        this.Controls.Add(menu);
        this.Controls.Add(windowBar);
    }

    private Control CreateStatCard(string title, string value, AntdUI.TType type)
    {
        var card = new AntdUI.Panel
        {
            Radius = 8,
            Shadow = 2,
            Padding = new Padding(12),
        };
        var lblTitle = new AntdUI.Label
        {
            Text = title,
            Dock = DockStyle.Top,
            Height = 22,
            ForeColor = Color.Gray,
        };
        var lblVal = new AntdUI.Label
        {
            Text = value,
            Dock = DockStyle.Fill,
            Font = new Font("Segoe UI", 16F, FontStyle.Bold),
            Type = type,
            TextAlign = ContentAlignment.MiddleLeft,
        };
        card.Controls.Add(lblVal);
        card.Controls.Add(lblTitle);
        return card;
    }

    private void HandleResponsiveBreakpoints()
    {
        int width = this.ClientSize.Width;

        // 小屏幕/分屏：收缩侧边栏，卡片改为单列
        if (width < 768)
        {
            menu.Collapsed = true;
            summaryGrid.Column = 1;
            summaryGrid.Height = 320;
        }
        // 中等屏幕：展开侧边栏，卡片双列
        else if (width < 1024)
        {
            menu.Collapsed = false;
            summaryGrid.Column = 2;
            summaryGrid.Height = 220;
        }
        // 宽屏模式：三列铺平
        else
        {
            menu.Collapsed = false;
            summaryGrid.Column = 3;
            summaryGrid.Height = 110;
        }
    }
}
```
