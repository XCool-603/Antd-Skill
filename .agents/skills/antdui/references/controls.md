# AntdUI 控件完整属性参考

> 文档来源：https://github.com/AntdUI/AntdUI/tree/main/doc/wiki/en/Control
> DefaultProperty / DefaultEvent 标注为设计器默认。

---

## 一、通用控件（General）

### Button 按钮

> DefaultProperty: `Text`，DefaultEvent: `Click`

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `Text` | 显示文字 | string | - |
| `Type` | 按钮类型 | TTypeMini | Default |
| `Shape` | 形状 | TShape | Default |
| `Radius` | 圆角半径 | int | 6 |
| `WaveSize` | 点击波纹大小 | int | 4 |
| `BorderWidth` | 边框宽度 | float | 0F |
| `AutoSize` | 自动大小 | bool | false |
| `AutoSizeMode` | 自动大小模式 | TAutoSize | None |
| `DisplayStyle` | 显示图标/文字/两者 | TButtonDisplayStyle | Default |
| `Ghost` | 幽灵按钮（透明背景） | bool | false |
| `Loading` | 加载中状态 | bool | false |
| `ImageSvg` | SVG 图标字符串 | string? | null |
| `Image` | 位图图标 | Image? | null |
| `ForeColor` | 文字颜色 | Color? | null |
| `ForeHover` | 悬停文字颜色 | Color? | null |
| `ForeActive` | 激活文字颜色 | Color? | null |
| `BackColor` | 背景颜色 | Color? | null |
| `BackExtend` | 渐变背景色（格式: `#ff0000,#0000ff`）| string? | null |
| `BackHover` | 悬停背景色 | Color? | null |
| `BackActive` | 激活背景色 | Color? | null |
| `DefaultBack` | Default 类型背景色 | Color? | null |
| `DefaultBorderColor` | Default 类型边框色 | Color? | null |
| `BackgroundImage` | 背景图片 | Image? | null |
| `BackgroundImageLayout` | 背景图片布局 | TFit | Fill |
| `OriginalBackColor` | 原始背景色（禁止覆盖） | Color | Transparent |

```csharp
var btn = new AntdUI.Button
{
    Text = "保存",
    Type = AntdUI.TTypeMini.Primary,
    Shape = AntdUI.TShape.Round,
    ImageSvg = AntdUI.SvgDb.SaveOutlined,
    Loading = false,
};
btn.Click += async (s, e) =>
{
    btn.Loading = true;
    await Task.Delay(1000);
    btn.Loading = false;
    AntdUI.Message.success(form, "保存成功");
};
```

### FloatButton 悬浮按钮

> DefaultProperty: `Text`，DefaultEvent: `Click`

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `Type` | 类型 | TTypeMini | Default |
| `Shape` | 形状 | TShape | Circle |
| `ImageSvg` | SVG 图标 | string? | null |
| `Image` | 位图图标 | Image? | null |
| `Text` | 文字 | string? | null |
| `Tooltip` | 悬停提示 | string? | null |

---

## 二、布局控件（Layout）

### Divider 分割线

> 区隔内容的分割线

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `Text` | 文字内容 | string | - |
| `LocalizationText` | 🌏 国际化文字 | string? | null |
| `Orientation` | 文字对齐方向 | TOrientation | None（居中）|
| `Vertical` | 是否垂直分割线 | bool | false |
| `ColorSplit` | 分割线颜色 | Color? | null |

```csharp
var divider = new AntdUI.Divider { Text = "基本信息" };
var vDivider = new AntdUI.Divider { Vertical = true, Height = 20 };
```

### StackPanel 堆栈布局

> 水平或垂直方向排列子控件

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `Vertical` | 是否垂直排列 | bool | false |
| `Gap` | 子控件间距(px) | int | 0 |
| `Wrap` | 是否换行 | bool | false |
| `PauseLayout` | 暂停布局更新 | bool | false |

### FlowPanel 流动布局

> 按行自动排列，超出自动换行

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `Gap` | 间距 | int | 0 |
| `GapRow` | 行间距 | int? | null |
| `Wrap` | 是否换行 | bool | true |
| `PauseLayout` | 暂停布局更新 | bool | false |

### GridPanel 格栅布局

> 精确划分行列区域

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `Column` | 列数 | int | 2 |
| `Gap` | 间距 | int | 0 |
| `GapRow` | 行间距 | int? | null |
| `PauseLayout` | 暂停布局更新 | bool | false |

### Splitter 分隔面板

> 可拖动调整比例的分隔面板

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `Vertical` | 是否垂直分隔 | bool | false |
| `SplitPosition` | 分隔条位置(px) | int | - |
| `MinSize` | 最小尺寸 | int | 20 |
| `MaxSize` | 最大尺寸 | int | - |

---

## 三、导航控件（Navigation）

### Breadcrumb 面包屑

> DefaultProperty: `Items`，DefaultEvent: `ItemClick`

```csharp
var bc = new AntdUI.Breadcrumb();
bc.Items.Add(new AntdUI.BreadcrumbItem("首页") { ImageSvg = AntdUI.SvgDb.HomeOutlined });
bc.Items.Add(new AntdUI.BreadcrumbItem("列表"));
bc.Items.Add(new AntdUI.BreadcrumbItem("详情"));
bc.ItemClick += (s, e) => Console.WriteLine(e.Item.Text);
```

### Dropdown 下拉菜单

> DefaultProperty: `Items`，DefaultEvent: `ItemClick`

```csharp
var dd = new AntdUI.Dropdown
{
    Text = "更多操作",
    Type = AntdUI.TTypeMini.Default,
};
dd.Items.Add(new AntdUI.MenuItem("编辑") { ImageSvg = AntdUI.SvgDb.EditOutlined });
dd.Items.Add(new AntdUI.MenuItem("删除") { ImageSvg = AntdUI.SvgDb.DeleteOutlined });
dd.ItemClick += (s, e) => Console.WriteLine(e.Item.Text);
```

### Menu 导航菜单

