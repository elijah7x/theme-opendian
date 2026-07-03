# AGENTS.md — Opendian

Obsidian theme. Single-file CSS, no build step. Aesthetic: **OpenCode / terminal**
— mono-forward type, sharp consistent radii, monochrome accents, flat tinted
states (no drop shadows). When in doubt, choose the more restrained, more
"developer-tool" option.

## 纪律（必须遵守）

1. **只改 `theme.css`，且只在本目录（`Opendian-dev`）。** 这是 `dev` 分支的工作区。
   - 旁边的 `../Opendian` 是 **`main` 分支**（发布产物）。**禁止直接编辑**它。
   - 发布流程是 `dev` → `main` 的合并，不是手动拷贝文件。
2. **改前先读。** 这是 2800+ 行的单文件，定位到目标区段（见下表）再动手，不要整文件重排。
3. **最小改动。** 不重构无关代码、不加装饰性注释、不"顺手优化"未要求的部分。
4. **改完即验**（见下）。纯 CSS 看不出对错，必须肉眼确认。
5. Obsidian 约定：**优先用官方 CSS 变量**（`--background-primary`、`--text-normal`、
   `--interactive-accent`…），自造 token 用 `--oc-*` 前缀。
6. **light / dark 双覆盖**：design token 分两套（`:root` 亮色 + `.theme-dark`）。
   改颜色相关的东西，两套都要顾到，否则暗色模式会破。

## 可视化验证回路

DEV_VAULT 已通过 symlink 把本主题挂进 `../DEV_VAULT/.obsidian/themes/Opendian-dev`，
改完 `theme.css` 后：

1. 在 Obsidian 里 reload（`Cmd+R`）或在外观设置里切走再切回主题。
2. 打开 `../DEV_VAULT` 里的测试文档对照：`表格测试.md`、`按钮体系测试.md`、
   `可访问性测试.md`、`Callout`/`Anthropic Design Reference.md` 等。
3. 桌面截图比对前后（Obsidian 是 Electron，浏览器类 MCP 用不上，用系统截图）。
4. 确认 light 和 dark 两种外观都正常。

## theme.css 区段地图

行号是近似值，**大改后会漂移**——重新生成用：
`awk '/====================/{getline t; if(t!~/=/) printf "%d: %s\n", NR, t}' theme.css`

| 行号 | 区段 |
|------|------|
| 16   | OPENCODE DESIGN TOKENS（`:root` 亮色 ~20，`.theme-dark` ~201：调色板 / 语义别名 / `--oc-*` 短 token / callout / 圆角 / 字体栈） |
| 377  | 1. BASE（`body`、prose 节奏、frontmatter properties） |
| 439  | 2. SIDEBAR & LOGO（左栏、vault 双色 wordmark、ribbon、侧栏图标对齐、设置图标、root split divider） |
| 819  | 3. TABS（沉浸式单栏 tab，浮动 pill 几何，pin 标记） |
| 1018 | 4. HEADINGS & TYPOGRAPHY |
| 1090 | 5. TABLE（关掉原生斑马纹、行 hover、紧凑表格） |
| 1185 | 6. CALLOUT（Starlight aside 风：中性面 + 语义色轨/标题/图标） |
| 1399 | 7. CODE BLOCKS（语言标签栏、复制按钮——刻意禁用） |
| 1507 | 8. CHECKBOXES & LISTS（普通勾选变量驱动，其余任务态自定义） |
| 1687 | 9. SETTINGS & MODALS（toggle pill、buttons、select/dropdown） |
| 1955 | 10. TAGS |
| 2005 | 11. RIGHT SIDEBAR / OUTLINE / BACKLINKS |
| 2211 | 12. SCROLLBARS |
| 2238 | 13. PROMPT / COMMAND PALETTE |
| 2292 | 14. TOOLTIPS / MENUS / DROPDOWNS（含 notice/toast 终端气泡） |
| 2381 | 15. STATUS BAR & TITLE BAR |
| 2475 | 16. SYNTAX HIGHLIGHTING（基础回退） |
| 2569 | 17. EDITOR / SOURCE MODE PARITY（源码模式：标题、格式标记、链接、inline code、选区） |
| 2838 | 18. MOBILE / NARROW LAYOUT（`max-width: 600px`） |
| 2893 | 19. REDUCED MOTION（`prefers-reduced-motion` 全局过渡/动画压到 0.01ms） |

## 视觉品味（CSS 通用，迁移自 taste-skill）

