# Docker应用库 · My Docker Hub

一个自托管的 Docker 应用收藏站。左侧是可自定义的分类，右侧是每一条 Docker 应用 / 镜像的信息卡片，把官网、GitHub、文档、版本更新、镜像地址、支持平台集中在一个页面里管理。

前后端一体、零第三方依赖：后端只用 Python 标准库，数据落在单个 SQLite 文件，Docker 一条命令即可部署。浏览器打开就是属于你自己的 Docker Hub 应用库。

- 项目名称：**Docker应用库**（My Docker Hub）
- 默认站点名称 / 网站标题：**Docker应用库**
- 默认站点副标题：**专属于你的DockerHub应用收藏**
- 版本：**1.4.1**

## 快速开始

```bash
git clone https://github.com/tanyang88/my-docker-hub.git
cd my-docker-hub
docker compose up -d --build
```

打开 <http://localhost:8080>，用 `admin / admin123` 登录，然后到「设置 → 账号安全」改掉默认密码。

不想装 Docker，也可以直接用 Python 跑（3.9+，无第三方依赖）：

```bash
python app/server.py
```

Windows 上双击 `start.bat` 即可在后台启动，`stop.bat` 停止。

## 功能

### 分类与浏览
- **分类管理**：侧栏固定两项「全部」和「未分类」，其余分类自行添加、删除；分类多时自动滚动（滚动时显示滚动条，静止 1.2 秒淡出）。登录后可拖动分类条目调整顺序（顺序存 `sort_order`），右侧数量徽标统一右对齐。
- **分类计数**：每个分类右侧显示该分类下的应用数量，与卡片边框保持固定间距。
- **多分类归属**：一条应用可同时属于多个分类，添加 / 编辑时用标签多选；不选任何分类即归入「未分类」。
- **删除分类**：先弹确认框；确认后同时属于其他分类的卡片只从该分类移除，只属于该分类的卡片转入「未分类」，卡片本身不会丢。
- **应用卡片**：128px 图标（不放大、等比缩放），展示名称、简介、官网、GitHub、版本更新、官方文档、支持系统、授权方式、镜像 / 下载地址；标签统一带冒号，平台用竖线分隔（如 `Linux | Windows`），未勾选任何平台时不显示「支持系统」这一行。「全部」视图与各分类视图下，标题右侧都会显示该卡片所属的分类标签。
- **布局切换**：每行 1 个卡片 / 2 个卡片 / 只显示图标（图标在上、名称在下，每行 5 个等宽）。默认每行 1 个卡片，选择记在浏览器里。
- **图标聚焦**：在「只显示图标」模式下点击卡片，会把应用名称填入搜索框并以完整卡片单独展示；清空搜索或点分类回到图标模式。
- **固定操作栏**：标题与「每行显示 / 每页显示 / 排序」固定在顶部，只有卡片区滚动，翻页不用滚回顶部。
- **多选批量**：点「多选」进入选择模式，可用「移动至」把选中卡片加入目标分类（在具体分类视图下同时从当前分类移出），或「删除」当前分类下的卡片；在「全部 / 未分类」视图下删除即彻底删除（二次确认）。
- **置顶**：卡片操作区「📌 置顶」让卡片固定排在最前，不受排序影响，再点一次取消。
- **导入Docker应用信息**：添加弹窗右上角「导入信息」可打开导入对话框，粘贴包含 Docker 应用名称、简介、官网、GitHub、版本更新、文档、图标、平台、授权、分类、下载地址、其他链接、卡片背景色的文字后，会按字段别名、Markdown 列表 / 表格、链接域名和同义词规则自动识别并填入表单。默认只补充空白字段，勾选「覆盖表单中已有内容」后才替换；导入只填表单，不会自动保存。示例区的复制按钮紧跟「示例文档 / AI 提示词」标题右侧，可直接复制给 AI 收集应用资料；背景色留空不报错，无效链接提示会列出具体来源行和准确数量。
- **卡片分享**：鼠标悬停卡片时，右上角「置顶」左侧显示「分享」按钮；点击后按导入格式，把名称、简介、官网、GitHub、版本更新、文档、支持系统、授权方式、下载地址、其他链接和卡片背景色复制到系统剪贴板（不输出图标链接）。未登录时仍显示「分享」，但只显示该按钮。
- **卡片更新日期**：鼠标悬停卡片时，在「分享」按钮左侧以同样式的日期标签显示「更新日期：2026年11月28日」。日期取该 Docker 应用最后一次编辑保存的时间，新增应用取创建时间；图标模式、双列布局和 480px 以下窄屏因空间有限不显示日期。
- **卡片背景色**：编辑时可自定义背景色（色板或手填 `#rrggbb`），文字颜色按背景亮度自动切黑白。
- **分页**：每页 10 / 20 / 30 / 50 / 100 条，支持页码跳转。