> DefaultProperty: `Items`，DefaultEvent: `SelectChanged`

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `Collapsed` | 折叠状态 | bool | false |
| `CollapsedWidth` | 折叠宽度 | int | 48 |
| `Unique` | 只保持一个子菜单展开 | bool | false |
| `Trigger` | 触发方式 | Trigger | Hover |
| `Indent` | 树形缩进 | bool | false |
| `InlineIndent` | 缩进宽度 | int? | null |
| `Gap` | 菜单项间距 | int? | null |
| `Radius` | 圆角 | int | 6 |
| `Round` | 圆角样式 | bool | false |
| `FocusMode` | 焦点指示模式 | TFocusMode | None |
| `FocusModeColor` | 焦点指示颜色 | Color? | null |
| `FocusModeAlign` | 焦点指示位置 | TAlignMini | None |
| `ShowSubBack` | 显示子菜单背景 | bool | false |
| `IconRatio` | 图标比例 | float | 1.2F |
| `MouseRightCtrl` | 鼠标右键控制 | bool | true |
| `ForeColor` | 文字颜色 | Color? | null |
| `ForeActive` | 激活文字颜色 | Color? | null |
| `BackHover` | 悬停背景色 | Color? | null |
| `BackActive` | 激活背景色 | Color? | null |

```csharp
var menu = new AntdUI.Menu { Dock = DockStyle.Left, Width = 220 };
menu.Items.Add(new AntdUI.MenuItem("首页") { ImageSvg = AntdUI.SvgDb.HomeOutlined });
var sys = new AntdUI.MenuItem("系统设置") { ImageSvg = AntdUI.SvgDb.SettingOutlined };
sys.Sub.Add(new AntdUI.MenuItem("用户管理") { ImageSvg = AntdUI.SvgDb.UserOutlined });
sys.Sub.Add(new AntdUI.MenuItem("角色管理") { ImageSvg = AntdUI.SvgDb.TeamOutlined });
menu.Items.Add(sys);
menu.SelectChanged += (s, e) => LoadPage(e.Item.Text);
```

### PageHeader 页头

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `Title` | 标题 | string | - |
| `SubTitle` | 副标题 | string? | null |
| `ShowBack` | 是否显示返回按钮 | bool | false |
| `Ghost` | 幽灵模式（透明背景）| bool | true |

### TabHeader 标签页头

> 独立的标签切换头部控件

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `Type` | 类型 | TTabType | Line |
| `Gap` | 间距 | int | 0 |
| `Centered` | 居中显示 | bool | false |

### Pagination 分页

> DefaultProperty: `Total`，DefaultEvent: `PageChanged`

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `Total` | 总条数 | int | 0 |
| `PageSize` | 每页条数 | int | 10 |
| `Current` | 当前页（从1开始）| int | 1 |
| `ShowSizeChanger` | 显示每页条数切换 | bool | false |
| `ShowQuickJumper` | 显示快速跳转 | bool | false |
| `ShowTotal` | 显示总条数文字 | bool | true |

```csharp
var pager = new AntdUI.Pagination
{
    Total = 500,
    PageSize = 20,
    Current = 1,
    ShowSizeChanger = true,
    ShowQuickJumper = true,
};
pager.PageChanged += (s, e) => LoadData(e.Current, e.PageSize);
```

### Steps 步骤条

> DefaultProperty: `Items`，DefaultEvent: `ItemClick`

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `Current` | 当前步骤（从0开始）| int | 0 |
| `Vertical` | 是否垂直方向 | bool | false |
| `Type` | 类型 | TStepType | Default |

```csharp
var steps = new AntdUI.Steps { Current = 1 };
steps.Items.Add(new AntdUI.StepItem
{
    Title = "第一步",
    Description = "已完成",
    Status = AntdUI.TStepStatus.Finish,
});
steps.Items.Add(new AntdUI.StepItem
{
    Title = "第二步",
    Description = "进行中",
    Status = AntdUI.TStepStatus.Process,
});
steps.Items.Add(new AntdUI.StepItem
{
    Title = "第三步",
    Description = "待处理",
    Status = AntdUI.TStepStatus.Wait,
});
```

---

## 四、数据录入控件（Data Entry）

### Checkbox 复选框

> DefaultProperty: `Checked`，DefaultEvent: `CheckedChanged`

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `Text` | 文字标签 | string? | null |
| `Checked` | 选中状态 | bool | false |
| `IndeterminateState` | 半选状态 | bool | false |
| `AutoSize` | 自动大小 | bool | false |
| `ForeColor` | 文字颜色 | Color? | null |
| `Fill` | 填充颜色 | Color? | null |

```csharp
// 单个复选框
var cb = new AntdUI.Checkbox { Text = "记住密码" };
cb.CheckedChanged += (s, e) => Config.RememberPwd = cb.Checked;

// CheckboxGroup（批量选项）
var cbGroup = new AntdUI.CheckboxGroup();
cbGroup.Items.Add(new AntdUI.CheckboxItem("苹果", "apple"));
cbGroup.Items.Add(new AntdUI.CheckboxItem("香蕉", "banana"));
cbGroup.Items.Add(new AntdUI.CheckboxItem("橙子", "orange"));
cbGroup.ValueChanged += (s, e) =>
{
    // cbGroup.Values 为已选 value 列表
};
```

### ColorPicker 颜色选择器

> DefaultProperty: `Value`，DefaultEvent: `ValueChanged`

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `Value` | 当前颜色 | Color | - |
| `ShowText` | 是否显示颜色值文字 | bool | true |
| `Format` | 颜色格式 | TColorFormat | Hex |
| `AllowClear` | 允许清除 | bool | false |

```csharp
var cp = new AntdUI.ColorPicker { Value = Color.Blue };
cp.ValueChanged += (s, e) => ApplyColor(cp.Value);
```

### DatePicker 日期选择器

> DefaultProperty: `Value`，DefaultEvent: `ValueChanged`，继承自 Input

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `Format` | 格式字符串 | string | "yyyy-MM-dd" |
| `Value` | 当前日期 | DateTime? | null |
| `MinDate` | 最小可选日期 | DateTime? | null |
| `MaxDate` | 最大可选日期 | DateTime? | null |
| `Placement` | 弹出位置 | TAlignFrom | BL |
| `ShowIcon` | 显示日历图标 | bool | true |
| `DropDownArrow` | 显示下拉箭头 | bool | false |
| `Presets` | 预设快捷选项 | BaseCollection | - |
| `BadgeAction` | 日期徽标回调 | Func<DateTime[], List<DateBadge>?>? | null |

```csharp
var dp = new AntdUI.DatePicker
{
    Format = "yyyy-MM-dd",
    MinDate = new DateTime(2020, 1, 1),
    MaxDate = DateTime.Now,
    AllowClear = true,
};
// 带时间
dp.Format = "yyyy-MM-dd HH:mm:ss";
dp.ValueChanged += (s, e) => Console.WriteLine(dp.Value);
```

### DatePickerRange 日期范围

> DefaultEvent: `ValueChanged`，继承自 Input

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `Format` | 格式 | string | "yyyy-MM-dd" |
| `Value` | 日期范围 | DateTime[]? | null（[开始, 结束]）|
| `Separator` | 分隔符文字 | string | "至" |

