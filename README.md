# 邓晓平 · 个人主页（GitHub Pages）

单页静态站点，纯 HTML + CSS + 少量原生 JS，无构建步骤、无外部依赖（图标为内联 SVG）。

```
homepage/
├── index.html            主页（唯一页面，所有区块通过锚点导航）
├── styles.css            全部样式
├── images/
│   ├── profile.jpg       头像（从简历中提取）
│   └── ….png / ….jpg     项目配图（8 张，见下）
├── files/
│   └── cv.pdf            简历 PDF（供页面「下载简历」按钮使用）
└── README.md
```

---

## 一、部署到 GitHub Pages

### 方式 A：用户主页（推荐，地址形如 `https://用户名.github.io`）

1. 在 GitHub 新建仓库，仓库名**必须**是 `<你的用户名>.github.io`（例如用户名是 `dengxiaoping`，仓库名就是 `dengxiaoping.github.io`）。
2. 把本目录（`homepage/`）里的**全部文件**放到该仓库根目录，即仓库根下直接是 `index.html`、`styles.css`、`images/`、`files/`。
3. 提交并推送：

   ```bash
   git init
   git add .
   git commit -m "add personal homepage"
   git branch -M main
   git remote add origin https://github.com/<你的用户名>/<你的用户名>.github.io.git
   git push -u origin main
   ```

4. 等待 1—2 分钟，访问 `https://<你的用户名>.github.io` 即可。

### 方式 B：项目子路径（地址形如 `https://用户名.github.io/仓库名/`）

1. 新建任意名称的仓库（如 `homepage`），把文件放到根目录并推送。
2. 仓库 **Settings → Pages** → Source 选 `Deploy from a branch` → Branch 选 `main` + `/ (root)` → Save。
3. 访问 `https://<你的用户名>.github.io/<仓库名>/`。

> 本页所有链接（`styles.css`、`images/profile.jpg`、`files/cv.pdf`、页内锚点）均为相对路径，因此**方式 A 和方式 B 都能直接工作**，无需修改。

---

## 二、日常维护（只改内容，不用碰样式）

| 想改什么 | 改哪里 |
| --- | --- |
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

页面里 8 张项目照片分别归属如下（`figure-grid` 图集组件）：

| 图片文件 | 所在区块 | 图注主体 |
| --- | --- | --- |
| `面向大型办公建筑的非侵入式负荷监测系统与技术.png` | 科研项目 → 主持（表格后第 1 个图集） | 非侵入式负荷监测自研监测终端 |
| `全频段及重点频段混合侦测电路设计-硬件.jpg` | 同上 | 测向天线阵列与硬件联试 |
| `全频段及重点频段混合侦测电路设计-软件.png` | 同上 | 无人机测向设备监控平台 |
| `超高速可见光通信终端项目-硬件.png` | 科研项目 → 主持（第 2 个图集，跨行大图） | ALINX 自研基带处理板 |
| `超高速可见光通信终端项目-系统.png` | 同上 | 系统联调现场 |
| `超高速可见光通信终端项目-测试软件.png` | 同上 | OFDM／QPSK 上位机软件 |
| `绿色智能建造和建筑工业化关键技术与成套装备.png` | 科研项目 → 参与（`<ul>` 之后） | 预制构件生产质量检测系统 |
| `高速数据接收机（HDR）.png` | 科研项目 → 折叠区「早期型号与预研项目」→ 工程项目末尾 | HDR 原理样机联试环境 |

---

## 三、项目配图（figure-grid 图集）

项目照片用 `.figure-grid` 组件排版：桌面端自动按 240px 最小宽排成多列，窄屏自动降为单列，点击任意图片在新标签页打开原图。

### 换图（最常用）

**直接替换 `images/` 下的同名文件**，不用动 HTML。建议：

- 尺寸：横图建议 ≥ 900px 宽，比例 16:10 或 4:3 效果最好（非该比例会被 `object-fit: cover` 居中裁切）
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

### 增删图 / 整块新增图集

- **增删单张图**：在一个 `figure-grid` 内增删 `<figure>` 块即可，栅格会自动重排。
- **新增一整块图集**：在目标位置粘一个完整的 `<div class="figure-grid">…</div>`。
- **跨整行的大图**：给 `<figure>` 加 `class="wide"`，例如 `<figure class="wide">`，该图会占满整行、高度 260px。

### 其他

- **`alt` 一定要写**，既是无障碍要求，也是图片加载失败时的兜底文字。
- **打印样式**已单独处理：打印时图片高度自动、最大 200px，并禁止跨页断开，Ctrl/Cmd+P 直接出干净纸质版。

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

- 已适配手机（760px 断点）、平板与桌面，导航栏在窄屏自动换行。
- 已提供 `@media print` 打印样式，直接 Ctrl/Cmd+P 可生成干净纸质版。
- 所有图片带 `alt`，所有纯图标链接带 `aria-label`。
