# AntdUI 常用场景代码模板

---

## 1. 完整登录窗口

```csharp
// LoginForm.cs（继承 BaseForm）
public partial class LoginForm : AntdUI.BaseForm
{
    private AntdUI.Input inputUser;
    private AntdUI.Input inputPwd;
    private AntdUI.Checkbox cbRemember;
    private AntdUI.Button btnLogin;

    public LoginForm()
    {
        InitializeComponent();
        BuildUI();
    }

    void BuildUI()
    {
        Size = new Size(400, 500);
        Text = "用户登录";

        var panel = new AntdUI.Panel
        {
            Dock = DockStyle.Fill,
            Radius = 0,
            Padding = new Padding(40),
        };

        var title = new AntdUI.Label
        {
            Text = "欢迎登录",
            Font = new Font("微软雅黑", 18F, FontStyle.Bold),
            Dock = DockStyle.Top,
            Height = 50,
            TextAlign = ContentAlignment.MiddleCenter,
        };

        inputUser = new AntdUI.Input
        {
            PlaceholderText = "请输入用户名",
            PrefixSvg = AntdUI.SvgDb.UserOutlined,
            AllowClear = true,
            Dock = DockStyle.Top,
            Margin = new Padding(0, 0, 0, 12),
            Height = 40,
        };

        inputPwd = new AntdUI.Input
        {
            PlaceholderText = "请输入密码",
            PrefixSvg = AntdUI.SvgDb.LockOutlined,
            UseSystemPasswordChar = true,
            Dock = DockStyle.Top,
            Margin = new Padding(0, 0, 0, 12),
            Height = 40,
        };

        cbRemember = new AntdUI.Checkbox
        {
            Text = "记住密码",
            Dock = DockStyle.Top,
            Margin = new Padding(0, 0, 0, 16),
        };

        btnLogin = new AntdUI.Button
        {
            Text = "登 录",
            Type = AntdUI.TTypeMini.Primary,
            Dock = DockStyle.Top,
            Height = 44,
            Shape = AntdUI.TShape.Round,
        };
        btnLogin.Click += BtnLogin_Click;

        panel.Controls.AddRange(new Control[]
        {
            btnLogin, cbRemember, inputPwd, inputUser, title
        });
        Controls.Add(panel);
    }

    private async void BtnLogin_Click(object sender, EventArgs e)
    {
        // 验证
        if (string.IsNullOrWhiteSpace(inputUser.Text))
        {
            inputUser.Status = AntdUI.TType.Error;
            AntdUI.Message.warn(this, "请输入用户名");
            return;
        }
        inputUser.Status = AntdUI.TType.None;

        if (string.IsNullOrWhiteSpace(inputPwd.Text))
        {
            inputPwd.Status = AntdUI.TType.Error;
            AntdUI.Message.warn(this, "请输入密码");
            return;
        }
        inputPwd.Status = AntdUI.TType.None;

        // 登录
        btnLogin.Loading = true;
        try
        {
            bool ok = await AuthService.LoginAsync(inputUser.Text, inputPwd.Text);
            if (ok)
            {
                AntdUI.Message.success(this, "登录成功");
                new MainForm().Show();
                Hide();
            }
            else
            {
                AntdUI.Message.error(this, "用户名或密码错误");
            }
        }
        finally
        {
            btnLogin.Loading = false;
        }
    }
}
```

---

## 2. 主窗口布局（侧边菜单 + 内容区）