写/审样式时主动规避 AI 设计 tells——本主题是终端/OpenCode 美学，更要克制：

- **不用纯黑/纯白。** 用 off-black、charcoal 这类带微色温的中性色。
- **强调色克制：最多一个，饱和度压住。** 禁 "AI 紫/霓虹蓝" 发光渐变。本主题是 monochrome 取向，accent 单一，别引入抢戏的彩色。
- **层级靠字重+颜色，不靠一味放大字号。**
- **状态用扁平染色填充（flat tinted fill），不用阴影/外发光。** 这正是本主题 active tab、icon hover 的既定做法，保持一致。
- **动画只动 `transform` 和 `opacity`**，绝不动 `top/left/width/height`——避免重排掉帧。
- **圆角走统一的 radius scale**（§TOKENS 已定义），别每处自造角值。
- **色板全局统一**，颜色走 `--oc-*` / 官方变量，别在组件里散写魔法色值。

## 提交前

- `manifest.json` 的 `version` 是否需要 bump。
- light + dark 都验过。
- 没有顺手改到无关区段。
- **如果要发版**：bump version 后打 tag（无 v 前缀，== version）并 `git push origin dev --tags`，workflow 自动接管（见「发版流程」）。

> ⚠️ **别被 `versions.json` 误导。** 仓库里有 `versions.json`，但**那是插件（plugin）概念，主题（theme）根本不读它**。它不影响安装、也不影响市场可见性——留着无害，但**不要**当发版必做项，更别以为动它能修市场问题。

> ⚠️ **市场可见性 = 在官方索引里 + release 合规，缺一不可（且会被踢出）。**
> 能否在 app 内市场搜到，取决于主题在不在官方 `obsidianmd/obsidian-releases` 的
> `community-css-themes.json`（那份清单就是"索引"）。**但索引不是一劳永逸的**——
> 推了不合规的 release 会被官方**踢出索引**。姊妹项目 Folio 就是这么掉的：一次 gh 版本
> 推送后被 de-index，清单里查无此项（**Opendian 目前仍在，别把它也搞掉**）。所以 release
> 合规不是"跟市场无关"，恰恰是**保住索引的必要条件**。
>
> **万一被踢出，恢复流程**：
> ① 先把仓库 + release 弄合规（tag == manifest `version` 且**无 v**、挂全资产、Latest 正确、
>   manifest 必填字段齐、theme.css 不引外部网络资源/远程字体）→
> ② 走 Obsidian 官方主题审核流程（在主题管理页拉取 gh 的**最新版本**提审）→
> ③ 通过后，官方索引机器人**每 1–2 小时**扫一遍，自动重新索引 → app 内又能搜到。
>
> **诊断顺序**：先 `grep -i opendian` 那份清单看在不在索引里；再查 latest release 的 tag
> 是否 == manifest version（无 v）+ 资产是否齐。两头都要看。

## 🚨 红线：发版必须合规，否则会被踢出主题市场索引

> **这些红线现在由 `release.yml` workflow 作为校验步骤强制执行**（见下「发版流程」）。
> 但红线本身不会过时——它是 workflow 的设计依据，也是 workflow 故障时手动应急的核对清单。
> 理解红线，才能理解 workflow 在防什么。

**这是运营铁律，比任何代码改动都重要，牢记：**

- Opendian **已在**官方索引里（`community-css-themes.json`）。只要每次发版都合规，
  发布后主题市场的版本号会**近乎实时**跟进——非常快。
- **一旦发出一个不合规的 release，主题会被官方直接剔除出索引**，app 内立刻搜不到。
  之后只能干等官方索引机器人**周期性扫描**（慢时要好几个小时）才重新收录。
- 所以：**宁可慢，不可错。** 发版前后必须逐条核对合规，绝不为图快跳过自检。

**"合规"= 每次发版都满足：**
1. release 的 tag **精确等于** manifest 的 `version`，**绝不带 `v` 前缀**。
2. release 挂全资产（至少 `theme.css` + `manifest.json`）。
3. release 被标为 **Latest**（`--latest`；乱序/超时可能标错，必须核）。
4. main 的 manifest `name` 是 `"Opendian"`（不是 `"Opendian-dev"`）、必填字段齐。
5. `theme.css` 不引用外部网络资源 / 远程字体（审核硬性要求）。