```csharp
var dr = new AntdUI.DatePickerRange { Format = "yyyy-MM-dd" };
dr.ValueChanged += (s, e) =>
{
    if (dr.Value?.Length == 2)
    {
        var start = dr.Value[0];
        var end   = dr.Value[1];
    }
};
```

### Input 输入框

> DefaultProperty: `Text`，DefaultEvent: `TextChanged`

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `Text` | 文本内容 | string | - |
| `PlaceholderText` | 占位符 | string? | null |
| `PlaceholderColor` | 占位符颜色 | Color? | null |
| `AllowClear` | 显示清除按钮 | bool | false |
| `ReadOnly` | 只读 | bool | false |
| `Multiline` | 多行模式 | bool | false |
| `WordWrap` | 自动换行（多行）| bool | true |
| `MaxLength` | 最大字符数 | int | - |
| `TextAlign` | 文字对齐 | HorizontalAlignment | Left |
| `UseSystemPasswordChar` | 密码框模式 | bool | false |
| `PasswordChar` | 自定义密码字符 | char | (char)0 |
| `PasswordCopy` | 密码可复制 | bool | false |
| `Radius` | 圆角 | int | 6 |
| `Round` | 胶囊形圆角 | bool | false |
| `Status` | 验证状态 | TType | None |
| `Variant` | 样式变体 | TVariant | Outlined |
| `BorderWidth` | 边框宽度 | float | 1F |
| `BorderColor` | 边框颜色 | Color? | null |
| `BorderHover` | 悬停边框颜色 | Color? | null |
| `BorderActive` | 激活边框颜色 | Color? | null |
| `CaretColor` | 光标颜色 | Color? | null |
| `CaretSpeed` | 光标闪烁速度(ms) | int | 1000 |
| `SelectionColor` | 选中文字背景色 | Color | 102, 0, 127, 255 |
| `AutoScroll` | 显示滚动条（多行）| bool | false |
| `ImeMode` | 输入法模式 | ImeMode | NoControl |
| `AcceptsTab` | 多行中允许 Tab | bool | false |
| `LineHeight` | 多行行高 | int | 0 |
| `WaveSize` | 点击波纹 | int | 4 |
| `Prefix` | 前缀文字 | string? | null |
| `Suffix` | 后缀文字 | string? | null |
| `PrefixText` | 前缀文字（组合框）| string? | null |
| `SuffixText` | 后缀文字（组合框）| string? | null |
| `PrefixIcon` | 前缀图标 | Image? | null |
| `SuffixIcon` | 后缀图标 | Image? | null |
| `PrefixSvg` | 前缀 SVG 图标 | string? | null |
| `SuffixSvg` | 后缀 SVG 图标 | string? | null |

```csharp
// 搜索框
var searchInput = new AntdUI.Input
{
    PlaceholderText = "搜索...",
    SuffixSvg = AntdUI.SvgDb.SearchOutlined,
    AllowClear = true,
    Radius = 20,
    Round = true,
};

// 前缀组合框（Addon）
var addonInput = new AntdUI.Input
{
    PrefixText = "https://",
    SuffixText = ".com",
    PlaceholderText = "输入域名",
};

// 验证失败状态
input.Status = AntdUI.TType.Error;
// 验证成功
input.Status = AntdUI.TType.Success;
// 清除验证状态
input.Status = AntdUI.TType.None;
```

### InputNumber 数字输入框

> DefaultProperty: `Value`，DefaultEvent: `ValueChanged`，继承自 Input

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `Value` | 当前值 | decimal | 0 |
| `Minimum` | 最小值 | decimal | 0 |
| `Maximum` | 最大值 | decimal | 100 |
| `Step` | 步进值 | decimal | 1 |
| `DecimalPlaces` | 小数位数 | int | 0 |
| `ShowControl` | 显示加减按钮 | bool | true |
| `WheelModifyEnabled` | 滚轮修改值 | bool | true |

```csharp
var numInput = new AntdUI.InputNumber
{
    Minimum = 1,
    Maximum = 999,
    Value = 10,
    Step = 1,
    DecimalPlaces = 0,
};
numInput.ValueChanged += (s, e) => Console.WriteLine(numInput.Value);
```

### Radio 单选框

> DefaultProperty: `Checked`，DefaultEvent: `CheckedChanged`

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `Text` | 文字标签 | string? | null |
| `Checked` | 是否选中 | bool | false |
| `AutoSize` | 自动大小 | bool | false |
| `ForeColor` | 文字颜色 | Color? | null |
| `Fill` | 填充颜色 | Color? | null |

```csharp
// RadioGroup（推荐）
var rg = new AntdUI.RadioGroup();
rg.Items.Add(new AntdUI.RadioItem("男", "male"));
rg.Items.Add(new AntdUI.RadioItem("女", "female"));
rg.ValueChanged += (s, e) => Console.WriteLine(rg.Value); // "male" or "female"

// 独立 Radio（需手动维护互斥）
var r1 = new AntdUI.Radio { Text = "选项A", Checked = true };
var r2 = new AntdUI.Radio { Text = "选项B" };
```

### Rate 评分

> DefaultProperty: `Value`，DefaultEvent: `ValueChanged`

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `Value` | 当前评分 | float | 0 |
| `Count` | 总星数 | int | 5 |
| `AllowHalf` | 允许半星 | bool | false |
| `AllowClear` | 点击已选清除 | bool | true |
| `ReadOnly` | 只读 | bool | false |
| `Fill` | 填充颜色 | Color? | null |

```csharp
var rate = new AntdUI.Rate
{
    Count = 5,
    Value = 3.5f,
    AllowHalf = true,
    ReadOnly = false,
};
rate.ValueChanged += (s, e) => Console.WriteLine($"评分: {rate.Value}");
```

### Select 下拉选择

> DefaultProperty: `Text`，DefaultEvent: `SelectedIndexChanged`，继承自 Input

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `MaxCount` | 下拉列表最多显示行 | int | 4 |
| `Placement` | 弹出位置 | TAlignFrom | BL |
| `List` | 列表样式（类似 Dropdown）| bool | false |
| `ListAutoWidth` | 列表自动宽度 | bool | false |
| `DropDownRadius` | 下拉圆角 | int? | null |
| `DropDownArrow` | 显示下拉箭头 | bool | false |
| `DropDownPadding` | 下拉内边距 | Size | 12, 5 |
| `DropDownTextAlign` | 下拉文字对齐 | TAlign | Left |
| `CloseIcon` | 显示关闭图标 | bool | false |
| `AutoText` | 自动设置文字 | bool | true |
| `WheelModifyEnabled` | 滚轮切换选项 | bool | true |
| `Empty` | 空数据仍然显示下拉 | bool | false |