```csharp
public partial class MainForm : AntdUI.BaseForm
{
    private AntdUI.Menu menu;
    private AntdUI.WindowBar windowBar;
    private Panel panelContent;

    public MainForm()
    {
        InitializeComponent();
        BuildLayout();
    }

    void BuildLayout()
    {
        Size = new Size(1200, 800);
        MinimumSize = new Size(900, 600);

        // 标题栏
        windowBar = new AntdUI.WindowBar
        {
            Dock = DockStyle.Top,
            Height = 48,
            ShowIcon = true,
            Text = "管理系统",
        };

        // 左侧菜单
        menu = new AntdUI.Menu
        {
            Dock = DockStyle.Left,
            Width = 220,
            Unique = true,
            Indent = true,
        };
        BuildMenu();
        menu.SelectChanged += Menu_SelectChanged;

        // 右侧内容区
        panelContent = new Panel
        {
            Dock = DockStyle.Fill,
            Padding = new Padding(16),
        };

        Controls.Add(panelContent);
        Controls.Add(menu);
        Controls.Add(windowBar);
    }

    void BuildMenu()
    {
        menu.Items.Add(new AntdUI.MenuItem("首页") { ImageSvg = AntdUI.SvgDb.HomeOutlined, Tag = "home" });

        var dataMenu = new AntdUI.MenuItem("数据管理") { ImageSvg = AntdUI.SvgDb.DatabaseOutlined };
        dataMenu.Sub.Add(new AntdUI.MenuItem("用户列表") { Tag = "users" });
        dataMenu.Sub.Add(new AntdUI.MenuItem("订单管理") { Tag = "orders" });
        menu.Items.Add(dataMenu);

        var sysMenu = new AntdUI.MenuItem("系统设置") { ImageSvg = AntdUI.SvgDb.SettingOutlined };
        sysMenu.Sub.Add(new AntdUI.MenuItem("基础配置") { Tag = "config" });
        sysMenu.Sub.Add(new AntdUI.MenuItem("权限管理") { Tag = "roles" });
        menu.Items.Add(sysMenu);
    }

    void Menu_SelectChanged(object sender, AntdUI.MenuSelectEventArgs e)
    {
        panelContent.Controls.Clear();
        Control page = e.Item.Tag switch
        {
            "home"   => new HomePage(),
            "users"  => new UsersPage(),
            "orders" => new OrdersPage(),
            _        => new Label { Text = "页面建设中..." },
        };
        page.Dock = DockStyle.Fill;
        panelContent.Controls.Add(page);
    }
}
```

---

## 3. 数据表格 CRUD 页面