**发完 30 秒强制自检（缺一不可）：**
```bash
gh api repos/elijahchan2019/obsidian-opendian-theme/releases/latest --jq '.tag_name'  # == manifest version，无 v
gh release view <版本> --json assets --jq '.assets[].name'                            # theme.css / manifest.json 在
gh api repos/elijahchan2019/obsidian-opendian-theme/releases --jq '.[]|select(.tag_name=="<版本>")|.draft'  # 必须 false
```
> ⚠️ 坑：`gh release create` 传大图可能**客户端超时**，把 release 卡在 **draft** 态
> ——draft 不建 tag、不算 Latest、市场也看不到。若自检发现 `draft=true`，
> 用 `gh release edit <版本> --draft=false --latest` 转正。

## 发版流程（tag 触发，GitHub Action 自动同步 main + 发 release）

> 自 v1.12.4 起，发版由 `.github/workflows/release.yml` 自动化。**不再手动 merge dev→main、
> 不再手动改 name、不再手动 rm 开发文件、不再手动 `gh release create`。** 这些步骤全部
> 由 workflow 在 tag 推送时执行，红线（无 v、tag==version、资产齐、`--latest`、非 draft、
> 无外部资源）作为显式校验步骤硬编码在内——任一不符直接 fail，发不出不合规的 release。

### 日常发版（你的全部操作）

```bash
# 在 dev 上：
# 1. 改 theme.css（或其他发布产物）
# 2. bump manifest.json 的 version，例如 1.12.4
# 3. commit + 打 tag（tag 必须无 v 前缀，必须 == version）+ 一条命令推送：
git add -A
git commit -m "v1.12.4: <改动摘要>"
git tag 1.12.4
git push origin dev --tags
# 完成。workflow 接管，约 1 分钟后 release 上线。
```

**就这些。** workflow 会自动：校验 tag → 镜像 dev 产物到 main（重写 name 为 `Opendian`、
排除 AGENTS.md/.opencode/.github）→ `gh release create --latest` 挂全资产 → 自检 tag/资产/draft。

### workflow 做了什么（无需你管，但要知道）

1. **校验红线**：tag 无 `v` 前缀、tag == manifest version、theme.css 无外部网络资源、必备文件齐全。任一不过 → fail，不动 main、不发 release。
2. **强制镜像 main**：把 dev 的发布产物（白名单：theme.css/manifest.json/README×2/截图×2/LICENSE/versions.json/.gitignore）拷到 main，**重写 manifest name 为 `Opendian`**。AGENTS.md/.opencode/.github 用白名单机制天然排除（不在名单里就不进 main）。
3. **建 release**：`--target main --latest`，非 draft，挂全资产。
4. **自检**：latest tag == 推送 tag、theme.css+manifest.json 在、draft=false。任一不符 → fail。

### 仍需人工把关的（workflow 管不到的）

- **release notes 内容**：workflow 从 commit 历史自动生成。如果要精心写 changelog，在 commit message body 里写清楚（workflow 取 commit subject）。
- **light/dark 视觉验证**：workflow 不看截图，你必须在 dev 上肉眼验过再发版。
- **obsidian 市场确认**：发版成功后，回 Obsidian 点「检查更新」确认不报错（这一步 workflow 替不了）。

### 为什么要 tag 触发（而不是 push dev 自动发）

- **防手滑**：bump version 是日常操作，但只有显式 `git tag` 才是发版意图。手滑改了 version 不会触发发版。
- **Obsidian 官方模式**：sample-theme/plugin 都是 tag-driven release，社区最成熟。
- **零额外工具**：tag 是 git 原生功能，不依赖 npm script 或外部 CLI。

### 手动发版（仅当 workflow 故障时的应急）

正常情况下**永远不要**手动发版——会让 main 状态与 workflow 不同步。仅当 workflow 本身坏了：

```bash
# 应急流程（与 workflow 逻辑等价）：
# 1. 在 dev 上确保 manifest version 已 bump
# 2. 切到 ../Opendian，手动同步 dev 的发布产物，改 name 为 Opendian
# 3. push main，gh release create <version> --target main --latest --title <version> ...
# 4. 立刻修 workflow，避免下次又得手动
```

**为什么 dev 的 name 是 "Opendian-dev"**：两个主题同名会在 Obsidian 里冲突。dev 叫 "Opendian-dev"、main 叫 "Opendian"，才能在同一 vault 同时存在并对照调试。**name 的差异现在由 workflow 在镜像时自动重写**——你本地 dev 始终是 "Opendian-dev"，无需手动切换。