```csharp
var select = new AntdUI.Select
{
    PlaceholderText = "请选择城市",
    AllowClear = true,
    MaxCount = 8,
};
// 普通选项
select.Items.Add(new AntdUI.SelectItem("北京", "bj"));
select.Items.Add(new AntdUI.SelectItem("上海", "sh"));
// 分组标题（不可选）
select.Items.Add(new AntdUI.SelectItem("其他城市") { Sub = true });
select.Items.Add(new AntdUI.SelectItem("广州", "gz"));

// 读取选中值
select.SelectedIndexChanged += (s, e) =>
{
    string val  = (string)select.Value;     // value
    string text = select.Text;              // text
};
```

### Slider 滑块

> DefaultProperty: `Value`，DefaultEvent: `ValueChanged`

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `Value` | 当前值 | int | 0 |
| `MinValue` | 最小值 | int | 0 |
| `MaxValue` | 最大值 | int | 100 |
| `Align` | 方向 | TAlignMini | - |
| `Fill` | 进度颜色 | Color? | null |
| `FillHover` | 悬停颜色 | Color? | null |
| `FillActive` | 激活颜色 | Color? | null |
| `TrackColor` | 轨道颜色 | Color? | null |
| `ShowTooltip` | 拖动时显示提示 | bool | true |

```csharp
var slider = new AntdUI.Slider
{
    MinValue = 0,
    MaxValue = 100,
    Value = 60,
    Align = AntdUI.TAlignMini.Left, // 横向
};
slider.ValueChanged += (s, e) => Console.WriteLine(slider.Value);
```

### Switch 开关

> DefaultProperty: `Checked`，DefaultEvent: `CheckedChanged`

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `Checked` | 选中状态 | bool | false |
| `CheckedText` | 选中时文字 | string? | null |
| `UnCheckedText` | 未选中时文字 | string? | null |
| `Loading` | 加载状态 | bool | false |
| `Fill` | 选中填充色 | Color? | null |
| `FillHover` | 悬停填充色 | Color? | null |
| `ForeColor` | 文字颜色 | Color? | null |

```csharp
var sw = new AntdUI.Switch
{
    Checked = false,
    CheckedText = "开",
    UnCheckedText = "关",
};
sw.CheckedChanged += (s, e) => Console.WriteLine($"开关: {sw.Checked}");
```

### TimePicker 时间选择器

> DefaultProperty: `Value`，DefaultEvent: `ValueChanged`，继承自 Input

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `Format` | 格式 | string | "HH:mm:ss" |
| `Value` | 当前时间 | TimeSpan? | null |
| `MinTime` | 最小时间 | TimeSpan? | null |
| `MaxTime` | 最大时间 | TimeSpan? | null |
| `ShowIcon` | 显示时钟图标 | bool | true |

```csharp
var tp = new AntdUI.TimePicker
{
    Format = "HH:mm",
    Value = new TimeSpan(9, 0, 0),
};
tp.ValueChanged += (s, e) => Console.WriteLine(tp.Value);
```

### Upload 上传

> DefaultEvent: `FileSelected`

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `Text` | 按钮文字 | string | "点击上传" |
| `Accept` | 文件过滤器 | string? | null |
| `Multiple` | 是否多选 | bool | false |
| `Directory` | 是否选择文件夹 | bool | false |
| `ImageMode` | 图片上传模式 | bool | false |

```csharp
var upload = new AntdUI.Upload
{
    Accept = "图片文件|*.jpg;*.png;*.gif",
    Multiple = true,
    ImageMode = true,
};
upload.FileSelected += (s, e) =>
{
    foreach (var file in e.Files)
    {
        Console.WriteLine(file.FullName);
    }
};
```

### InputTag 标签输入

> 输入并回车生成标签

```csharp
var inputTag = new AntdUI.InputTag
{
    PlaceholderText = "输入后按 Enter 添加标签",
    AllowClear = true,
};
// inputTag.Values 为当前所有标签字符串列表
```

---

## 五、数据展示控件（Data Display）

### Avatar 头像

> DefaultProperty: `Image`，DefaultEvent: `Click`

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `Text` | 文字头像内容 | string? | null |
| `Image` | 图片 | Image? | null |
| `ImageSvg` | SVG 图标 | string? | null |
| `Shape` | 形状 | TShape | Circle |
| `Radius` | 圆角（Square时）| int | 6 |
| `BackColor` | 背景色 | Color | Transparent |
| `BorderWidth` | 边框宽度 | float | 0F |
| `BorderColor` | 边框颜色 | Color | 246, 248, 250 |
| `ImgFit` | 图片填充模式 | TFit | Cover |

```csharp
var avatar = new AntdUI.Avatar
{
    Text = "张",
    Shape = AntdUI.TShape.Circle,
    BackColor = Color.FromArgb(0x1677ff),
    ForeColor = Color.White,
    Size = new Size(40, 40),
};
```

### Badge 徽标

> DefaultProperty: `Text`，DefaultEvent: `Click`

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `Count` | 数字徽标值 | int | 0 |
| `Dot` | 小圆点模式 | bool | false |
| `OverflowCount` | 最大显示数（超出显示+）| int | 99 |
| `ShowZero` | Count=0 时是否显示 | bool | false |
| `Status` | 状态点类型 | TType | None |
| `Text` | 自定义文字 | string? | null |
| `Fill` | 填充颜色 | Color? | null |
| `ForeColor` | 文字颜色 | Color? | null |
| `Offset` | 位置偏移 | Point | - |

```csharp
var badge = new AntdUI.Badge { Count = 5 };
var dotBadge = new AntdUI.Badge { Dot = true };
var statusBadge = new AntdUI.Badge { Status = AntdUI.TType.Success, Text = "在线" };
```

### Calendar 日历

> DefaultEvent: `DateChanged`

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `Value` | 当前日期 | DateTime | 今天 |
| `Mode` | 显示模式 | TCalendarMode | Month |
| `ShowHeader` | 显示头部 | bool | true |
| `FullScreen` | 全屏模式 | bool | true |
| `BadgeAction` | 日期徽标回调 | Func<DateTime[], List<DateBadge>?>? | null |

```csharp
var cal = new AntdUI.Calendar
{
    Value = DateTime.Now,
    FullScreen = true,
};
cal.DateChanged += (s, e) => Console.WriteLine(cal.Value.ToShortDateString());
```

### Carousel 走马灯

