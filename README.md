# 邓晓平 · 个人主页（GitHub Pages）

单页静态站点，纯 HTML + CSS + 少量原生 JS，无构建步骤、无外部依赖（图标为内联 SVG）。**中英文双语**，右上角切换。

```
homepage/
├── index.html            中文主页（默认首页，所有区块通过锚点导航）
├── en.html               英文主页（与 index.html 一一对应，见「六、中英文双语」）
├── styles.css            全部样式（两页共用）
├── images/
│   ├── profile.jpg       头像（从简历中提取）
│   └── ….png / ….jpg     项目配图（8 张，见下）
├── files/
│   └── cv.pdf            简历 PDF（供页面「下载简历」按钮使用）
└── README.md
```

> 两个页面**共用同一套 `styles.css`、`images/`、`files/`**，锚点 id 也完全一致（`#about`、`#projects`…），所以切换语言时能停在同一个区块。

---

## 一、部署到 GitHub Pages

> **本项目实际参数**（下文示例均按此填写）：
> - 用户名：`XiaopingDeng`
> - 仓库名：`XiaopingDeng.github.io`
> - 目标地址：<https://XiaopingDeng.github.io>
> - 认证方式：**HTTPS + Personal Access Token**（不用 SSH）

### 方式 A：用户主页（推荐，地址形如 `https://用户名.github.io`）

#### A-1. 在网页端新建仓库

1. 打开 <https://github.com/new>。
2. **Repository name** 填 `XiaopingDeng.github.io` —— 必须严格等于 `<用户名>.github.io`，否则用户主页不生效。
3. **Public** 必须选公开（私有仓库的 Pages 需要付费账号）。
4. **不要**勾选 `Add a README file`、`.gitignore`、`license` —— 保持**空仓库**，否则首次 push 会因远端有提交而冲突。
5. 点 **Create repository**。

#### A-2. 生成 Personal Access Token（classic）

SSH 因公钥未注册到 GitHub 而报 `Permission denied (publickey)`，改用 token 最省事：

1. 打开 <https://github.com/settings/tokens> → **Generate new token** → **Generate new token (classic)**。
2. **Note** 随便填（如 `homepage-push`）；**Expiration** 建议选 90 天或自定义。
3. **Scopes** 勾 **`repo`**（整块勾上，含 `repo:status`、`public_repo` 等子项）—— 这是 push 代码到仓库的最小必需权限。
4. 拉到底点 **Generate token**，页面顶部会显示一串 `ghp_…`。
5. **立刻复制保存**（只显示这一次），例如存到本地密码管理器。

#### A-3. 提交并推送

已在本机 `homepage/` 目录里初始化过 git 的，只需确认 remote 用 HTTPS：

```bash
cd /e/邓晓平/CVS/web/homepage

# 确认/切换为 HTTPS（SSH 写法 git@github.com:... 换成 https://github.com/...）
git remote set-url origin https://github.com/XiaopingDeng/XiaopingDeng.github.io.git
git remote -v          # 应显示 https://github.com/XiaopingDeng/XiaopingDeng.github.io.git

git push -u origin main
```

还没初始化过的，从零走一遍：

```bash
git init
git add .
git commit -m "add personal homepage"
git branch -M main
git remote add origin https://github.com/XiaopingDeng/XiaopingDeng.github.io.git
git push -u origin main
```

push 时会弹出凭据输入框（或提示 `Username for 'https://github.com':`）：

| 提示项 | 填什么 |
| --- | --- |
| `Username for 'https://github.com':` | `XiaopingDeng` |
| `Password for 'https://XiaopingDeng@github.com':` | **粘贴刚才那串 `ghp_…` token**（不是 GitHub 登录密码） |

> 若误弹到「浏览器登录」窗口，直接关掉，选终端里手动输入凭据的方式。
> token 想要免输，可执行 `git config --global credential.helper manager` 让它记住；不设也不影响 push，每次粘贴一遍即可。