### 搜索与排序
- **搜索**：顶栏按名称 / 简介模糊匹配，输入即搜（320ms 防抖），点分类自动清空搜索。结果按相关度排序：名称完全一致 > 忽略空格连字符后一致 > 名称前缀 > 名称包含 > 忽略符号后包含 > 仅简介命中；置顶与所选排序只在同相关度时作为次级规则。搜索兼容 `7 zip` 这类写法，命中关键词在名称与简介里标黄（可在「设置 → 搜索」关闭）。
- **排序**：默认、最新更新、最早上架、A-Z、Z-A、热门；置顶卡片始终最前。中文名按拼音排序（内置 `app/pinyin.py` 拼音表，无需第三方库）。
- **筛选**：侧栏「系统平台」「授权方式」两个下拉框，与分类、搜索、排序叠加生效。

### 热门统计
- 前端只上报三类最小信息：卡片内链接点击（带链接标识）、仅图标模式下点卡片、防抖后的搜索关键词。写入 `events` 原始表与 `software_stats` 按天汇总表。
- 热度分按「统计时间窗」计算：近 7 天 = `0.6 × 近 7 天(τ=7) + 0.4 × 近 30 天(τ=14)`；近 30 天 = 单窗口 `τ=14`；关闭则热度归零并隐藏热门排序。行为权重：链接点击 1.0、图标点击 0.6、聚焦查看 0.4、被搜索命中 0.3，结果落在 `hot_score` 派生列。
- 同一 IP 对同一目标 10 秒内只计一次；纯 SQLite 实现，IP 只在内存用于防刷，不落库、不记 UA。
- 工具栏「🔥 热门榜」可看热门应用 Top 10（含热度分）、搜索词 Top 20（近 30 天累计）、近 7 天趋势。

### 登录、设置与数据
- 右上角「登录」，支持记住密码（30 天，否则 12 小时）与忘记密码引导。未登录时所有写操作入口隐藏，接口返回 401。
- **重名提醒**：添加应用时如果名称已存在（忽略首尾空格与英文大小写），保存前弹出「已有同名Docker应用」提醒——点「确认」继续添加同名条目，点「取消」关闭提醒并停留在添加窗口，已填内容全部保留，可改名后再次保存。查重按名称扫全库，不受当前分类、搜索词和分页影响；编辑已有条目不会触发该提醒。
- 「设置」包含：站点名称 / 副标题 / 网站标题 / 网站图标（上传后左上角图标与标签页图标一起更新）、热门统计（时间窗、是否显示热门榜）、搜索高亮开关、账号安全（改用户名 / 密码）、登录防护（连续登录失败多少次后锁定、锁定时长，填 0 表示不限制）、备份与恢复、关于（当前版本 1.2.0）。
- 交互细节：单击卡片标题即复制标题到系统剪贴板（HTTPS 用 Clipboard API，局域网 http 自动回退为兼容方式），标题悬停变主题色、指针变手型；多选模式下点标题优先执行选中，不会误复制。所有文本 / 密码 / 数字输入框和多行说明框右侧内置「×」清空按钮，动态新增的下载地址行、其他链接行同样带清空按钮；密码框在「×」左侧还有「显示 / 隐藏密码」切换，切换后文字不会压到按钮。
- 主题：顶栏「明亮 / 跟随系统 / 暗黑」三档，记住在当前浏览器，默认跟随系统。
- 数据：`data/app.db` 存全部内容，`data/uploads/` 存上传图标，容器重建不丢。

## 首次启动与默认账号

首次启动自动建库并创建管理员：

- 用户名：`admin`
- 密码：`admin123`

**部署后请立即在「设置 → 账号安全」改成自己的密码。** 也可用环境变量 `ADMIN_USER` / `ADMIN_PASSWORD` 覆盖。