> DefaultProperty: `Image`，DefaultEvent: `SelectIndexChanged`

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `Autoplay` | 自动播放 | bool | false |
| `Autodelay` | 自动切换间隔(s) | int | 4 |
| `Touch` | 手势滑动 | bool | true |
| `TouchOut` | 滑出边界 | bool | false |
| `DotSize` | 指示点大小 | Size | 28×4 |
| `DotPosition` | 指示点位置 | TAlignMini | Bottom |

```csharp
var carousel = new AntdUI.Carousel
{
    Dock = DockStyle.Fill,
    Autoplay = true,
    Autodelay = 3,
};
carousel.Pages.Add(new AntdUI.CarouselItem(image1));
carousel.Pages.Add(new AntdUI.CarouselItem(image2));
carousel.Pages.Add(new AntdUI.CarouselItem(image3));
```

### Label 文字标签

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `Text` | 文字内容 | string | - |
| `Type` | 颜色类型 | TType | None |
| `AutoEllipsis` | 超出省略 | bool | false |
| `AutoSize` | 自动大小 | bool | false |

### Image 图片

> DefaultProperty: `Image`，DefaultEvent: `Click`

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `Image` | 图片 | Image? | null |
| `ImageSvg` | SVG 图标 | string? | null |
| `ImgFit` | 填充模式 | TFit | Contain |
| `Radius` | 圆角 | int | 0 |
| `EnablePreview` | 点击预览大图 | bool | false |

```csharp
var img = new AntdUI.Image
{
    Image = Properties.Resources.Photo,
    ImgFit = AntdUI.TFit.Contain,
    EnablePreview = true,
    Radius = 8,
};
```

### Progress 进度条

> DefaultProperty: `Value`，DefaultEvent: `Click`

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `Value` | 当前进度(0-100) | float | 0 |
| `State` | 状态（影响颜色）| TType | None |
| `Shape` | 形状 | TShapeProgress | Round |
| `Fill` | 进度颜色 | Color? | null |
| `Back` | 背景颜色 | Color? | null |
| `Radius` | 圆角 | int | 0 |
| `ShowTextDot` | 小数位数 | int | 0 |
| `ShowInTaskbar` | 任务栏显示进度 | bool | false |
| `UseSystemText` | 使用系统文字 | bool | false |

```csharp
// 线性进度条
var progress = new AntdUI.Progress
{
    Value = 75,
    State = AntdUI.TType.Success,
    Shape = AntdUI.TShapeProgress.Round,
};

// 圆形进度条
var circleProgress = new AntdUI.Progress
{
    Value = 60,
    Shape = AntdUI.TShapeProgress.Circle,
    Size = new Size(120, 120),
};

// 步骤进度条
var stepsProgress = new AntdUI.Progress
{
    Value = 40,
    Shape = AntdUI.TShapeProgress.Steps,
};

// 任务栏进度
progress.ShowInTaskbar = true;
```

### QRCode 二维码

> DefaultProperty: `Text`，DefaultEvent: `Click`

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `Text` | 二维码内容 | string | - |
| `Fill` | 前景颜色 | Color | Black |
| `Back` | 背景颜色 | Color | White |
| `Level` | 纠错级别 | QRCodeLevel | M |
| `Logo` | 中心 Logo | Image? | null |

```csharp
var qr = new AntdUI.QRCode
{
    Text = "https://github.com/AntdUI/AntdUI",
    Fill = Color.Black,
    Back = Color.White,
    Size = new Size(200, 200),
};
```

### Segmented 分段控制器

> DefaultProperty: `Items`，DefaultEvent: `ValueChanged`

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `Value` | 当前选中值 | object? | null |
| `SelectedIndex` | 当前选中索引 | int | -1 |
| `Radius` | 圆角 | int | 6 |

```csharp
var seg = new AntdUI.Segmented();
seg.Items.Add(new AntdUI.SegmentedItem("日", "day") { ImageSvg = AntdUI.SvgDb.CalendarOutlined });
seg.Items.Add(new AntdUI.SegmentedItem("周", "week"));
seg.Items.Add(new AntdUI.SegmentedItem("月", "month"));
seg.ValueChanged += (s, e) => Console.WriteLine(seg.Value);
```

### Table 表格

> DefaultProperty: `Columns`，DefaultEvent: `CellClick`

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `Bordered` | 显示边框 | bool | false |
| `FixedHeader` | 固定表头 | bool | true |
| `VisibleHeader` | 显示表头 | bool | true |
| `VirtualMode` | 虚拟模式 | bool | false |
| `RowHeight` | 行高 | int? | null |
| `RowHeightHeader` | 表头行高 | int? | null |
| `Gap` | 内边距 | int | 12 |
| `GapCell` | 单元格内边距 | int? | 6 |
| `Radius` | 圆角 | int | 0 |
| `EditMode` | 编辑模式 | TEditMode | None |
| `ClipboardCopy` | 允许复制行 | bool | true |
| `EnableHeaderResizing` | 可拖动调整列宽 | bool | false |
| `ColumnDragSort` | 列拖拽排序 | bool | false |
| `AutoSizeColumnsMode` | 列宽模式 | ColumnsMode | Auto |
| `DefaultExpand` | 树形默认展开 | bool | false |
| `FilterRealTime` | 过滤实时生效 | bool | false |
| `SummaryCustomize` | 启用内置摘要定制 | bool | false |
| `ShowTip` | 省略提示 | bool | true |
| `ScrollBarAvoidHeader` | 滚动条避开表头 | bool | false |

**Column 列对象属性：**

| 属性 | 说明 | 类型 |
|:---|:---|:---|
| `Key` | 数据字段名 | string |
| `Title` | 表头显示文字 | string |
| `Width` | 列宽(px) | int |
| `Fixed` | 固定列 | bool |
| `Align` | 文字对齐 | TAlign |
| `SortOrder` | 排序方式 | SortOrder |
| `Filterable` | 是否可过滤 | bool |
| `ColSpan` | 合并列数 | int |
| `GetValue` | 自定义取值委托 | Func<object,object?>? |
| `SetValue` | 自定义设值委托 | Action<object,object?>? |
| `Render` | 自定义渲染委托 | Action<Graphics,RectangleF,object>? |