#### A-4. 开启 Pages 并访问

1. 推送成功后进仓库 **Settings → Pages**。
2. **Source** 选 `Deploy from a branch`。
3. **Branch** 选 `main`，目录选 `/ (root)`，点 **Save**。
4. 等待 1—2 分钟（首次构建稍慢），访问 <https://XiaopingDeng.github.io> 即可。

### 方式 B：项目子路径（地址形如 `https://用户名.github.io/仓库名/`）

1. 新建任意名称的仓库（如 `homepage`），把文件放到根目录并按上面 A-2、A-3 的方式推送。
2. 仓库 **Settings → Pages** → Source 选 `Deploy from a branch` → Branch 选 `main` + `/ (root)` → Save。
3. 访问 `https://XiaopingDeng.github.io/<仓库名>/`。

### 常见报错对照

| 报错 | 原因 | 处理 |
| --- | --- | --- |
| `git@github.com: Permission denied (publickey).` | 用的是 SSH，且本机公钥没注册到该账号 | 换成 HTTPS：`git remote set-url origin https://github.com/XiaopingDeng/XiaopingDeng.github.io.git` |
| `fatal: repository '…' not found` | 仓库还没在网页端创建，或名字/大小写不一致 | 先去 <https://github.com/new> 建 `XiaopingDeng.github.io` |
| `remote: Support for password authentication was removed` | Password 填了 GitHub 登录密码 | Password 处改填 `ghp_…` token |
| `! [rejected] main -> main (fetch first)` | 建库时勾了 README，远端已有提交 | `git pull --rebase origin main` 后再 push；或删库重建空仓库 |
| 404 访问不到主页 | 仓库名不是 `<用户名>.github.io`，或 Pages 未开启 | 核对仓库名，并到 Settings → Pages 确认 Branch 为 `main` + `/ (root)` |

> 本页所有链接（`styles.css`、`images/profile.jpg`、`files/cv.pdf`、页内锚点）均为相对路径，因此**方式 A 和方式 B 都能直接工作**，无需修改。

---

## 二、日常维护（只改内容，不用碰样式）

> **注意：站内所有内容都有中英两份。** 下表里的「改哪里」指中文版 `index.html`；同样的改动要在英文版 `en.html` 的对应位置再做一遍。因为两页结构完全一致（区块、id、class、图片文件名都相同），所以是「照着同一个小节改两遍」，不涉及任何样式调整。英文全文另有一份对照说明，见「六、中英文双语」。

| 想改什么 | 改哪里 |
| --- | --- |
| 姓名、职称、单位 | `index.html` 顶部 `<!-- 顶部介绍 -->` 区块（英文版在 `en.html` 同处，英文姓名放 `<h1>`、中文名放 `<h1 class="name-en">`） |
| 语言切换控件 | 顶部导航条末尾的 `<div class="lang-switch">`；两页各一个，**只改 href 不要改 id**（`id="langLink"` 供脚本保留锚点用） |
| 中／英文页面互链 | 两页 `<head>` 里的 `<link rel="alternate" hreflang="zh-CN" …>` 与 `hreflang="en"`；换域名时要一起改 |
| 姓名、职称、单位 | `index.html` 顶部 `<!-- 顶部介绍 -->` 区块 |
| 个人简介段落 | 区块 `<!-- 关于我 -->` 的第一、二段 |
| 教育与工作经历 | 同区块的 `<ul class="timeline">` |
| 最新动态 | 区块 `<!-- 最新动态 -->` 的 `news-list`，建议按时间倒序，每条固定 `<span class="when">年月</span><span class="what">内容</span>` 两段 |
| 科研项目 | 区块 `<!-- 科研项目 -->`，表格里一行一个项目 |
| 论文 | 区块 `<!-- 论文与专利成果 -->` 的 `pub-list`，编号自动生成 |
| 论文标签 | `<span class="tag sci">SCI</span>` / `tag ei` / `tag core` |
| 教学与奖励 | 区块 `<!-- 教学与奖励 -->`。其中「指导研究生」两张卡片（在读 / 已毕业）与「主讲课程」段落、`历年教学工作量` 折叠表均在此 |
| 指导研究生名单 | 该区块 `.card-grid` 内两张 `.card`，在读按年级降序、已毕业按年级降序，格式为 `姓名（年级，研究方向，去向）`，用 `<br>` 分隔 |
| 主讲课程 | 该区块第一段 `<p class="pub-meta">`，本科 / 研究生 / 公选课分号分隔 |
| 联系方式 | 区块 `<!-- 联系我们 -->` |
| 换头像 | 直接替换 `images/profile.jpg`（建议正方形、480×480 以上） |
| 换简历 PDF | 直接替换 `files/cv.pdf` |
| 社交主页链接 | `index.html` 中的 `github.com/XiaopingDeng`、`gitee.com/XiaopingDeng`、`space.bilibili.com/291197097`，顶部图标区与「联系我们」各有一处 |
| 换项目配图 | 替换 `images/` 下同名文件即可，无需改 HTML；若要增删图或改图注，见下面「三、项目配图」 |