```csharp
public class UsersPage : UserControl
{
    private AntdUI.Table table;
    private AntdUI.Input searchInput;
    private AntdUI.Button btnAdd, btnRefresh;
    private AntdUI.Pagination pagination;

    private List<User> allData = new();
    private int pageSize = 20;

    public UsersPage()
    {
        BuildUI();
        LoadData(1, pageSize);
    }

    void BuildUI()
    {
        // 顶部工具栏
        var toolbar = new AntdUI.StackPanel
        {
            Dock = DockStyle.Top,
            Height = 52,
            Gap = 8,
            Padding = new Padding(0, 8, 0, 8),
        };

        searchInput = new AntdUI.Input
        {
            Width = 240,
            PlaceholderText = "搜索用户名...",
            SuffixSvg = AntdUI.SvgDb.SearchOutlined,
            AllowClear = true,
        };
        searchInput.TextChanged += (s, e) => DoSearch();

        btnRefresh = new AntdUI.Button
        {
            Text = "刷新",
            ImageSvg = AntdUI.SvgDb.ReloadOutlined,
        };
        btnRefresh.Click += (s, e) => LoadData(pagination.Current, pageSize);

        btnAdd = new AntdUI.Button
        {
            Text = "新增用户",
            Type = AntdUI.TTypeMini.Primary,
            ImageSvg = AntdUI.SvgDb.PlusOutlined,
        };
        btnAdd.Click += BtnAdd_Click;

        toolbar.Controls.AddRange(new Control[] { searchInput, btnRefresh, btnAdd });

        // 表格
        table = new AntdUI.Table
        {
            Dock = DockStyle.Fill,
            Bordered = true,
            FixedHeader = true,
        };
        table.Columns = new AntdUI.ColumnCollection
        {
            new AntdUI.Column("Id", "ID") { Width = 70, Align = AntdUI.TAlign.Center },
            new AntdUI.Column("Name", "姓名") { Width = 120 },
            new AntdUI.Column("Email", "邮箱") { Width = 200 },
            new AntdUI.Column("Role", "角色") { Width = 100 },
            new AntdUI.Column("Status", "状态") { Width = 90, Align = AntdUI.TAlign.Center },
            new AntdUI.Column("Action", "操作") { Width = 160, Fixed = true },
        };
        table.CellButtonClick += Table_CellButtonClick;

        // 分页
        pagination = new AntdUI.Pagination
        {
            Dock = DockStyle.Bottom,
            Total = 0,
            PageSize = pageSize,
            ShowSizeChanger = true,
            ShowTotal = true,
        };
        pagination.PageChanged += (s, e) => LoadData(e.Current, e.PageSize);

        Controls.Add(table);
        Controls.Add(pagination);
        Controls.Add(toolbar);
    }

    async void LoadData(int page, int size)
    {
        AntdUI.Spin.open(this, "加载中...");
        try
        {
            var result = await UserService.GetPageAsync(page, size);
            table.DataSource = result.Items;
            pagination.Total = result.Total;
        }
        finally
        {
            AntdUI.Spin.close(this);
        }
    }

    void DoSearch()
    {
        string kw = searchInput.Text.Trim();
        table.DataSource = allData
            .Where(u => string.IsNullOrEmpty(kw) || u.Name.Contains(kw))
            .ToList();
    }

    async void BtnAdd_Click(object sender, EventArgs e)
    {
        var editForm = new UserEditForm();
        var result = await AntdUI.Modal.openAsync(
            new AntdUI.Modal.Config(FindForm(), "新增用户", editForm)
            {
                Width = 500,
                OkText = "保存",
                CloseIcon = true,
            }
        );
        if (result == DialogResult.OK)
        {
            await UserService.CreateAsync(editForm.GetModel());
            LoadData(1, pageSize);
            AntdUI.Message.success(FindForm(), "用户创建成功");
        }
    }

    async void Table_CellButtonClick(object sender, AntdUI.TableButtonEventArgs e)
    {
        var user = (User)e.Record;
        if (e.Button.Text == "编辑")
        {
            var editForm = new UserEditForm(user);
            var result = await AntdUI.Modal.openAsync(
                new AntdUI.Modal.Config(FindForm(), "编辑用户", editForm)
                { Width = 500, OkText = "保存" }
            );
            if (result == DialogResult.OK)
            {
                await UserService.UpdateAsync(editForm.GetModel());
                LoadData(pagination.Current, pageSize);
            }
        }
        else if (e.Button.Text == "删除")
        {
            var confirm = AntdUI.Modal.open(
                new AntdUI.Modal.Config(FindForm(), "确认删除", $"确定删除用户 {user.Name} 吗？")
                {
                    OkType = AntdUI.TTypeMini.Error,
                    OkText = "删除",
                    Icon = AntdUI.TType.Warn,
                }
            );
            if (confirm == DialogResult.OK)
            {
                await UserService.DeleteAsync(user.Id);
                LoadData(pagination.Current, pageSize);
                AntdUI.Notification.open(new AntdUI.Notification.Config(FindForm(), "删除成功", $"用户 {user.Name} 已删除")
                {
                    Icon = AntdUI.TType.Success,
                });
            }
        }
    }
}
```

---

## 4. 带验证的表单