## 忘记密码

登录弹窗里的「忘记密码」会给出一条重置命令，实际重置有两种方式：

```bash
# 容器部署（compose）
docker compose exec my-docker-hub python app/reset_password.py 新密码

# 本地直接运行
python app/reset_password.py 新密码
```

也可以用环境变量，适合临时写进 `docker-compose.yml`：

```bash
RESET_USERNAME=admin RESET_PASSWORD=新密码 python app/reset_password.py
```

重置会同时清掉所有登录会话，已登录的设备需要重新登录。

## 技术栈

| 层 | 实现 |
|---|---|
| 后端 | Python 3.11 标准库（`http.server` + `sqlite3`），无第三方依赖 |
| 前端 | 原生 HTML / CSS / JS，无构建步骤 |
| 数据 | SQLite 单文件数据库 |
| 鉴权 | PBKDF2-SHA256（20 万次迭代）+ HttpOnly Cookie 会话 |
| 部署 | Docker / Docker Compose，镜像基于 `python:3.11-slim`，构建时不联网装包 |

## 目录结构

```
my-docker-hub/
├─ app/
│  ├─ server.py            # 后端：API + 静态资源 + 上传 + 鉴权
│  ├─ pinyin.py            # 内嵌拼音表：中文名按拼音排序
│  ├─ reset_password.py    # 忘记密码时的重置脚本
│  └─ static/
│     ├─ index.html        # 页面结构
│     ├─ styles.css        # 样式（含深色主题）
│     ├─ app.js            # 交互逻辑（含中英文 i18n）
│     ├─ favicon.svg       # 站点图标（矢量，顶栏与标签页共用）
│     ├─ favicon.ico
│     ├─ favicon-16/32/48/64/192/512.png
│     └─ apple-touch-icon.png
├─ Dockerfile
├─ docker-compose.yml
├─ .dockerignore
├─ .gitignore
├─ start.bat / start.ps1   # Windows 一键启动
├─ stop.bat / stop.ps1     # Windows 一键停止
├─ data/                   # 运行时生成：app.db + uploads/
└─ README.md
```

## Docker 部署

前置：已安装 Docker（Windows / macOS 用 Docker Desktop，Linux 用 docker-ce）。

```bash
git clone https://github.com/tanyang88/my-docker-hub.git
cd my-docker-hub
docker compose up -d --build   # 构建并后台启动
docker compose ps              # 查看状态
docker compose logs -f         # 查看日志
```

停止 / 更新：

```bash
docker compose down          # 停止并移除容器（数据仍在 ./data）
docker compose up -d --build # 改代码后重新构建
```

只用 Docker、不用 compose：

```bash
docker build -t my-docker-hub:latest .
docker run -d --name my-docker-hub -p 8080:8080 -v "$(pwd)/data:/app/data" --restart unless-stopped my-docker-hub:latest
```

Windows PowerShell 里 `-v` 要写绝对路径：

```powershell
docker run -d --name my-docker-hub -p 8080:8080 -v "D:\Docker\my-docker-hub\data:/app/data" --restart unless-stopped my-docker-hub:latest
```

## 推送到 Docker Hub

把 `yourname` 换成你的 Docker Hub 用户名（必须全小写）：

```bash
docker login
docker build -t yourname/my-docker-hub:latest -t yourname/my-docker-hub:1.3.0 .
docker push yourname/my-docker-hub:latest
docker push yourname/my-docker-hub:1.3.0
```

之后任何装了 Docker 的机器都能直接拉取，不需要源码：

```bash
docker run -d --name my-docker-hub -p 8080:8080 \
  -v "$(pwd)/data:/app/data" -e TZ=Asia/Shanghai \
  --restart unless-stopped yourname/my-docker-hub:latest
```

镜像内固定 `DATA_DIR=/app/data`，挂载 `data` 即持久化。升级：`docker pull` → `docker rm -f my-docker-hub` → 用同样命令重新 run（`data` 不动，数据不丢）。

## 本地运行与 Windows 一键脚本

```bash
python app/server.py
```

| 脚本 | 作用 |
|---|---|
| `start.bat` / `start.ps1` | 设置 `PORT=8080`、`DATA_DIR=./data` 后隐藏窗口启动；8080 有旧实例会先关掉；失败时打印 `data/server.err.log` 最后几行 |
| `stop.bat` / `stop.ps1` | 停止监听 8080 的进程，数据目录不动 |

