# 王旌旗 · 我的 VIBE 世界 — 项目交接文档 (HANDOFF)

> 本文档面向接手本项目的 AI / 开发者。它完整记录了**这个网站是什么、怎么做的、怎么部署的、当前状态、以及下一步**。
> 最后更新：2026-09-05（部署时的会话）。

---

## 1. 项目是什么

一个**单文件纯静态个人网站**，中文，清新（light）风格 + 借用 v0.app 模板 **COMPUTE**（"The Platform to Build & Ship AI Agents"）的结构与视觉 DNA。

**内容主体**
- 姓名：**王旌旗**（Wang Jingqi）
- 学校：**青岛科技大学**
- 爱好：**VIBE CODING**（喜欢"感觉/灵感驱动"，用 AI Agent 结对编程搞小项目）
- 有女朋友，网站有他俩的合照（"我们" 区）
- 其他社交：GitHub `JQJQ66`、B 站主页

**文件清单（仓库根目录）**
| 文件 | 说明 |
|---|---|
| `index.html` | **唯一源码**：内嵌全部 HTML + CSS + JS，体积 ~41KB，自包含 |
| `profile.jpg` | 本人照片（原中文名 `个人照片.jpg`，已改名） |
| `us.jpg` | 和女朋友合照（原 `我和媳妇.jpg`，已改名） |
| `README.md` | 本文档 |

> 站点没有构建流程、没有 package.json、不是任何框架项目。就是一个 `index.html` + 两张图。任何人改图片或改文案，直接改文件即可，部署是"即拷即用"。

---

## 2. 技术实现（`index.html`）