```csharp
// 完整示例
var table = new AntdUI.Table
{
    Dock = DockStyle.Fill,
    Bordered = true,
    FixedHeader = true,
    VirtualMode = true,           // 大数据虚拟模式
    EnableHeaderResizing = true,
    EditMode = AntdUI.TEditMode.DoubleClick,
};

table.Columns = new AntdUI.ColumnCollection
{
    new AntdUI.Column("Id", "ID") { Width = 60, Fixed = true },
    new AntdUI.Column("Name", "姓名") { Width = 120, Filterable = true },
    new AntdUI.Column("Age", "年龄") { Width = 80, Align = AntdUI.TAlign.Right },
    new AntdUI.Column("Status", "状态")
    {
        Width = 100,
        Render = (g, rect, row) =>
        {
            // 自定义渲染：根据状态绘制颜色标签
        }
    },
};

table.DataSource = users;

// 事件
table.CellClick    += (s, e) => { };
table.CellDoubleClick += (s, e) => { };
table.RowDoubleClick  += (s, e) => { };
table.SelectChanged   += (s, e) =>
{
    // e.Items 为选中行列表（AllowCheck = true 时）
};
```

### Tag 标签

> DefaultProperty: `Text`，DefaultEvent: `Click`

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `Text` | 文字内容 | string | - |
| `Type` | 预设颜色类型 | TType | None |
| `Closable` | 显示关闭按钮 | bool | false |
| `BorderWidth` | 边框宽度 | float | 0F |
| `Radius` | 圆角 | int | 4 |
| `AutoSize` | 自动大小 | bool | false |
| `ForeColor` | 文字颜色 | Color? | null |
| `BackColor` | 背景颜色 | Color? | null |
| `ImageSvg` | SVG 图标 | string? | null |

```csharp
var tag1 = new AntdUI.Tag { Text = "成功", Type = AntdUI.TType.Success };
var tag2 = new AntdUI.Tag { Text = "警告", Type = AntdUI.TType.Warn };
var tag3 = new AntdUI.Tag { Text = "可关闭", Closable = true };
tag3.CloseClick += (s, e) => tag3.Visible = false;
```

### Timeline 时间轴

> DefaultProperty: `Items`，DefaultEvent: `ItemClick`

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `Items` | 时间轴数据 | TimelineItem[] | [] |
| `Gap` | 间距 | int? | null |
| `ForeColor` | 文字颜色 | Color? | null |
| `FontDescription` | 描述字体 | Font? | null |
| `PauseLayout` | 暂停布局 | bool | false |

```csharp
var tl = new AntdUI.Timeline();
tl.Items.Add(new AntdUI.TimelineItem("创建订单", "2024-01-01 10:00")
{
    Type = AntdUI.TType.Success,
});
tl.Items.Add(new AntdUI.TimelineItem("支付完成", "2024-01-01 10:05")
{
    Type = AntdUI.TType.Success,
});
tl.Items.Add(new AntdUI.TimelineItem("等待发货", "进行中")
{
    Type = AntdUI.TType.Info,
});
```

### Tooltip 文字提示

```csharp
// 为任意控件挂载提示
AntdUI.Tooltip.SetTip(button, "这是一个操作按钮");

// 带标题
AntdUI.Tooltip.SetTip(button, new AntdUI.TooltipConfig
{
    Text = "详细说明",
    Placement = AntdUI.TAlignFrom.Top,
});
```

### Tree 树形控件

> DefaultProperty: `Items`，DefaultEvent: `SelectChanged`

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `Items` | 树节点集合 | TreeItem[] | [] |
| `SelectItem` | 当前选中项 | TreeItem? | null |
| `Checkable` | 显示复选框 | bool | false |
| `CheckStrictly` | 父子节点独立选择 | bool | true |
| `Multiple` | 多选 | bool | false |
| `BlockNode` | 节点占满整行 | bool | false |
| `Draggable` | 可拖拽节点 | bool | true |
| `DragHandleVisible` | 显示拖拽手柄 | bool | true |
| `VirtualMode` | 虚拟模式 | bool | false |
| `Gap` | 节点间距 | int | 8 |
| `GapIndent` | 子级缩进 | int? | null |
| `Radius` | 圆角 | int | 6 |
| `EmptyText` | 空数据提示文字 | string? | null |
| `PauseLayout` | 暂停布局 | bool | false |

```csharp
var tree = new AntdUI.Tree { Checkable = true, Multiple = false };

var root = new AntdUI.TreeItem("根节点") { Expand = true, ImageSvg = AntdUI.SvgDb.FolderOutlined };
var child1 = new AntdUI.TreeItem("子节点1") { ImageSvg = AntdUI.SvgDb.FileOutlined };
var child2 = new AntdUI.TreeItem("子节点2") { ImageSvg = AntdUI.SvgDb.FileOutlined };
var grandChild = new AntdUI.TreeItem("孙节点") { ImageSvg = AntdUI.SvgDb.FileOutlined };
child2.Add(grandChild);
root.Add(child1);
root.Add(child2);
tree.Items.Add(root);

tree.SelectChanged   += (s, e) => Console.WriteLine(e.Item?.Text);
tree.CheckedChanged  += (s, e) => { /* e.Item, e.Checked */ };
```

---

## 六、反馈控件（Feedback）

### Alert 警告提示

> DefaultProperty: `Text`，DefaultEvent: `Click`

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `Text` | 内容文字 | string? | null |
| `TextTitle` | 标题 | string? | null |
| `Type` | 类型 | TType | Info |
| `ShowIcon` | 显示图标 | bool | false |
| `Closable` | 可关闭 | bool | false |
| `CloseText` | 关闭按钮文字 | string? | null |
| `TextAlign` | 文字对齐 | ContentAlignment | MiddleLeft |

```csharp
var alert = new AntdUI.Alert
{
    TextTitle = "提示",
    Text = "请完善您的个人信息以获得更好的体验",
    Type = AntdUI.TType.Info,
    ShowIcon = true,
    Closable = true,
};
```

### Drawer 抽屉

**Drawer.Config 配置属性：**

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `Form` | 所属窗口 | Form | 必填 |
| `Content` | 内容控件 | Control | 必填 |
| `Align` | 弹出方向 | TAlignMini | Right |
| `Mask` | 显示遮罩 | bool | true |
| `MaskClosable` | 点击遮罩关闭 | bool | true |
| `Padding` | 内边距 | int | 24 |
| `ColorScheme` | 颜色方案 | TAMode | Auto |
| `Dispose` | 关闭后释放控件 | bool | true |
| `ManualActivateParent` | 手动激活父窗口 | bool | false |
| `Tag` | 自定义数据 | object? | null |

```csharp
// 右侧抽屉
var content = new Panel { Width = 400 };
// ... 构建 content 内容
AntdUI.Drawer.open(new AntdUI.Drawer.Config(this, content)
{
    Align = AntdUI.TAlignMini.Right,
    Mask = true,
    MaskClosable = true,
    Padding = 24,
});

// 底部抽屉
AntdUI.Drawer.open(new AntdUI.Drawer.Config(this, content2)
{
    Align = AntdUI.TAlignMini.Bottom,
    Padding = 16,
});
```