### 项目配图位置对照

页面里 8 张项目照片**各自内嵌在对应项目条目的描述容器内**（`figure-grid` 图集组件）：

| 图片文件 | 归属项目条目 | 图注主体 |
| --- | --- | --- |
| `面向大型办公建筑的非侵入式负荷监测系统与技术.png` | 主持项目 · 面向大型办公建筑的非侵入式负荷监测系统与技术 | 非侵入式负荷监测自研监测终端 |
| `全频段及重点频段混合侦测电路设计-硬件.jpg` | 主持项目 · 全频段及重点频段混合侦测电路设计及 PCB Layout | 测向天线阵列与硬件联试 |
| `全频段及重点频段混合侦测电路设计-软件.png` | 同上 | 无人机测向设备监控平台 |
| `超高速可见光通信终端项目-硬件.png` | 主持项目 · 超高速可见光通信终端项目 | ALINX 自研基带处理板 |
| `超高速可见光通信终端项目-系统.png` | 同上（`class="tall"` 竖图） | 系统联调现场 |
| `超高速可见光通信终端项目-测试软件.png` | 同上 | OFDM／QPSK 上位机软件 |
| `绿色智能建造和建筑工业化关键技术与成套装备.png` | 参与项目 · 山东省重点研发计划（重大科技创新工程） | 预制构件生产质量检测系统 |
| `高速数据接收机（HDR）.png` | 折叠区「早期型号与预研项目」· 高速数据接收机（HDR） | HDR 原理样机联试环境 |

---

## 三、项目配图（figure-grid 图集）

项目照片用 `.figure-grid` 组件排版：桌面端自动按 180px 最小宽排成多列，窄屏自动降为单列，点击任意图片在新标签页打开原图。

**每个图集内嵌在它所对应的那个项目条目里**（主管项目在表格行 `<td>` 内，参与项目/工程项目在 `<li>` 内），不再集中堆在表格之后。图注说明写在 `figcaption` 中。

### 换图（最常用）

**直接替换 `images/` 下的同名文件**，不用动 HTML。建议：

- **尺寸：任意比例均可完整显示，图片不会被裁切。** 横图建议 ≥ 900px 宽，比例 16:10 或 4:3 排版最匀称
- **竖长图**（如 `超高速可见光通信终端项目-系统.png`，1600×2133）给 `<figure>` 加 `class="tall"`，会限高 400px 且不拉伸变形
- **跨整行的大图**给 `<figure>` 加 `class="wide"`，会占满整行
- 体积：单张控制在 300 KB 以内，可显著加快首屏加载

### 改图注

在 `index.html` 里找到对应 `<figure>`，改 `<figcaption>` 文字：