```csharp
public class UserEditForm : UserControl
{
    private AntdUI.Input inputName, inputEmail;
    private AntdUI.Select selectRole;
    private AntdUI.Switch swStatus;
    private AntdUI.DatePicker dpBirthday;
    private AntdUI.InputNumber numAge;

    public UserEditForm(User? user = null)
    {
        BuildForm();
        if (user != null) FillData(user);
    }

    void BuildForm()
    {
        var layout = new AntdUI.GridPanel
        {
            Dock = DockStyle.Fill,
            Column = 2,
            Gap = 16,
            Padding = new Padding(16),
        };

        // 姓名
        layout.Controls.Add(CreateField("姓名 *", inputName = new AntdUI.Input
        {
            PlaceholderText = "请输入姓名",
        }));

        // 邮箱
        layout.Controls.Add(CreateField("邮箱 *", inputEmail = new AntdUI.Input
        {
            PlaceholderText = "请输入邮箱",
        }));

        // 角色
        selectRole = new AntdUI.Select { PlaceholderText = "请选择角色" };
        selectRole.Items.Add(new AntdUI.SelectItem("管理员", "admin"));
        selectRole.Items.Add(new AntdUI.SelectItem("普通用户", "user"));
        layout.Controls.Add(CreateField("角色 *", selectRole));

        // 生日
        layout.Controls.Add(CreateField("生日", dpBirthday = new AntdUI.DatePicker
        {
            Format = "yyyy-MM-dd",
        }));

        // 年龄
        layout.Controls.Add(CreateField("年龄", numAge = new AntdUI.InputNumber
        {
            Minimum = 1,
            Maximum = 120,
        }));

        // 状态
        layout.Controls.Add(CreateField("启用状态", swStatus = new AntdUI.Switch
        {
            Checked = true,
            CheckedText = "启用",
            UnCheckedText = "禁用",
        }));

        Controls.Add(layout);
    }

    Panel CreateField(string label, Control ctrl)
    {
        var p = new Panel { Height = 72 };
        var lbl = new AntdUI.Label
        {
            Text = label,
            Dock = DockStyle.Top,
            Height = 24,
        };
        ctrl.Dock = DockStyle.Bottom;
        ctrl.Height = 36;
        p.Controls.Add(ctrl);
        p.Controls.Add(lbl);
        return p;
    }

    void FillData(User user)
    {
        inputName.Text  = user.Name;
        inputEmail.Text = user.Email;
        selectRole.Value = user.Role;
        dpBirthday.Value = user.Birthday;
        numAge.Value = user.Age;
        swStatus.Checked = user.IsActive;
    }

    public bool Validate()
    {
        bool ok = true;
        if (string.IsNullOrWhiteSpace(inputName.Text))
        {
            inputName.Status = AntdUI.TType.Error;
            ok = false;
        }
        else inputName.Status = AntdUI.TType.None;

        if (!IsValidEmail(inputEmail.Text))
        {
            inputEmail.Status = AntdUI.TType.Error;
            ok = false;
        }
        else inputEmail.Status = AntdUI.TType.None;

        if (selectRole.Value == null)
        {
            // Select 验证（通过 Status 属性）
            ok = false;
        }
        return ok;
    }

    public User GetModel() => new User
    {
        Name     = inputName.Text,
        Email    = inputEmail.Text,
        Role     = (string)selectRole.Value,
        Birthday = dpBirthday.Value,
        Age      = (int)numAge.Value,
        IsActive = swStatus.Checked,
    };

    static bool IsValidEmail(string email) =>
        System.Text.RegularExpressions.Regex.IsMatch(email,
            @"^[^@\s]+@[^@\s]+\.[^@\s]+$");
}
```

---

## 5. 实时搜索与 Table 过滤

```csharp
// 客户端本地过滤
searchInput.TextChanged += (s, e) =>
{
    string kw = searchInput.Text.Trim().ToLower();
    if (string.IsNullOrEmpty(kw))
        table.DataSource = originalData;
    else
        table.DataSource = originalData
            .Where(item => item.Name.ToLower().Contains(kw)
                        || item.Code.ToLower().Contains(kw))
            .ToList();
};

// 服务端搜索（防抖）
System.Windows.Forms.Timer searchTimer = new() { Interval = 400 };
searchTimer.Tick += async (s, e) =>
{
    searchTimer.Stop();
    await LoadData(1, pageSize, searchInput.Text);
};
searchInput.TextChanged += (s, e) =>
{
    searchTimer.Stop();
    searchTimer.Start();
};
```

---

## 6. 文件上传与进度显示