### Message 全局消息

```csharp
// 各类型消息
AntdUI.Message.info(this, "普通消息");
AntdUI.Message.success(this, "操作成功");
AntdUI.Message.warn(this, "注意事项");
AntdUI.Message.error(this, "操作失败");

// Loading 消息（手动关闭）
string key = AntdUI.Message.loading(this, "正在处理，请稍候...");
await DoWorkAsync();
AntdUI.Message.close(key);

// 自定义配置
AntdUI.Message.open(new AntdUI.Message.Config(this, "自定义图标消息")
{
    Icon = AntdUI.TType.Success,
    Duration = 3000,  // ms，0 = 不自动关闭
});
```

### Modal 对话框

**Modal.Config 配置属性：**

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `Target` | 所属目标（推荐）| Control | 必填 |
| `Title` | 标题 | string? | null |
| `Content` | 内容（string/Control）| object | 必填 |
| `Width` | 宽度 | int | 416 |
| `ContentPadding` | 内容区内边距 | Size | 0, 0 |
| `OkText` | 确认按钮文字 | string | "确定" |
| `CancelText` | 取消按钮文字 | string | "取消" |
| `OkType` | 确认按钮类型 | TTypeMini | Primary |
| `CancelType` | 取消按钮类型 | TTypeMini | Default |
| `Keyboard` | 支持 ESC 关闭 | bool | true |
| `Mask` | 显示遮罩 | bool | true |
| `MaskClosable` | 点击遮罩关闭 | bool | true |
| `CloseIcon` | 显示关闭图标 | bool | false |
| `ColorScheme` | 颜色方案 | TAMode | Auto |
| `Font` | 内容字体 | Font? | null |
| `DefaultFocus` | 默认焦点到确认 | bool | false |
| `DefaultAcceptButton` | 回车触发确认 | bool | true |
| `Icon` | 内置图标 | TType | None |

```csharp
// ① 基础信息
AntdUI.Modal.open(new AntdUI.Modal.Config(this, "提示", "操作完成！")
{
    Icon = AntdUI.TType.Success,
});

// ② 危险确认
var result = AntdUI.Modal.open(new AntdUI.Modal.Config(this, "删除确认",
    "确定要删除该记录吗？此操作不可恢复。")
{
    OkText = "删除",
    OkType = AntdUI.TTypeMini.Error,
    Icon = AntdUI.TType.Warn,
});
if (result == DialogResult.OK) DeleteRecord();

// ③ 自定义控件内容
var form = BuildEditForm();
await AntdUI.Modal.openAsync(new AntdUI.Modal.Config(this, "编辑信息", form)
{
    Width = 600,
    ContentPadding = Size.Empty,
    CloseIcon = true,
    OkText = "保存",
});
```

### Notification 通知提醒

**Notification.Config 配置属性：**

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `ID` | 唯一标识（用于更新）| string? | null |
| `Target` | 所属目标 | Control | 必填 |
| `Title` | 标题 | string? | null |
| `Text` | 内容文字 | string? | null |
| `Icon` | 图标类型 | TType | None |
| `IconCustom` | 自定义图标 | IconInfo? | null |
| `Placement` | 显示位置 | TAlignFrom | TR |
| `Duration` | 自动关闭时间(ms) | int | 4500（0=不关闭）|
| `Font` | 字体 | Font? | null |
| `FontTitle` | 标题字体 | Font? | null |
| `ColorScheme` | 颜色方案 | TAMode | Auto |

```csharp
// 各类型通知
AntdUI.Notification.open(new AntdUI.Notification.Config(this, "成功", "文件上传完成")
{
    Icon = AntdUI.TType.Success,
    Placement = AntdUI.TAlignFrom.TR,
});
AntdUI.Notification.open(new AntdUI.Notification.Config(this, "失败", "连接服务器超时")
{
    Icon = AntdUI.TType.Error,
    Duration = 0,  // 不自动关闭
});
```

### Popover 气泡卡片

```csharp
// 方式一：静态挂载（设计器友好）
var popover = new AntdUI.Popover
{
    Title = "快捷说明",
    Content = "点击此按钮可以执行 XXX 操作",
    Placement = AntdUI.TAlignFrom.Top,
    Trigger = AntdUI.Trigger.Hover,
};
AntdUI.Popover.SetPopover(myButton, popover);

// 方式二：代码控制显示/隐藏
AntdUI.Popover.Show(myButton, "提示内容", AntdUI.TAlignFrom.Bottom);
```

### Spin 加载中

```csharp
// 全窗体遮罩
AntdUI.Spin.open(this);
AntdUI.Spin.open(this, "正在加载数据...");
AntdUI.Spin.close(this);

// 作为嵌入控件
var spin = new AntdUI.Spin
{
    Dock = DockStyle.Fill,
    Spinning = true,
    Tip = "数据处理中",
};

// 局部区域遮罩
AntdUI.Spin.open(panelContent, "加载中...");
AntdUI.Spin.close(panelContent);
```

### Skeleton 骨架屏

> DefaultEvent: `Click`

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `Active` | 动画效果 | bool | false |
| `Avatar` | 显示头像占位 | bool | false |
| `AvatarSize` | 头像大小 | int | 40 |
| `Rows` | 文字行数 | int | 3 |
| `Round` | 圆角文字占位 | bool | false |

```csharp
var skeleton = new AntdUI.Skeleton
{
    Dock = DockStyle.Fill,
    Active = true,
    Avatar = true,
    Rows = 4,
    Round = false,
};
// 数据加载完成后隐藏
skeleton.Visible = false;
realContent.Visible = true;
```

---

## 七、窗体与其他

### BaseForm 基础窗体

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `AutoHandDpi` | 自动处理 DPI | bool | true |
| `Dark` | 深色模式 | bool | false |
| `Mode` | 颜色模式 | TAMode | Auto |
| `IsMax` | 是否最大化 | bool | false |
| `IsFull` | 是否全屏 | bool | false |
| `DisableTheme` | 禁用主题 | bool | false |

### Tabs 标签页容器

> DefaultProperty: `Pages`，DefaultEvent: `SelectedIndexChanged`

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `Alignment` | 标签位置 | TabAlignment | Top |
| `Centered` | 居中显示 | bool | false |
| `TextCenter` | 文字居中（Left/Right时）| bool | false |
| `Gap` | 间距 | int | 8 |
| `Fill` | 背景色 | Color? | null |
| `FillHover` | 悬停背景 | Color? | null |
| `FillActive` | 激活背景 | Color? | null |
| `TypExceed` | 超出 UI 类型 | TabTypExceed | Button |
| `EnableSwitch` | 允许切换 | bool | true |
| `EnablePageScrolling` | 滚轮切换页 | bool | true |
| `EnablePageCloseByMouseMiddle` | 鼠标中键关闭 | bool | true |
| `EnablePageCloseByMouseDoubleClick` | 双击关闭 | bool | true |
| `CloseDisposePage` | 关闭后释放页 | bool | false |