双击 `start.bat`，浏览器打开 <http://127.0.0.1:8080>。日志写在 `data/server.out.log` 与 `data/server.err.log`。

默认监听 `0.0.0.0:8080`，数据写入项目下的 `data/`。

## 配置项

| 环境变量 | 默认值 | 说明 |
|---|---|---|
| `PORT` | `8080` | 服务端口 |
| `HOST` | `0.0.0.0` | 监听地址 |
| `DATA_DIR` | 项目下 `data/`（镜像内 `/app/data`） | 数据库与上传文件目录 |
| `ADMIN_USER` | `admin` | 首次初始化创建的管理员用户名 |
| `ADMIN_PASSWORD` | `admin123` | 首次初始化创建的管理员密码 |

改端口：编辑 `docker-compose.yml` 的 `ports`，例如 `"9000:8080"` 表示宿主机 9000 映射容器 8080。

## 站点设置（存数据库）

登录后在「设置」里改，保存即生效：

| 设置项 | 键 | 可选值 |
|---|---|---|
| 站点名称 | `site_name` | 任意文本，默认「Docker应用库」 |
| 站点副标题 | `site_tagline` | 品牌下方那行文案，默认「专属于你的DockerHub应用收藏」 |
| 网站标题 | `site_title` | 浏览器标签页标题，默认「Docker应用库」 |
| 网站图标 | `site_icon` | 图标 URL 或上传后的 `/uploads/xxx.png`；支持 png / jpg / jpeg / gif / webp / svg，≤ 5MB。推荐 SVG 或 512×512 透明底 PNG |
| 外观主题 | `theme` | `light` / `dark` / `system`（默认 `system`） |
| 语言 | `language` | `zh-CN` / `en` |
| 热门统计时间窗 | `hot_window` | `7`（默认）/ `30` / `off` |
| 热门榜入口 | `hot_board` | `1`（默认显示）/ `0`（隐藏） |
| 搜索高亮 | `search_highlight` | `1`（默认）/ `0` |
| 登录失败上限 | `login_max_fails` | 连续失败多少次后锁定账户，`0`（默认，不限制）/ 正整数 |
| 登录锁定时长 | `login_lock_minutes` | 锁定多少分钟，`0`（默认，不限制）/ 正整数；两项任一为 `0` 即不启用 |

登录防护的行为：同一用户名连续密码错误达到上限时，立即返回「账户已锁定，请在 N 分钟后再试」（HTTP 429），锁定期内即使密码正确也会被拒绝；登录成功即清零失败计数。锁定记录存在 `login_guard` 表，忘记密码仍可用 `reset_password.py` 重置。


## 数据备份与迁移

- 登录后「设置 → 备份与恢复 → 下载备份」，得到包含 `app.db` 与全部上传图标的 zip。
- 恢复：上传该 zip（也接受单独的 `app.db`），会覆盖现有数据并强制重新登录。
- 手工备份：打包 `data/` 目录即可；迁到新机器把 `data/` 放回同一位置再 `docker compose up -d`。
- 想从空白开始：删掉 `data/app.db` 后重启，会重新建库并创建默认管理员。

## API 一览

公开接口（无需登录）：

| 方法 | 路径 | 说明 |
|---|---|---|
| GET | `/api/health` | 健康检查 |
| GET | `/api/categories` | 分类列表，返回 `{ items, total, uncategorized }` |
| GET | `/api/software?category_id=all&page=1&page_size=20&sort=default&q=&platform=&license=` | 卡片分页列表（`page_size` 默认 20、上限 120）；`category_id` 取 `all` / `uncategorized` / 分类 id；`q` 非空时按名称 / 简介搜索并忽略分类；`platform` 取 `windows`/`mac`/`linux`/`android`/`ios`/`harmonyos`；`license` 取 `free`/`subscription`/`onetime`/`metered`/`freemium`；条件可叠加，置顶卡片始终最前 |
| GET | `/api/settings` | 站点设置 |
| GET | `/api/auth/me` | 当前登录状态 |
| GET | `/api/hot` | 热门榜数据 `{ window, items, searches, trend, total_events }` |
| POST | `/api/events` | 行为埋点（热门排序数据源，匿名可用）：`{ "type": "link_click" \| "icon_click" \| "focus_view", "software_id": 3, "field": "url_home" }` 或 `{ "type": "search", "keyword": "nginx" }`；类型不合法返回 400，同一 IP + 同一目标 10 秒内重复上报被忽略 |