```csharp
var upload = new AntdUI.Upload
{
    Accept = "所有文件|*.*",
    Multiple = false,
};

var progress = new AntdUI.Progress
{
    Value = 0,
    Shape = AntdUI.TShapeProgress.Round,
    ShowInTaskbar = true,
    Visible = false,
};

upload.FileSelected += async (s, e) =>
{
    var file = e.Files[0];
    progress.Visible = true;
    progress.Value = 0;

    var msgKey = AntdUI.Message.loading(mainForm, $"上传 {file.Name}...");
    try
    {
        await UploadService.UploadAsync(file.FullName, percent =>
        {
            // 更新进度（需在 UI 线程）
            this.Invoke(() => progress.Value = percent);
        });
        AntdUI.Message.close(msgKey);
        AntdUI.Notification.open(new AntdUI.Notification.Config(mainForm, "上传成功", file.Name)
        {
            Icon = AntdUI.TType.Success,
        });
    }
    catch (Exception ex)
    {
        AntdUI.Message.close(msgKey);
        AntdUI.Message.error(mainForm, $"上传失败: {ex.Message}");
    }
    finally
    {
        progress.Visible = false;
    }
};
```

---

## 7. Tree 文件浏览器

```csharp
var tree = new AntdUI.Tree
{
    Dock = DockStyle.Left,
    Width = 220,
    Draggable = false,
};

void LoadDirectory(string path)
{
    tree.Items.Clear();
    var root = new AntdUI.TreeItem(Path.GetFileName(path))
    {
        Expand = true,
        ImageSvg = AntdUI.SvgDb.FolderOpenOutlined,
        Tag = path,
    };
    LoadSubItems(root, path);
    tree.Items.Add(root);
}

void LoadSubItems(AntdUI.TreeItem parent, string dirPath)
{
    foreach (var dir in Directory.GetDirectories(dirPath))
    {
        var item = new AntdUI.TreeItem(Path.GetFileName(dir))
        {
            ImageSvg = AntdUI.SvgDb.FolderOutlined,
            Tag = dir,
        };
        parent.Add(item);
    }
    foreach (var file in Directory.GetFiles(dirPath))
    {
        var item = new AntdUI.TreeItem(Path.GetFileName(file))
        {
            ImageSvg = AntdUI.SvgDb.FileOutlined,
            Tag = file,
        };
        parent.Add(item);
    }
}

tree.SelectChanged += (s, e) =>
{
    string path = (string)e.Item.Tag;
    if (File.Exists(path))
        OpenFile(path);
    else if (e.Item.Children.Count == 0)
        LoadSubItems(e.Item, path);
};
```

---

## 8. 主题切换（深色/浅色）

```csharp
var sw = new AntdUI.Switch
{
    CheckedText = "🌙 深色",
    UnCheckedText = "☀️ 浅色",
    Checked = false,
};
sw.CheckedChanged += (s, e) =>
{
    AntdUI.Config.Mode = sw.Checked ? AntdUI.TMode.Dark : AntdUI.TMode.Light;
    // 继承 BaseForm 的窗口自动刷新，无需手动处理
};
```

---

## 9. AOT 安全的 Table 数据绑定

```csharp
// AOT 兼容：使用委托绑定而非反射
[DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicProperties)]
public class UserDto
{
    public int Id { get; set; }
    public string Name { get; set; } = "";
    public string Email { get; set; } = "";
}

// 或使用 Column.GetValue 委托（100% AOT 安全）
table.Columns = new AntdUI.ColumnCollection
{
    new AntdUI.Column("Id", "ID")
    {
        Width = 60,
        GetValue = row => ((UserDto)row).Id,
    },
    new AntdUI.Column("Name", "姓名")
    {
        Width = 120,
        GetValue = row => ((UserDto)row).Name,
        SetValue = (row, val) => ((UserDto)row).Name = (string)val,
    },
};
```

---

## 10. 跨线程 UI 更新模板

```csharp
// 安全更新 UI 的通用方法
void SafeInvoke(Action action)
{
    if (InvokeRequired) Invoke(action);
    else action();
}

// 后台任务示例
Task.Run(async () =>
{
    for (int i = 0; i <= 100; i++)
    {
        await Task.Delay(50);
        SafeInvoke(() =>
        {
            progress.Value = i;
            label.Text = $"进度: {i}%";
        });
    }
    SafeInvoke(() =>
    {
        AntdUI.Message.success(this, "处理完成！");
    });
});
```