```csharp
var tabs = new AntdUI.Tabs { Dock = DockStyle.Fill, Alignment = TabAlignment.Top };

var page1 = new AntdUI.TabPage("概览")
{
    ImageSvg = AntdUI.SvgDb.HomeOutlined,
};
page1.Controls.Add(dashboardPanel);

var page2 = new AntdUI.TabPage("设置") { Closable = true };
page2.Controls.Add(settingsPanel);

tabs.Pages.Add(page1);
tabs.Pages.Add(page2);

tabs.SelectedIndexChanged += (s, e) => Console.WriteLine($"切换: {tabs.SelectedTab?.Text}");
tabs.PageClosing += (s, e) => { e.Cancel = !ConfirmClose(); };
```

### Panel 面板容器

| 属性 | 说明 | 类型 | 默认值 |
|:---|:---|:---|:---|
| `Radius` | 圆角 | int | 0 |
| `Shadow` | 阴影深度 | int | 0 |
| `ShadowOpacity` | 阴影透明度 | float | 0.1F |
| `ShadowColor` | 阴影颜色 | Color? | null |
| `Back` | 背景色 | Color? | null |
| `BorderWidth` | 边框宽度 | float | 0F |
| `BorderColor` | 边框颜色 | Color? | null |

```csharp
var card = new AntdUI.Panel
{
    Radius = 8,
    Shadow = 4,
    ShadowOpacity = 0.12F,
    Back = Color.White,
    Padding = new Padding(16),
};
```

---

## 八、SVG 图标速查（SvgDb 常用）

| 类别 | 常用图标名（Outlined / Filled）|
|:---|:---|
| 方向/导航 | `ArrowUpOutlined`, `ArrowDownOutlined`, `ArrowLeftOutlined`, `ArrowRightOutlined`, `LeftOutlined`, `RightOutlined` |
| 通用操作 | `PlusOutlined`, `MinusOutlined`, `CloseOutlined`, `CheckOutlined`, `SearchOutlined` |
| 数据操作 | `EditOutlined`, `DeleteOutlined`, `CopyOutlined`, `ScissorOutlined`, `SaveOutlined` |
| 文件 | `FileOutlined`, `FilePdfOutlined`, `FileExcelOutlined`, `FolderOutlined`, `FolderOpenOutlined` |
| 上传下载 | `UploadOutlined`, `DownloadOutlined`, `ExportOutlined`, `ImportOutlined` |
| 用户 | `UserOutlined`, `UserAddOutlined`, `TeamOutlined`, `LockOutlined`, `UnlockOutlined` |
| 系统 | `SettingOutlined`, `HomeOutlined`, `AppstoreOutlined`, `MenuOutlined`, `BellOutlined` |
| 状态 | `CheckCircleOutlined`, `CloseCircleOutlined`, `ExclamationCircleOutlined`, `InfoCircleOutlined`, `WarningOutlined` |
| 多媒体 | `PictureOutlined`, `CameraOutlined`, `VideoCameraOutlined`, `AudioOutlined` |
| 日期时间 | `CalendarOutlined`, `ClockCircleOutlined`, `ScheduleOutlined` |
| 图表 | `BarChartOutlined`, `LineChartOutlined`, `PieChartOutlined`, `DotChartOutlined` |
| 邮件消息 | `MailOutlined`, `MessageOutlined`, `NotificationOutlined`, `CommentOutlined` |

```csharp
// 按钮图标
btn.ImageSvg = AntdUI.SvgDb.PlusOutlined;

// 使用自定义 SVG 字符串
btn.ImageSvg = @"<svg viewBox='0 0 24 24' xmlns='http://www.w3.org/2000/svg'><path d='...' /></svg>";
```

---

## 九、枚举类型完整参考

| 枚举 | 值 | 用途 |
|:---|:---|:---|
| `TMode` | `Light`, `Dark`, `Auto` | 全局颜色模式 |
| `TAMode` | `Auto`, `Light`, `Dark` | 控件级颜色方案 |
| `TTypeMini` | `Default`, `Primary`, `Success`, `Warning`, `Error`, `Info` | 按钮/控件类型 |
| `TType` | `None`, `Success`, `Info`, `Warn`, `Error` | 状态/通知类型 |
| `TShape` | `Default`, `Round`, `Circle` | 控件形状 |
| `TShapeProgress` | `Default`, `Circle`, `Round`, `Mini`, `Steps` | Progress 形状 |
| `TAlign` | `Left`, `Center`, `Right` | 水平对齐 |
| `TAlignMini` | `Top`, `Bottom`, `Left`, `Right` | 四方向 |
| `TAlignFrom` | `TL`, `TR`, `BL`, `BR` | 弹出位置 |
| `TFit` | `Fill`, `None`, `Contain`, `Cover` | 图片填充 |
| `TAutoSize` | `None`, `Auto`, `Width`, `Height` | 自动尺寸 |
| `TVariant` | `Outlined`, `Borderless`, `Filled`, `Underline` | Input 变体 |
| `TOrientation` | `None`, `Left`, `Right` | 分割线文字方向 |
| `TEditMode` | `None`, `Click`, `DoubleClick` | Table 编辑触发 |
| `TEditSelection` | `None`, `All`, `Start`, `End` | 编辑默认选中 |
| `TEditInputStyle` | `Default`, `Borderless` | 编辑框样式 |
| `Trigger` | `Click`, `Hover` | 触发方式 |
| `TMenuMode` | `Inline`, `Horizontal`, `Vertical` | Menu 布局 |
| `TFocusMode` | `None`, `Line`, `Dot` | Menu 焦点样式 |
| `ColumnsMode` | `Auto`, `None`, `Fill`, `Header` | Table 列宽模式 |
| `TColorFormat` | `Hex`, `Rgb`, `Hsl`, `Hsb` | 颜色格式 |
| `TStepStatus` | `Wait`, `Process`, `Finish`, `Error` | 步骤状态 |
| `TButtonDisplayStyle` | `Default`, `Text`, `Image` | 按钮显示内容 |
| `TabAlignment` | `Top`, `Bottom`, `Left`, `Right` | Tabs 方向 |
| `TabTypExceed` | `Button`, `Scroll` | Tabs 超出样式 |
