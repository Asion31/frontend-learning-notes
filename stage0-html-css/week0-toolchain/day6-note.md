# stage0-html-css / week0-toolchain / Day6 笔记（Week0 复盘）

> 日期：2026-11-22

## 今日任务

本周复盘：补全笔记 + JS 算法 0~2 题

## 一、Week0 十问自测（答案对照）

### Git

| # | 问题 | 答案 | 一句话记忆 |
|---|---|---|---|
| 1 | 三区各放什么、提交必经哪两步 | 工作区＝正在编辑的文件；暂存区（index）＝挑出来"下次提交要包含什么"；版本库＝`.git` 里已生成的历史快照。路径是 `git add` → `git commit` | **add 挑货，commit 打包上架** |
| 2 | `??` / `A` / `AM` | `??` 未跟踪（Git 没见过的新文件）；`A` 已进暂存区的新文件；`AM` 已 add 之后工作区又改过（下次 commit 只含 add 时那一版） | **左列看暂存区，右列看工作区** |
| 3 | 本地已提交未推送怎么显示 | 显示 `Your branch is ahead of 'origin/main' by 1 commit`；ahead 1＝本地比远程多 1 条提交，还没 push | **ahead 是"我领先远程几条"** |
| 4 | fetch vs pull | `fetch` 只把远程更新拉到本地的 `origin/main` 记录里，不动工作区和当前分支；`pull` ＝ `fetch` + `merge`，立刻合并进当前分支，可能带出冲突 | **fetch 只侦察，pull 直接动手** |
| 5 | 冲突三标记 + 收尾动作 | `<<<<<<< HEAD`（当前分支版本） / `=======`（分界） / `>>>>>>> feature/xxx`（被合并分支版本）。删标记、留正确内容后，必须 `git add` + `git commit` 生成合并提交才算完成 | **冲突不是解决完就完事，要再提交** |

### 命令行 / 环境

| # | 问题 | 答案 |
|---|---|---|
| 6 | 装了 Git 却提示找不到命令 | 因为 `PATH` 是系统找命令时按顺序搜索的目录清单，`git.exe` 所在的 `C:\Program Files\Git\cmd` 不在清单里。加进 PATH 后**必须新开终端**（新进程才读新环境变量），VSCode 也要重启 |

### DevTools

| # | 问题 | 答案 |
|---|---|---|
| 7 | 200 / 304 / failed | **200** 服务器返回完整内容；**304** 缓存里的还是最新的，服务器只回响应头不回正文（数据照样能用，省流量）；**failed** 请求根本没连上（断网 / DNS / 被拦），压根不是服务器给的响应 |
| 8 | Elements 改色刷新还在吗 | 不在，刷新就还原。改的是浏览器内存里的运行时样式，源文件没动，刷新是从文件重新加载。要永久生效必须改文件 |
| 9 | 怎么跳到报错代码行 | Console 报错信息后面带 `文件名:行号`（如 `day5-devtools-demo.html:73`），点它直接跳到 Sources 对应位置 |
| 10 | 断点暂停时能否用 Console 查值 / 陌生 localStorage 键 | 能，暂停时 Console 处于该作用域，输入变量名取到的是当时的真实值。陌生键最常见原因：`file://` 下所有本地页面**共用一套** localStorage，是别的本地页面写的（以后 `localhost` / 真实域名每个源各自独立） |

## 二、本周（Week0）真正学会的四件事

1. **Git 有"三区"，命令只是在这三区之间搬东西**——以后所有 Git 困惑（为什么 status 显示怪、为什么 diff 是空的）都能靠这张图定位
2. **本地和远程是两个仓库，需要显式同步**：`clone` 建立关系、`commit` 进本地、`push` 才到远程、`fetch/pull` 才拿到别人的更新
3. **分支让改动可以隔离，合并冲突是"两边改了同一处"，不是出错**
4. **DevTools 五个面板分工明确**：Elements 看结构改样式、Console 查报错试代码、Sources 查执行过程、Network 查请求、Application 查存储

## 三、踩过的坑（已修）

- 环境：`git` 装了但不在 PATH → 加 PATH 并重开终端
- 误操作：`git status > tatus --short` 敲串，生成了一个 6368 字节脏文件并提交推送 → 用 `git rm -- '文件名'` 删除后重新提交推送
- 认知：把 Network 的 **304** 当成失败 → 其实是"用缓存、省流量"

## 四、遗留 / 下一周预告

- 遗留：CORS 跨域为什么被挡、`Preserve log` 的具体用途 → stage2 讲网络时补
- 下一周 `stage0-html-css/week1-html-basic`：HTML 语义化骨架 → 表单 → 列表与表格 → meta / SEO / 可访问性 → 个人简介页 → 复盘

## 卡点与解决（STAR 素材）

- **S 情境**：week0 收尾做十问自测，检验 Git / 命令行 / DevTools 到底有没有真懂
- **T 目标**：找出"以为会了其实没懂"的点，避免带着模糊概念进 HTML 阶段
- **A 做法**：先凭印象答，再逐题对照标准答案，重点看**304 与 failed 的区别**、**fetch 与 pull 的差别**、**冲突后要再 commit** 这三处
- **R 结果**：Git 整条工作流能自己讲一遍，DevTools 五面板都亲手操作过；week0 通关，进入 HTML

## 代码位置

- `src/`（Day5 练习页 `day5-devtools-demo.html` 保留作 DevTools 素材）

## 待补 / 疑问

- `git rebase` 和 `merge` 的区别（等有分支协作了再看）
- DevTools 的 Performance / Lighthouse 面板（留到 stage7 性能优化）
