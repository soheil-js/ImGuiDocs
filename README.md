# Hexa.NET.ImGui — راهنمای کامل Widgetها و UI

> راهنمای کاربردی برای استفاده از **Hexa.NET.ImGui** در C# با تمرکز روی ساخت UIهای مدرن، Debug Tool، Overlay و ابزارهای داخلی.
>
> **نکته:** امضای بعضی متدها ممکن است بین نسخه‌های Hexa.NET.ImGui کمی متفاوت باشد. اگر IntelliSense نسخه شما امضای متفاوتی نشان داد، همان overload نسخه نصب‌شده را مبنا قرار دهید.

---

## فهرست مطالب

1. [مفاهیم پایه](#1-مفاهیم-پایه)
2. [Text](#2-text)
3. [Button](#3-button)
4. [Checkbox و RadioButton](#4-checkbox-و-radiobutton)
5. [Input](#5-input)
6. [Slider](#6-slider)
7. [Drag](#7-drag)
8. [Combo و Selectable](#8-combo-و-selectable)
9. [Color](#9-color)
10. [Tree و CollapsingHeader](#10-tree-و-collapsingheader)
11. [Layout](#11-layout)
12. [Child Window](#12-child-window)
13. [Tabs](#13-tabs)
14. [Tables](#14-tables)
15. [ProgressBar](#15-progressbar)
16. [Plot](#16-plot)
17. [Image و ImageButton](#17-image-و-imagebutton)
18. [Menu](#18-menu)
19. [Popup و Modal](#19-popup-و-modal)
20. [Tooltip](#20-tooltip)
21. [Disabled State](#21-disabled-state)
22. [Style](#22-style)
23. [Font](#23-font)
24. [Window](#24-window)
25. [ID و ##](#25-id-و-)
26. [State و مدیریت داده](#26-state-و-مدیریت-داده)
27. [یک پنل کامل](#27-یک-پنل-کامل)
28. [الگوهای کاربردی Debug/Overlay](#28-الگوهای-کاربردی-debugoverlay)
29. [Cheat Sheet](#29-cheat-sheet)

---

# 1. مفاهیم پایه

Dear ImGui یک UI فوری یا **Immediate Mode GUI** است.

برخلاف UIهای سنتی، معمولاً لازم نیست برای هر کنترل یک Object دائمی بسازید. در هر Frame کد UI اجرا می‌شود و وضعیت مورد نیاز را خود برنامه نگه می‌دارد.

ساختار کلی:

```csharp
public static void Draw()
{
    ImGui.Begin("My Window");

    ImGui.Text("Hello World");

    if (ImGui.Button("Click"))
    {
        Console.WriteLine("Clicked");
    }

    ImGui.End();
}
```

تقریباً همیشه باید این قانون را رعایت کنید:

```text
Begin(...)
    Widget
    Widget
    Widget
End(...)
```

برای APIهای Begin/End نیز معمولاً:

```csharp
if (ImGui.BeginSomething(...))
{
    // content
}

ImGui.EndSomething();
```

استفاده می‌شود.

---

# 2. Text

## Text

برای نمایش متن ساده:

```csharp
ImGui.Text("Hello World");
```

مثال:

```csharp
ImGui.Text("Player Information");
ImGui.Text("Health: 100");
ImGui.Text("Armor: 75");
```

## TextColored

برای متن رنگی:

```csharp
ImGui.TextColored(
    new Vector4(1f, 0f, 0f, 1f),
    "ERROR"
);
```

`Vector4` معمولاً به شکل زیر است:

```text
R G B A
```

مقادیر بین `0` و `1` هستند.

مثلاً:

```csharp
new Vector4(1, 0, 0, 1) // Red
new Vector4(0, 1, 0, 1) // Green
new Vector4(0, 0, 1, 1) // Blue
new Vector4(1, 1, 1, 1) // White
```

## TextDisabled

برای متن کم‌رنگ:

```csharp
ImGui.TextDisabled("This option is unavailable.");
```

## TextWrapped

برای متن طولانی:

```csharp
ImGui.TextWrapped(
    "This is a long text that will automatically wrap based on the available width."
);
```

## LabelText

Label و Value:

```csharp
ImGui.LabelText("Username", "Soheil");
```

## BulletText

```csharp
ImGui.BulletText("First");
ImGui.BulletText("Second");
ImGui.BulletText("Third");
```

---

# 3. Button

## Button

```csharp
if (ImGui.Button("Click Me"))
{
    Console.WriteLine("Clicked!");
}
```

نکته مهم:

`Button()` مقدار `true` را فقط در Frameای برمی‌گرداند که کاربر روی آن کلیک کرده است.

مثال:

```csharp
if (ImGui.Button("Connect"))
{
    Connect();
}
```

## SmallButton

```csharp
if (ImGui.SmallButton("X"))
{
    Close();
}
```

## InvisibleButton

برای ساخت کنترل کاملاً سفارشی:

```csharp
if (ImGui.InvisibleButton(
    "CustomArea",
    new Vector2(100, 40)))
{
    Console.WriteLine("Clicked");
}
```

این Widget خودش ظاهر خاصی ندارد و بیشتر برای کنترل‌های Custom استفاده می‌شود.

## ArrowButton

```csharp
if (ImGui.ArrowButton("Previous", ImGuiDir.Left))
{
    Previous();
}
```

جهت‌ها:

```csharp
ImGuiDir.Left
ImGuiDir.Right
ImGuiDir.Up
ImGuiDir.Down
```

---

# 4. Checkbox و RadioButton

## Checkbox

```csharp
bool enabled = false;

ImGui.Checkbox("Enabled", ref enabled);
```

مثال واقعی:

```csharp
bool esp = false;

if (ImGui.Checkbox("ESP", ref esp))
{
    Console.WriteLine($"ESP: {esp}");
}
```

چون مقدار با `ref` ارسال می‌شود، ImGui می‌تواند آن را تغییر دهد.

## RadioButton

برای انتخاب یک گزینه از چند گزینه:

```csharp
int selected = 0;

if (ImGui.RadioButton("Easy", selected == 0))
{
    selected = 0;
}

if (ImGui.RadioButton("Normal", selected == 1))
{
    selected = 1;
}

if (ImGui.RadioButton("Hard", selected == 2))
{
    selected = 2;
}
```

---

# 5. Input

## InputText

برای متن:

```csharp
string name = "Soheil";

ImGui.InputText(
    "Name",
    ref name,
    100
);
```

## InputTextWithHint

برای Placeholder:

```csharp
string username = "";

ImGui.InputTextWithHint(
    "Username",
    "Enter username...",
    ref username,
    100
);
```

## InputTextMultiline

برای TextArea:

```csharp
string code = "";

ImGui.InputTextMultiline(
    "Code",
    ref code,
    500,
    new Vector2(500, 200)
);
```

برای Editorها و Debug Toolها بسیار مناسب است.

## InputInt

```csharp
int health = 100;

ImGui.InputInt(
    "Health",
    ref health
);
```

## InputFloat

```csharp
float speed = 1.5f;

ImGui.InputFloat(
    "Speed",
    ref speed
);
```

## InputDouble

```csharp
double value = 10.5;

ImGui.InputDouble(
    "Value",
    ref value
);
```

## InputFloat2 / 3 / 4

برای Vector:

```csharp
float[] position = { 10f, 20f, 30f };

ImGui.InputFloat3(
    "Position",
    ref position[0]
);
```

خانواده:

```text
InputFloat
InputFloat2
InputFloat3
InputFloat4

InputInt
InputInt2
InputInt3
InputInt4
```

---

# 6. Slider

Slider برای انتخاب مقدار در یک محدوده است.

## SliderFloat

```csharp
float volume = 0.5f;

ImGui.SliderFloat(
    "Volume",
    ref volume,
    0f,
    1f
);
```

## SliderInt

```csharp
int health = 100;

ImGui.SliderInt(
    "Health",
    ref health,
    0,
    100
);
```

## SliderFloat3

برای Vector:

```csharp
float[] position = { 10f, 20f, 30f };

ImGui.SliderFloat3(
    "Position",
    ref position[0],
    0f,
    100f
);
```

خانواده:

```text
SliderFloat
SliderFloat2
SliderFloat3
SliderFloat4

SliderInt
SliderInt2
SliderInt3
SliderInt4
```

## VSliderFloat

Slider عمودی:

```csharp
float value = 50f;

ImGui.VSliderFloat(
    "Volume",
    new Vector2(30, 150),
    ref value,
    0f,
    100f
);
```

---

# 7. Drag

Drag شبیه Slider است، ولی برای تغییر سریع مقدار با Drag کردن طراحی شده است.

## DragFloat

```csharp
float speed = 5f;

ImGui.DragFloat(
    "Speed",
    ref speed,
    0.1f,
    0f,
    100f
);
```

## DragInt

```csharp
int value = 50;

ImGui.DragInt(
    "Value",
    ref value,
    1f,
    0,
    100
);
```

خانواده:

```text
DragFloat
DragFloat2
DragFloat3
DragFloat4

DragInt
DragInt2
DragInt3
DragInt4

DragScalar
DragScalarN
```

### چه زمانی Slider و چه زمانی Drag؟

تقریباً:

```text
Slider -> وقتی کاربر باید محدوده را به صورت واضح ببیند
Drag   -> وقتی تغییر سریع مقدار مهم‌تر است
Input  -> وقتی مقدار دقیق باید وارد شود
```

---

# 8. Combo و Selectable

## Combo

Dropdown ساده:

```csharp
string[] difficulties =
{
    "Easy",
    "Normal",
    "Hard"
};

int selected = 0;

ImGui.Combo(
    "Difficulty",
    ref selected,
    difficulties,
    difficulties.Length
);
```

## BeginCombo

وقتی کنترل بیشتری می‌خواهید:

```csharp
string current = "AK-47";

if (ImGui.BeginCombo("Weapon", current))
{
    if (ImGui.Selectable("AK-47"))
    {
        current = "AK-47";
    }

    if (ImGui.Selectable("M4A1"))
    {
        current = "M4A1";
    }

    if (ImGui.Selectable("AWP"))
    {
        current = "AWP";
    }

    ImGui.EndCombo();
}
```

مزیت `BeginCombo` این است که داخل Dropdown می‌توانید Widgetهای دیگری هم قرار دهید.

## Selectable

```csharp
if (ImGui.Selectable("Player 1"))
{
    Console.WriteLine("Player 1 selected");
}
```

مثال:

```csharp
string[] players =
{
    "Player 1",
    "Player 2",
    "Player 3"
};

foreach (string player in players)
{
    if (ImGui.Selectable(player))
    {
        Console.WriteLine($"Selected: {player}");
    }
}
```

---

# 9. Color

## ColorEdit3

RGB:

```csharp
Vector3 color = new(1f, 0f, 0f);

ImGui.ColorEdit3(
    "Color",
    ref color
);
```

## ColorEdit4

RGBA:

```csharp
Vector4 color = new(1f, 0f, 0f, 1f);

ImGui.ColorEdit4(
    "Color",
    ref color
);
```

## ColorPicker3

```csharp
Vector3 color = new(1f, 0f, 0f);

ImGui.ColorPicker3(
    "Color",
    ref color
);
```

## ColorPicker4

```csharp
Vector4 color = new(1f, 0f, 0f, 1f);

ImGui.ColorPicker4(
    "Color",
    ref color
);
```

تفاوت اصلی:

```text
ColorEdit -> کنترل فشرده‌تر
ColorPicker -> انتخاب رنگ کامل‌تر
3 -> RGB
4 -> RGBA
```

---

# 10. Tree و CollapsingHeader

## TreeNode

برای ساخت Tree:

```csharp
if (ImGui.TreeNode("Players"))
{
    ImGui.Text("Player 1");
    ImGui.Text("Player 2");
    ImGui.Text("Player 3");

    ImGui.TreePop();
}
```

ساختار:

```text
▼ Players
    Player 1
    Player 2
    Player 3
```

## Tree چند مرحله‌ای

```csharp
if (ImGui.TreeNode("Game"))
{
    if (ImGui.TreeNode("Players"))
    {
        ImGui.Text("Player 1");
        ImGui.Text("Player 2");

        ImGui.TreePop();
    }

    if (ImGui.TreeNode("Weapons"))
    {
        ImGui.Text("AK-47");
        ImGui.Text("M4A1");
        ImGui.Text("AWP");

        ImGui.TreePop();
    }

    ImGui.TreePop();
}
```

## CollapsingHeader

برای Section:

```csharp
if (ImGui.CollapsingHeader("Settings"))
{
    ImGui.Text("Settings Content");
}
```

برای UIهای Debug بسیار کاربردی است:

```text
▼ Player
▼ Visual
▼ Aimbot
▼ Misc
```

---

# 11. Layout

## SameLine

قرار دادن Widgetها کنار هم:

```csharp
ImGui.Button("Save");

ImGui.SameLine();

ImGui.Button("Cancel");
```

نتیجه:

```text
[ Save ] [ Cancel ]
```

## Spacing

فاصله عمودی:

```csharp
ImGui.Spacing();
```

## Dummy

فضای خالی با اندازه مشخص:

```csharp
ImGui.Dummy(
    new Vector2(100, 20)
);
```

مثلاً:

```csharp
ImGui.Text("Top");

ImGui.Dummy(new Vector2(0, 20));

ImGui.Text("Bottom");
```

## Separator

```csharp
ImGui.Text("Player");

ImGui.Separator();

ImGui.Text("Health: 100");
```

## SeparatorText

برای Header:

```csharp
ImGui.SeparatorText("Player Settings");
```

## Indent

```csharp
ImGui.Indent();

ImGui.Text("Indented");

ImGui.Unindent();
```

## BeginGroup / EndGroup

برای گروه‌بندی:

```csharp
ImGui.BeginGroup();

ImGui.Text("Name");
ImGui.Button("Edit");

ImGui.EndGroup();
```

Group برای اعمال Layout یا رفتار مشترک روی چند Widget مفید است.

---

# 12. Child Window

Child یک ناحیه مستقل داخل Window است.

```csharp
if (ImGui.BeginChild(
    "PlayerList",
    new Vector2(300, 200)))
{
    for (int i = 0; i < 100; i++)
    {
        ImGui.Text($"Player {i}");
    }

    ImGui.EndChild();
}
```

یکی از کاربردهای اصلی آن Scroll Area است.

مثلاً:

```text
┌─────────────────────┐
│ Player 1            │
│ Player 2            │
│ Player 3            │
│ Player 4            │
│ Player 5            │
│ ...                 │
│                     │
└─────────────────────┘
```

---

# 13. Tabs

## BeginTabBar

```csharp
if (ImGui.BeginTabBar("MainTabs"))
{
    if (ImGui.BeginTabItem("Player"))
    {
        ImGui.Text("Player settings");

        ImGui.EndTabItem();
    }

    if (ImGui.BeginTabItem("Visual"))
    {
        ImGui.Text("Visual settings");

        ImGui.EndTabItem();
    }

    ImGui.EndTabBar();
}
```

ساختار:

```text
┌────────┬────────┬────────┐
│ Player │ Visual │ Misc   │
└────────┴────────┴────────┘
```

## BeginTabItem

هر Tab با این ساختار:

```csharp
if (ImGui.BeginTabItem("ESP"))
{
    // content

    ImGui.EndTabItem();
}
```

---

# 14. Tables

Table برای نمایش داده‌های ساختاریافته بسیار مناسب است.

```csharp
if (ImGui.BeginTable("Players", 3))
{
    ImGui.TableSetupColumn("Name");
    ImGui.TableSetupColumn("Health");
    ImGui.TableSetupColumn("Distance");

    ImGui.TableHeadersRow();

    ImGui.TableNextRow();

    ImGui.TableNextColumn();
    ImGui.Text("Player 1");

    ImGui.TableNextColumn();
    ImGui.Text("100");

    ImGui.TableNextColumn();
    ImGui.Text("25m");

    ImGui.EndTable();
}
```

نتیجه مفهومی:

```text
┌──────────┬────────┬──────────┐
│ Name     │ Health │ Distance │
├──────────┼────────┼──────────┤
│ Player 1 │ 100    │ 25m      │
└──────────┴────────┴──────────┘
```

## چند Row

```csharp
string[] names =
{
    "Player 1",
    "Player 2",
    "Player 3"
};

int[] health =
{
    100,
    75,
    42
};

if (ImGui.BeginTable("Players", 2))
{
    ImGui.TableSetupColumn("Name");
    ImGui.TableSetupColumn("Health");

    ImGui.TableHeadersRow();

    for (int i = 0; i < names.Length; i++)
    {
        ImGui.TableNextRow();

        ImGui.TableNextColumn();
        ImGui.Text(names[i]);

        ImGui.TableNextColumn();
        ImGui.Text(health[i].ToString());
    }

    ImGui.EndTable();
}
```

---

# 15. ProgressBar

```csharp
float progress = 0.75f;

ImGui.ProgressBar(
    progress,
    new Vector2(300, 20),
    "75%"
);
```

مقدار معمولاً بین:

```text
0.0 -> 0%
1.0 -> 100%
```

است.

مثال:

```csharp
float loading = 0.35f;

ImGui.ProgressBar(
    loading,
    new Vector2(-1, 0),
    "Loading..."
);
```

---

# 16. Plot

## PlotLines

```csharp
float[] values =
{
    10f,
    20f,
    15f,
    30f,
    25f,
    40f
};

ImGui.PlotLines(
    "FPS",
    ref values[0],
    values.Length
);
```

برای نمایش داده‌های متغیر مثل FPS، Ping یا مصرف منابع مفید است.

## PlotHistogram

```csharp
ImGui.PlotHistogram(
    "Memory",
    ref values[0],
    values.Length
);
```

---

# 17. Image و ImageButton

برای Image باید یک Texture/Texture ID معتبر از Backend خود داشته باشید.

مفهوم کلی:

```csharp
ImGui.Image(
    texture,
    new Vector2(128, 128)
);
```

ImageButton:

```csharp
if (ImGui.ImageButton(
    "MyImage",
    texture,
    new Vector2(64, 64)))
{
    Console.WriteLine("Clicked");
}
```

نکته مهم:

`texture` در اینجا باید با نوع Texture/ImTextureID مورد انتظار نسخه و Backend شما سازگار باشد. نحوه ساخت Texture به DirectX/OpenGL/Vulkan/SDL/Backend مورد استفاده شما وابسته است.

---

# 18. Menu

## BeginMenuBar

اگر Window شما MenuBar داشته باشد:

```csharp
if (ImGui.BeginMenuBar())
{
    if (ImGui.BeginMenu("File"))
    {
        if (ImGui.MenuItem("Open"))
        {
        }

        if (ImGui.MenuItem("Save"))
        {
        }

        if (ImGui.MenuItem("Exit"))
        {
        }

        ImGui.EndMenu();
    }

    ImGui.EndMenuBar();
}
```

ساختار:

```text
File
 ├── Open
 ├── Save
 └── Exit
```

## MenuItem

```csharp
if (ImGui.MenuItem("Reset Settings"))
{
    ResetSettings();
}
```

---

# 19. Popup و Modal

## OpenPopup

```csharp
if (ImGui.Button("Delete"))
{
    ImGui.OpenPopup("Confirm Delete");
}
```

بعد:

```csharp
if (ImGui.BeginPopupModal("Confirm Delete"))
{
    ImGui.Text("Are you sure?");

    if (ImGui.Button("Yes"))
    {
        Delete();
        ImGui.CloseCurrentPopup();
    }

    ImGui.SameLine();

    if (ImGui.Button("No"))
    {
        ImGui.CloseCurrentPopup();
    }

    ImGui.EndPopup();
}
```

Popup برای Context Menu، Dialog و کنترل‌های موقت کاربرد دارد.

---

# 20. Tooltip

## SetTooltip

```csharp
ImGui.Text("Health");

if (ImGui.IsItemHovered())
{
    ImGui.SetTooltip(
        "Current player health"
    );
}
```

## SetItemTooltip

در APIهایی که این helper را دارند:

```csharp
ImGui.Button("ESP");

ImGui.SetItemTooltip(
    "Enable player ESP"
);
```

Tooltip همیشه مربوط به آخرین Item مناسب است.

---

# 21. Disabled State

برای غیرفعال کردن چند Widget:

```csharp
bool connected = false;

ImGui.BeginDisabled(!connected);

ImGui.Button("Send");

ImGui.InputText(
    "Message",
    ref message,
    200
);

ImGui.EndDisabled();
```

اگر `connected == false` باشد، کنترل‌ها Disabled هستند.

---

# 22. Style

Dear ImGui اجازه می‌دهد Style را به صورت Global یا موقت تغییر دهید.

## PushStyleColor

مثلاً متن قرمز:

```csharp
ImGui.PushStyleColor(
    ImGuiCol.Text,
    new Vector4(1, 0, 0, 1)
);

ImGui.Text("Danger");

ImGui.PopStyleColor();
```

مهم:

هر `PushStyleColor` باید با `PopStyleColor` متعادل شود.

## PushStyleVar

مثلاً Rounding:

```csharp
ImGui.PushStyleVar(
    ImGuiStyleVar.FrameRounding,
    10f
);

ImGui.Button("Rounded");

ImGui.PopStyleVar();
```

## چند Style همزمان

```csharp
ImGui.PushStyleColor(
    ImGuiCol.Button,
    new Vector4(0.2f, 0.5f, 1f, 1f)
);

ImGui.PushStyleVar(
    ImGuiStyleVar.FrameRounding,
    8f
);

ImGui.Button("Custom");

ImGui.PopStyleVar();
ImGui.PopStyleColor();
```

ترتیب Pop بهتر است معکوس Push باشد.

---

# 23. Font

اگر Font مناسب در Backend شما Load شده باشد:

```csharp
ImGui.PushFont(myFont);

ImGui.Text("Large / Custom Font");

ImGui.PopFont();
```

همیشه:

```text
PushFont
    Draw
PopFont
```

---

# 24. Window

## Begin / End

ساخت Window:

```csharp
ImGui.Begin("My Window");

ImGui.Text("Hello");

ImGui.End();
```

## Window با شرط باز بودن

در APIهای رایج:

```csharp
bool open = true;

if (ImGui.Begin("My Window", ref open))
{
    ImGui.Text("Content");
}

ImGui.End();
```

اگر کاربر Window را ببندد:

```csharp
open == false
```

می‌شود.

## SetNextWindowSize

قبل از Begin:

```csharp
ImGui.SetNextWindowSize(
    new Vector2(600, 400)
);

ImGui.Begin("Debugger");

ImGui.Text("Content");

ImGui.End();
```

## SetNextWindowPos

```csharp
ImGui.SetNextWindowPos(
    new Vector2(100, 100)
);
```

این APIها برای کنترل اولیه Layout پنجره مفید هستند.

---

# 25. ID و ##

این قسمت برای ImGui بسیار مهم است.

اگر چند Widget Label یکسان داشته باشند، ImGui ممکن است آنها را یک Item یکسان در نظر بگیرد.

راه‌حل استفاده از `##` است.

مثلاً:

```csharp
ImGui.Button("Delete##player1");
ImGui.Button("Delete##player2");
ImGui.Button("Delete##player3");
```

روی UI هر سه ممکن است فقط:

```text
Delete
Delete
Delete
```

نمایش داده شوند، ولی ID داخلی آنها متفاوت است:

```text
Delete##player1
Delete##player2
Delete##player3
```

## PushID

روش دیگر:

```csharp
ImGui.PushID(1);

ImGui.Button("Delete");

ImGui.PopID();

ImGui.PushID(2);

ImGui.Button("Delete");

ImGui.PopID();
```

این مفهوم در ساخت List و Table بسیار مهم است.

---

# 26. State و مدیریت داده

ImGui خودش Application State شما را مدیریت نمی‌کند.

مثلاً:

```csharp
static bool esp;
static int health = 100;
static float fov = 90f;
static string playerName = "Soheil";
```

و در Draw:

```csharp
ImGui.Checkbox("ESP", ref esp);

ImGui.SliderInt(
    "Health",
    ref health,
    0,
    100
);

ImGui.SliderFloat(
    "FOV",
    ref fov,
    1,
    180
);

ImGui.InputText(
    "Name",
    ref playerName,
    100
);
```

این متغیرها باید بین Frameها باقی بمانند.

اشتباه:

```csharp
public void Draw()
{
    bool esp = false;

    ImGui.Checkbox("ESP", ref esp);
}
```

چون هر Frame دوباره:

```text
esp = false
```

می‌شود.

بهتر:

```csharp
private bool _esp;
```

و:

```csharp
public void Draw()
{
    ImGui.Checkbox("ESP", ref _esp);
}
```

---

# 27. یک پنل کامل

در این مثال چند Widget را با هم ترکیب می‌کنیم.

```csharp
using System.Numerics;

public static class DebugPanel
{
    private static bool _esp;
    private static bool _godMode;
    private static bool _bunnyHop;

    private static int _health = 100;

    private static float _fov = 90f;
    private static float _smooth = 5f;

    private static string _playerName = "Soheil";

    private static Vector4 _espColor =
        new(0f, 1f, 0f, 1f);

    public static void Draw()
    {
        ImGui.Begin("Game Debugger");

        if (ImGui.BeginTabBar("MainTabs"))
        {
            DrawPlayerTab();
            DrawVisualTab();
            DrawAimbotTab();
            DrawMiscTab();

            ImGui.EndTabBar();
        }

        ImGui.End();
    }

    private static void DrawPlayerTab()
    {
        if (!ImGui.BeginTabItem("Player"))
            return;

        ImGui.InputText(
            "Name",
            ref _playerName,
            100
        );

        ImGui.SliderInt(
            "Health",
            ref _health,
            0,
            100
        );

        ImGui.Checkbox(
            "God Mode",
            ref _godMode
        );

        ImGui.EndTabItem();
    }

    private static void DrawVisualTab()
    {
        if (!ImGui.BeginTabItem("Visual"))
            return;

        ImGui.Checkbox(
            "ESP",
            ref _esp
        );

        ImGui.ColorEdit4(
            "ESP Color",
            ref _espColor
        );

        ImGui.SliderFloat(
            "FOV",
            ref _fov,
            30f,
            180f
        );

        ImGui.EndTabItem();
    }

    private static void DrawAimbotTab()
    {
        if (!ImGui.BeginTabItem("Aimbot"))
            return;

        ImGui.SliderFloat(
            "FOV",
            ref _fov,
            1f,
            180f
        );

        ImGui.SliderFloat(
            "Smooth",
            ref _smooth,
            1f,
            20f
        );

        ImGui.EndTabItem();
    }

    private static void DrawMiscTab()
    {
        if (!ImGui.BeginTabItem("Misc"))
            return;

        ImGui.Checkbox(
            "Bunny Hop",
            ref _bunnyHop
        );

        ImGui.Separator();

        if (ImGui.Button("Apply"))
        {
            ApplySettings();
        }

        ImGui.SameLine();

        if (ImGui.Button("Reset"))
        {
            ResetSettings();
        }

        ImGui.EndTabItem();
    }

    private static void ApplySettings()
    {
        // Apply settings
    }

    private static void ResetSettings()
    {
        _esp = false;
        _godMode = false;
        _bunnyHop = false;

        _health = 100;
        _fov = 90f;
        _smooth = 5f;
    }
}
```

---

# 28. الگوهای کاربردی Debug/Overlay

## الگوی Settings

```csharp
if (ImGui.CollapsingHeader("Graphics"))
{
    bool fullscreen = true;
    bool vsync = true;
    int fps = 144;

    ImGui.Checkbox(
        "Fullscreen",
        ref fullscreen
    );

    ImGui.Checkbox(
        "VSync",
        ref vsync
    );

    ImGui.SliderInt(
        "FPS",
        ref fps,
        30,
        360
    );
}
```

---

## الگوی Player List

```csharp
if (ImGui.BeginChild(
    "Players",
    new Vector2(300, 250)))
{
    for (int i = 0; i < 20; i++)
    {
        ImGui.PushID(i);

        ImGui.Selectable(
            $"Player {i + 1}"
        );

        ImGui.PopID();
    }

    ImGui.EndChild();
}
```

---

## الگوی Table + Button

```csharp
if (ImGui.BeginTable("Players", 3))
{
    ImGui.TableSetupColumn("Name");
    ImGui.TableSetupColumn("Health");
    ImGui.TableSetupColumn("Action");

    ImGui.TableHeadersRow();

    for (int i = 0; i < 5; i++)
    {
        ImGui.PushID(i);

        ImGui.TableNextRow();

        ImGui.TableNextColumn();
        ImGui.Text($"Player {i + 1}");

        ImGui.TableNextColumn();
        ImGui.Text("100");

        ImGui.TableNextColumn();

        if (ImGui.SmallButton("Inspect"))
        {
            InspectPlayer(i);
        }

        ImGui.PopID();
    }

    ImGui.EndTable();
}
```

---

## الگوی Tooltip

```csharp
ImGui.Text("FOV");

if (ImGui.IsItemHovered())
{
    ImGui.BeginTooltip();

    ImGui.Text("Field of View");

    ImGui.Separator();

    ImGui.Text(
        "Controls the visible angle."
    );

    ImGui.EndTooltip();
}
```

---

## الگوی Help Marker

```csharp
ImGui.Text("FOV");

ImGui.SameLine();

ImGui.TextDisabled("(?)");

if (ImGui.IsItemHovered())
{
    ImGui.SetTooltip(
        "Controls the field of view."
    );
}
```

---

# 29. Cheat Sheet

## Text

```text
Text
TextColored
TextDisabled
TextWrapped
LabelText
BulletText
```

## Buttons

```text
Button
SmallButton
InvisibleButton
ArrowButton
ImageButton
```

## Boolean

```text
Checkbox
RadioButton
```

## Input

```text
InputText
InputTextWithHint
InputTextMultiline

InputInt
InputInt2
InputInt3
InputInt4

InputFloat
InputFloat2
InputFloat3
InputFloat4

InputDouble
```

## Slider

```text
SliderInt
SliderInt2
SliderInt3
SliderInt4

SliderFloat
SliderFloat2
SliderFloat3
SliderFloat4

SliderAngle

VSliderInt
VSliderFloat
```

## Drag

```text
DragInt
DragInt2
DragInt3
DragInt4

DragFloat
DragFloat2
DragFloat3
DragFloat4

DragScalar
DragScalarN
```

## Selection

```text
Combo
BeginCombo
Selectable
BeginListBox
ListBox
```

## Color

```text
ColorEdit3
ColorEdit4
ColorPicker3
ColorPicker4
```

## Tree

```text
TreeNode
TreePop
CollapsingHeader
```

## Layout

```text
SameLine
Spacing
Dummy
Separator
SeparatorText

Indent
Unindent

BeginGroup
EndGroup

BeginChild
EndChild
```

## Tabs

```text
BeginTabBar
EndTabBar

BeginTabItem
EndTabItem
```

## Tables

```text
BeginTable
EndTable

TableNextRow
TableNextColumn

TableSetupColumn
TableSetupScrollFreeze
TableHeadersRow
TableHeader
```

## Progress / Graph

```text
ProgressBar

PlotLines
PlotHistogram
```

## Images

```text
Image
ImageButton
```

## Menus

```text
BeginMenuBar
EndMenuBar

BeginMainMenuBar
EndMainMenuBar

BeginMenu
EndMenu

MenuItem
```

## Popup

```text
OpenPopup
BeginPopup
EndPopup

BeginPopupModal
EndPopup

CloseCurrentPopup
```

## Tooltip

```text
SetTooltip
SetItemTooltip
BeginTooltip
EndTooltip

IsItemHovered
```

## Window

```text
Begin
End

SetNextWindowSize
SetNextWindowPos
SetNextWindowCollapsed
SetNextWindowFocus
SetNextWindowBgAlpha
```

## State / ID

```text
PushID
PopID

PushStyleColor
PopStyleColor

PushStyleVar
PopStyleVar

PushFont
PopFont

BeginDisabled
EndDisabled
```

---

# ترتیب پیشنهادی یادگیری

اگر تازه با ImGui شروع کرده‌اید، همه APIها را همزمان حفظ نکنید.

ترتیب مناسب:

```text
1. Begin / End
2. Text
3. Button
4. Checkbox
5. InputText
6. InputInt / InputFloat
7. SliderInt / SliderFloat
8. DragFloat
9. SameLine
10. Separator
11. Combo
12. Selectable
13. CollapsingHeader
14. BeginChild
15. BeginTabBar
16. BeginTable
17. Popup
18. Tooltip
19. PushStyleColor
20. PushStyleVar
21. PushFont
22. Custom Drawing
```

بعد از این مرحله، بهتر است به جای حفظ کردن Widgetهای بیشتر، روی این موضوعات تمرکز کنید:

```text
Layout
State Management
ID Stack
Style System
Fonts
Tables
Popups
DrawList
Custom Controls
Performance
```

---

# نکته مهم درباره Hexa.NET.ImGui

`Hexa.NET.ImGui` یک Binding/Wrapper برای استفاده از Dear ImGui در اکوسیستم .NET است.

بنابراین ممکن است بین نسخه‌های مختلف Hexa.NET.ImGui:

- نام بعضی Typeها متفاوت باشد
- overloadها تغییر کرده باشند
- نوع Texture/ImTextureID متفاوت باشد
- بعضی APIهای جدید Dear ImGui تازه به Binding اضافه شده باشند
- بعضی پارامترهای pointer/reference در C# متفاوت باشند

بنابراین برای کد نهایی همیشه IntelliSense نسخه نصب‌شده و API همان نسخه را مرجع قرار دهید.

---

# جمع‌بندی

برای ساخت یک Overlay یا Debug Tool حرفه‌ای، این ترکیب بسیار قدرتمند است:

```text
Window
 ├── TabBar
 │
 ├── Player
 │    ├── InputText
 │    ├── SliderInt
 │    └── Checkbox
 │
 ├── Visual
 │    ├── Checkbox
 │    ├── ColorEdit4
 │    └── SliderFloat
 │
 ├── Players
 │    └── Table
 │
 └── Actions
      ├── Button
      └── Popup
```

با همین اجزای پایه می‌توانید UIهای کاملاً حرفه‌ای برای:

- Debugger
- Game Overlay
- Internal Tools
- Developer Tools
- Profilers
- Editors
- Configuration Panels
- Monitoring Tools

بسازید.