`sort` 取值：`default`、`updated`、`oldest`、`name_asc`、`name_desc`、`hot`。

需要登录（否则 401）：

| 方法 | 路径 | 说明 |
|---|---|---|
| POST | `/api/auth/login` | 登录 `{ "username", "password", "remember" }`，成功后下发 `hub_session` Cookie；触发登录防护时返回 429 与锁定提示 |
| POST | `/api/auth/logout` | 退出登录 |
| PUT | `/api/auth/account` | 改用户名 / 密码 `{ "current_password", "username", "new_password" }` |
| POST | `/api/categories` | 新建分类 `{ "name": "基础设施" }` |
| POST | `/api/categories/order` | 保存分类顺序 `{ "ids": [3, 1, 2] }` |
| PUT | `/api/categories/{id}` | 重命名分类 |
| DELETE | `/api/categories/{id}` | 删除分类：属于其他分类的卡片只从本分类移除，只属于本分类的转入「未分类」 |
| POST | `/api/software` | 新建卡片，`category_ids` 可为空数组（空即「未分类」） |
| PUT | `/api/software/{id}` | 更新卡片（整条覆盖，`category_ids` 整体替换） |
| DELETE | `/api/software/{id}` | 删除卡片（所有分类下） |
| DELETE | `/api/software/{id}?category_id=3` | 只删除该分类下的卡片；已不属于任何分类则整条删除 |
| POST | `/api/software/{id}/pin` | 置顶 / 取消 `{ "pinned": true }` |
| POST | `/api/software/bulk_move` | 批量移动 `{ "ids": [1,2], "to_category_id": 3, "from_category_id": 2 }` |
| POST | `/api/software/bulk_delete` | 批量删除 `{ "ids": [1,2], "category_id": 3 }`（带分类只移出该分类，否则彻底删除） |
| POST | `/api/upload?filename=icon.png` | 上传图标，请求体为文件二进制 |
| PUT | `/api/settings` | 保存站点设置（只接受上表列出的键） |
| GET | `/api/backup` | 下载备份 zip |
| POST | `/api/restore` | 恢复备份（zip 或裸 db） |

卡片对象字段：`name`、`category_ids`、`category_names`、`description`、`logo`、`url_home`、`url_github`、`url_release`、`url_doc`、`downloads`（`[{text, link}]`）、`extra_links`（`[{text, link, label?}]`）、四个固定链接的自定义名称与显示文字（`url_*_label` / `url_*_text`）、`platforms`（布尔对象）、`licenses`（字符串数组）、`pinned`、`card_color`。

## 常见问题

- **端口被占用**：改 `docker-compose.yml` 的 `ports`，或停掉占用 8080 的程序。
- **页面能打开但接口 500**：看 `docker compose logs`，通常是 `data/` 无写权限；Linux 上 `sudo chown -R 1000:1000 data`。
- **图标不显示**：外链图标可能被防盗链，建议用「上传本地图片」存到自己服务器。
- **忘记密码 / 被锁在外面**：用 `reset_password.py` 重置，不依赖网页登录。
- **放到公网**：在容器前加一层 Nginx / Caddy 做 HTTPS 反代到 `127.0.0.1:8080`；项目是单管理员模型，公网部署务必先改默认密码，必要时再叠一层访问控制（Basic Auth 或 VPN）。
- **登录多久失效**：勾选记住密码 30 天，否则 12 小时；改密码、恢复备份会让所有会话立即失效。

## 版本说明

### 1.4.1
- **卡片更新日期**：鼠标悬停卡片时，在「分享」按钮左侧新增更新日期标签，格式为「更新日期：2026年11月28日」；标签沿用操作按钮的边框、圆角、字号和 6px 间距。日期取最后一次保存时间，新增应用取创建时间；未登录时同样显示，只读不可点击。图标模式、双列布局和 480px 以下窄屏因宽度不足不显示该标签。
- 设置 → 关于显示「当前版本：1.4.1」。