### 技术栈
- 纯 HTML5 + CSS（自定义变量/设计 token）+ 原生 JS，无框架、无打包。
- 字体走 Google Fonts（Inter / JetBrains Mono / Noto Sans SC），有系统字体兜底。
- 设计 token 集中在 `:root`：`--bg`、`--accent`(#10b981 emerald)、`--sky`、`--rose`、`--ui-bg`(#101720 深色终端)、`--radius`、`--container`(1120px) 等。

### 页面结构（自上而下）
| 区块 | id | 内容 |
|---|---|---|
| 导航 | — | 吸顶。链接：首页/认识我/VIBE 工作室/找到我/我们 + 「联系我」按钮；移动端汉堡菜单 |
| Hero | `#top` | 头衔、姓名渐变、副标题、介绍、按钮（进入我的 VIBE 世界 / 看看我和她）、chips；右侧本人头像 + 浮动卡片 + 终端打字机mockup |
| 跑马灯 | — | 滚动 tags（# VIBE CODING / # build & ship / # AI Agents…） |
| 认识我 | `#about` | 3 张卡（我是谁/我在干嘛/我热爱的事）+ **统计数字**（`#stats`） |
| 工作室 | `#studio` | **深色 COMPUTE 风**：标题"把灵感，一键 Build & Ship 成现实"，4 张特性卡 + 控制台面板（`#tabInd`） |
| 找到我 | `#links` | 2 张链接卡：GitHub、B 站 |
| 我们 | `#love` | 合照（us.jpg）+ "代码是我的热爱，而她是我的世界" + 3 条文案 |
| CTA | `#contact` | "要不要一起，做个好玩的东西？" + GitHub / B站 / 回到顶部 按钮 |
| 页脚 | — | 品牌简介、社交图标（GitHub + B站）、探索/关于栏、版权 + `built with vibe` |

### 动态效果（v0 风，已实现并验证）
- **流动渐变标题**（`@keyframes gradientFlow`，作用于 hero 姓名 `.accent`）
- **动态漂浮光斑**（`.bg-decor::before/::after` → `blobFloat`；`.avatar-blob` → `blobSpin`）
- **点阵光晕背景**（`.bg-dots`，radial dots + mask 渐隐）
- **顶部滚动进度条**（`#progressBar`，scroll 更新 width）
- **卡片聚光跟随**（`.card.spotlight`，mousemove 设置 `--mx/--my`，radial glow）
- **按钮扫光**（`.btn-accent/.btn-primary::after` → `shine`）
- **控制台标签滑动指示条**（`.console-tab-indicator` `#tabInd`，点击/初始定位移动）
- **数字滚动统计**（`#stats .num`、`.m-val[data-count]`——IntersectionObserver 触发 `runCount`，ease-out）
- **终端打字动画**（`.term-body`，`typeLine()` 逐字）
- 跑马灯、滚动显现（`.reveal/.in` + d1/d2/d3 交错）、平滑滚动、导航滚动高亮、移动端菜单

### JS 入口（都在 `index.html` 底部 `<script>`）
移动菜单 toggle、导航高亮 `onScroll`、IntersectionObserver reveal、终端 `typeLine`、进度条 `updateProgress`、标签指示 `moveInd`、计数 `runCount`+`countIO`、聚光 `mousemove`。

---

## 3. 部署全记录

站点当前同时可用在**两处**（都已验证可访问）。下面是完整链路。

### 3.1 源代码 / GitHub
- 本地 git 仓库已建，分支从 `master` 改为 **`main`**。
- 远程仓库：**`JQJQ66/wjq-site`**（公开），默认分支 `main`。
  - 远程地址：`https://github.com/JQJQ66/wjq-site.git`
  - 提交历史：`4e917ba`（初始站点）→ `6d8dd12`（加 v0 风动态效果）。
- **GitHub 认证**：本机通过 **Git Credential Manager** 存了 GitHub token（用户名 `JQJQ66`）。
  - ⚠️ **不要在文档或对话里明文粘贴该 token**。需要时用：`printf "protocol=https\nhost=github.com\n\n" | git credential fill` 读取 password。

### 3.2 GitHub Pages（已上线，稳定）
- 已开启，来源 `main` 分支 `/`（根目录），HTTPS 强制。
- 线上地址：**`https://jqjq66.github.io/wjq-site/`**
- 已验证：首页 200、图片 `profile.jpg`/`us.jpg` 均 200。
- 用于**应急 / 简历**（国内访问偏慢）。

### 3.3 腾讯云 EdgeOne Pages（已上线，国内可达，免备案）
- 项目名：**`wjq-site`**，项目 id：**`makers-xopt4qcccbuf`**，最近一次部署 id：**`dpta0iu86pbi`**。
- **构建设置**（已按静态站配置）：
  - 框架预设：`Other`
  - 根目录：`./`（界面显示 `/`）
  - 构建输出目录：`/`（仓库根）
  - 构建命令 / 安装命令：**留空**（纯静态，不构建）
  - 加速区域：**`Global (MLC excluded)` = 全球（不含中国大陆）→ 免 ICP 备案**（未备案，合规）
  - 环境变量：无
- 默认预览地址（当前可用）：**`https://wjq-site-dpta0iu86pbi.edgeone.dev`**
  - 用浏览器验证过：页面完整渲染。⚠️ curl 直连会 401（CDN 只放行浏览器请求），属正常。
- ⚠️ 环境限制：本开发环境（数据中心 IP）**连不上** `console.tencentcloud.com` / `edgeone.tencent.com`（连接被拒）。操作 EdgeOne 控制台必须用**能连腾讯云的网络 / 用户自己的浏览器**。本次是用 `browser-cdp`（agent-browser）驱动 Chrome、由用户单独登录腾讯云后操作完成的。

### 3.4 域名（阿里云）
- 域名：**`jq大帝.xyz`**（中文 IDN）。**punycode 形式：`xn--jq-mu9c51q.xyz`** —— 在后台/DNS 里必须用这个纯英文形式。
- 注册商：**阿里云**（域名实名认证已完成）。

### 3.5 Alibaba Cloud DNS 已添加的记录（在阿里云「域名解析」里）
| 主机记录 | 类型 | 记录值 | 作用 |
|---|---|---|---|
| `@` | CNAME | `jq大帝.xyz.pages.dnsoe9.com`（Ali 显示中文；punycode 即 `xn--jq-mu9c51q.xyz.pages.dnsoe9.com`） | 把域名指到 EdgeOne |
| `edgeonereclaim` | TXT | `reclaim-b9qosczhu46p7qucnpoxnbkbnyuptn5e` | 归属权验证（已完成，可删） |

> 说明：同一记录 EdgeOne 显示为 `xn--jq-mu9c51q.xyz.pages.dnsoe9.com`，阿里云显示为 `jq大帝.xyz.pages.dnsoe9.com` —— **是同一串**（punycode ↔ Unicode 显示差异）。

### 3.6 EdgeOne 域名绑定（归属权验证已通过）
- EdgeOne「Domains」已绑定：`wjq-site.edgeone.dev`（已生效）与 **`xn--jq-mu9c51q.xyz`**（关联环境 Production）。
- EdgeOne 给你的 CNAME 目标：**`xn--jq-mu9c51q.xyz.pages.dnsoe9.com`**。
- 归属权验证（TXT reclaim）：**已完成（验证成功）**。

---

## 4. 当前状态 / 未完成事项

> 截至交接时（2026-09-05）：**网站已上线并可用**。自定义域名正在最后收尾。

- ✅ 站点可在 GitHub Pages 访问：`https://jqjq66.github.io/wjq-site/`
- ✅ 站点可在 EdgeOne 预览地址访问：`https://wjq-site-dpta0iu86pbi.edgeone.dev`
- 🔄 自定义域名 `jq大帝.xyz`：DNS CNAME 已加、归属权已验证；EdgeOne 里域名状态显示**「部署中」、HTTPS「未配置」**。需等 CNAME 全网生效 + EdgeOne **自动签发 HTTPS 证书**，通常数分钟内变为**「已生效」/HTTPS 配置完成**。

**下一步（若状态仍未变）**
1. 在用户浏览器确认能连腾讯云后，登录 EdgeOne 看 `xn--jq-mu9c51q.xyz` 是否转为「已生效」；若 HTTPS 仍「未配置」，找 EdgeOne 的证书/域名模块手动触发或确认自动签发。
2. 域名生效后，用浏览器访问 `https://jq大帝.xyz`（或 `https://xn--jq-mu9c51q.xyz`）确认真实打开。
3. 可选：给阿里云再加 `www` → CNAME → 同一个目标值，让 `www.jq大帝.xyz` 也能打开。
4. 可选：删除阿里云里那条 `edgeonereclaim` 的 TXT 记录（验证已完成，不删也安全）。

---

## 5. 要改的东西 / 注意点

- **邮箱**：站点 CTA 里目前没有假邮箱（已被 GitHub / B 站按钮替换）。若想放真实邮箱，需在 `index.html` 的 `#contact` 区自行换成真实地址（`mailto:`）。
- **图片**：页面引用的是 `profile.jpg` 和 `us.jpg`（不是中文名）。任何位置想换成别的图，改 `<img src>` 即可。
- **内容**：作品集/找到我里的 GitHub、B 站地址已填真实值；若想改 B 站链接里的 `?spm_id_from=...` 或新增 Twitter/Weibo，都在 `index.html` 对应卡片里改。
- **部署只读配置**：
  - GitHub Pages：来源 `main` 分支 `/`。
  - EdgeOne：加速区域保持「不含中国大陆」以免触发 ICP 备案；若未来想大陆更快、愿走备案，另切区域。

---

## 6. 常用命令回顾（给接手者）
```bash
# 从凭据管理器读 GitHub token（不要明文贴在文档/命令里）
printf "protocol=https\nhost=github.com\n\n" | git credential fill

# 看远程与分支
git -C "c:/Users/13706/Desktop/1" remote -v
git -C "c:/Users/13706/Desktop/1" branch --show-current

# 本地预览
cd "c:/Users/13706/Desktop/1" && python -m http.server 4173
# 浏览器打开 http://localhost:4173/index.html
```