```html
<figure>
    <a href="images/xxx.png" target="_blank" rel="noopener">
        <img src="images/xxx.png" alt="图片说明" loading="lazy">
    </a>
    <figcaption><b>加粗标题</b><br>第二行补充说明</figcaption>
</figure>
```

`figcaption` 里 `<b>` 会显示为深色加粗，`<br>` 换行，后面接常规灰色小字。

> 图注是文字，所以**中英各有一份**：英文图注在 `en.html` 里同样位置的那个 `<figure>` 中；图片文件同名共用，但 `alt` 也各写各的。

### 增删图 / 整块新增图集

- **增删单张图**：在一个 `figure-grid` 内增删 `<figure>` 块即可，栅格会自动重排。
- **新增一整块图集**：在目标项目中粘一个完整的 `<div class="figure-grid">…</div>`（放在该项目自己的 `<td>` 或 `<li>` 内）。
- **跨整行的大图**：给 `<figure>` 加 `class="wide"`，例如 `<figure class="wide">`，该图会占满整行。
- **竖长图**：给 `<figure>` 加 `class="tall"`，例如 `<figure class="tall">`，限高 400px 居中显示。

### 其他

- **`alt` 一定要写**，既是无障碍要求，也是图片加载失败时的兜底文字。
- **不要用 `object-fit: cover` 配固定高度** —— 那会把图裁掉。当前样式是 `img { width: 100%; height: auto }`，整张图完整显示。
- **打印样式**已单独处理：打印时图片自然高度、禁止跨页断开，Ctrl/Cmd+P 直接出干净纸质版。

---

## 四、可选调整

**1. 公开手机号**
`index.html` 的「联系我们」区块里有一段被注释掉的 Mobile 卡片，去掉 `<!--` 和 `-->` 即可显示。手机号直接暴露在公网会被爬虫抓取，默认关闭。

**2. 加上 Google Scholar / 其他学术主页图标**
顶部介绍区的 `.social-icons` 现有 6 个图标：邮件、单位、简历 PDF、GitHub、Gitee、哔哩哔哩。若要再加 Google Scholar、ORCID 等，仿照现有 `<a><svg>…</svg></a>` 结构追加一个即可（图标为内联 SVG，无需引入图标库）。

**3. 配色**
`styles.css` 顶部 `:root` 中：

```css
--accent: #f4a261;       /* 主强调色（暖橙） */
--accent-deep: #e76f51;  /* 链接色 */
```

改成任意色值即可整体换肤。

**4. 字体**
`index.html` 里通过 Google Fonts 加载 Poppins 与 Noto Sans SC。若国内访问较慢或无法加载，浏览器会自动回落到 `styles.css` 中已写好的系统字体栈（PingFang SC / 微软雅黑 等），版面不会走形。也可以直接删掉那两行 `<link>` 走纯系统字体。

**5. 本地预览**
直接双击 `index.html` 即可。若要用本地服务器：

```bash
cd homepage
python -m http.server 8000
# 浏览器打开 http://localhost:8000 
```

---

## 五、无障碍与移动端

- 已适配手机（760px 断点）、平板与桌面，导航栏在窄屏自动换行；语言切换控件在 804px 以下会落到导航下方居中。
- 已提供 `@media print` 打印样式，直接 Ctrl/Cmd+P 可生成干净纸质版。
- 所有图片带 `alt`，所有纯图标链接带 `aria-label`。

---

## 六、中英文双语

站点是**两个文件、一套样式**，不是运行时翻译：

| 文件 | 语言 | 地址 |
| --- | --- | --- |
| `index.html` | 中文（`<html lang="zh-CN">`） | `https://XiaopingDeng.github.io/` |
| `en.html` | 英文（`<html lang="en">`） | `https://XiaopingDeng.github.io/en.html` |

### 为什么用两个文件而不是 JavaScript 换字