### 1.4.0
- **导入Docker应用信息**：添加弹窗新增「导入信息」入口和导入对话框，粘贴文字即可按规则识别名称、简介、官网、GitHub、版本更新、文档、图标、支持系统、授权方式、分类、下载地址、其他链接和卡片背景色，并填入表单；默认只补空白字段，勾选「覆盖表单中已有内容」后才替换。对话框提供可复制的「示例文档 / AI 提示词」，复制按钮紧跟标题右侧。
- **导入提示优化**：背景色为空不误报，格式错误时附带 `卡片背景色：#F3F4F6 或 #FFF` 示例；多个无效链接显示真实总数，并逐行列出具体来源行。
- **卡片分享**：卡片右上角新增「分享」按钮，位于「置顶」左侧，样式和间距与原操作按钮一致；点击按导入格式复制应用资料，不输出图标链接。未登录时悬停仍显示「分享」。
- 设置 → 关于显示「当前版本：1.4.0」。

### 1.3.0
- 卡片区右侧的页面空白处、紧邻卡片滚动条新增向上的「回到顶部」和向下的「滑到底部」两个按钮，不占用卡片宽度；点击后平滑滚动到卡片区顶部或底部。
- 没有可滚动内容时按钮自动隐藏，到达顶部或底部时对应按钮自动置灰；手机端及没有足够右侧空白的窄窗口会自动隐藏，避免压住滚动条。
- 设置 → 关于显示「当前版本：1.3.0」。

### 1.2.0
- 新增添加应用时的重名提醒：名称与库里已有条目重复（忽略首尾空格和英文大小写）时，保存前弹出「已有同名Docker应用」窗口；「确认」继续添加，「取消」返回添加窗口且已填内容完整保留。查重扫全库，不受分类、搜索词、分页影响；编辑已有条目不触发。
- 卡片标题右侧的分类标签改为在「全部」视图和所有自定义分类视图下都显示（「未分类」视图不显示）。
- 设置 → 关于显示「当前版本：1.2.0」。

### 1.1.0
- 卡片标题可一键复制：单击标题即把名称写入系统剪贴板并提示「已复制标题：xxx」；HTTPS 下用 Clipboard API，局域网 http 自动回退到兼容方案，确保真正写入剪贴板。标题悬停变主题色、指针变手型；多选模式下单击仍优先执行选中，不会误复制。
- 所有文本、密码、数字输入框以及多行说明框右侧内置「×」清空按钮，点击即清空并保持输入框聚焦；动态新增的下载地址行、其他链接行自动带上清空按钮。
- 登录窗口和设置里的密码输入框新增「显示 / 隐藏密码」按钮，图标与提示文字随状态切换，密码框已预留按钮空间，文字不会被按钮压住。
- 搜索框沿用原有清除按钮；数字框隐藏系统步进按钮，避免与清空按钮重叠；分页跳转框保持原紧凑宽度。
- 设置 → 关于显示「当前版本：1.1.0」。

### 1.0.2
- 「设置 → 登录防护」布局调整：两个输入框加宽自适应，保存按钮移到最右侧，控件间距保持不变。
- 设置 → 关于显示「当前版本：1.0.2」。

### 1.0.1
- 新增「设置 → 登录防护」：连续登录失败达到设定次数后按设定时长锁定账户，填 0 表示不限制；触发时登录接口返回 429 与剩余锁定时间。
- 修复「添加 Docker 应用」弹窗里「图标」区域中「上传本地图片」与「清除」两个按钮高度不一致的问题，两者现在等高对齐。
- 设置 → 关于显示「当前版本：1.0.1」。

### 1.0
- 首个版本：站点名称与网站标题默认「Docker应用库」，副标题「专属于你的DockerHub应用收藏」，设置 → 关于显示「当前版本：1.0」。
- Docker 蓝色渐变站点图标（白色鲸鱼驮容器），矢量 SVG + ICO + 16 / 32 / 48 / 64 / 192 / 512 与 180px 触屏图标。
- 分类与多分类归属、卡片置顶、批量移动 / 删除、自定义背景色、搜索高亮、拼音排序、热门统计与热门榜、备份恢复、深浅色主题、中英文切换。
- 链接区支持「名称 / 链接 / 显示文字」三列，其他链接可按住左侧手柄拖曳排序；下载地址全部留空时卡片不显示该行。