两者都要「改一处、动两遍」，所以维护成本相同；但双文件方案额外拿到：不依赖 JS（关掉脚本英文页照样正常）、英文页有自己的 `<html lang="en">` 与英文 `<meta>`（利于搜索引擎与屏幕阅读器）、可直接 Ctrl+P 打印英文版、英文页 URL 可以单独发给外方合作者。

### 切换控件

顶部吸顶导航条末尾的 `.lang-switch`：

```html
<div class="lang-switch" role="group" aria-label="语言切换 / Language">
    <span class="is-current" lang="zh-CN">中文</span>
    <a href="en.html" id="langLink" data-href="en.html" hreflang="en" lang="en" title="Switch to English">EN</a>
</div>
```

- 当前语言用 `<span class="is-current">`（橙底白字，不可点），另一种语言用 `<a>`。英文页里两者对调。
- **摆放方式：与导航链接同处一个 flex 行**，靠 `flex` 分配宽度，所以两者永远不会重叠——`.banner` 是 `display:flex; justify-content:center; gap:16px`，`.navbar { flex:1 1 auto }`（在扣掉切换控件后的剩余宽度里居中）、`.lang-switch { flex:0 0 auto }`。**不要**把 `.lang-switch` 改成 `position:absolute` 贴右端：导航是按整幅宽度居中的，英文标签更宽时会直接压到切换控件上（本站早期版本就是这个 bug）。
- **≤804px 时落到导航下方居中**（`@media (max-width: 804px)`：`.banner` 允许换行、`.navbar` 占满一行、`.lang-switch` 加一点上边距）。804 这个数不是随手写的：实测英文导航要**单行**放下需要可视宽度 ≥ 805px，门槛取低了会留下「导航已折两行、切换控件悬在两行缝隙里」的中间状态。
- 打印时隐藏。
- **`id="langLink"` 与 `data-href` 是脚本钩子**：页面底部脚本会把当前区块锚点接到链接后面，于是「在科研项目处点 EN」会跳到 `en.html#projects` 而不是回到页首。删掉这两个属性只是失去该体验，不会报错。

### 新增／修改内容的规则

两页必须**保持结构一致**：区块顺序、`<section id>`、class、`<figure>` 顺序、图片文件名、`<details>` 数量都要对得上；只有「人看的文字」不同。英文页里 `<i>…</i>` 比中文页多，是因为英文按学术惯例把期刊名、课程名排成斜体，这是正常的文字层差异。

改完可以用脚本自查两页标签数是否一致（任选一种）：

```python
import re, io
def c(p):
    s = io.open(p, encoding='utf-8').read()
    return {t: (len(re.findall(r'<'+t+r'[\s>/]', s)), len(re.findall(r'</'+t+r'>', s)))
              for t in ['section','div','ul','ol','li','details','table','tr','td','figure','figcaption','h2','h3','p']}
a, b = c('index.html'), c('en.html')
for t in a:
    print(t, a[t], b[t], 'OK' if a[t] == b[t] else '<<< 不一致')
```

两点已知的「假警报」，不必修：`<a>` 在两页都会报「开口比闭口多 1」（多出来的那个在「联系我们」被注释掉的卡片里）；`<i>` 两页数量本来就不同（见上）。

### 英文页特有的两个排版注意点

1. **单位名称很长**，Intro 右侧 `.school-info` 因此不能设 `flex-shrink: 0`——否则会把整行顶出页面产生横向滚动条。现在是 `flex: 0 1 auto; max-width: 34%`，长名称会自动折行。
2. 英文名 `Xiaoping Deng` 比「邓晓平」宽，窄屏（约 780px 以下）`<h1>` 会折成两行，属正常表现。

> 想改成「访问英文浏览器自动跳英文页」也可以做，但默认**不做**：自动跳转容易把中文用户从首页踢走，也不利于搜索引擎收录，且一旦跳转规则写错会来回弹。需要时再说。

