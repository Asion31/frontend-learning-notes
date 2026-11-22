# stage0-html-css / week0-toolchain / Day3 笔记

> 日期：2026-11-22

## 今日任务

远程仓库：GitHub / Gitee 建库、clone、push、pull、.gitignore 的作用（node_modules 为什么不提交）

## 一、本地仓库 与 远程仓库 的关系

```
本机（工作区 + 暂存区 + 本地版本库）                 GitHub（远程仓库）
        commit 之后           git push  ───────────────▶
                               ◀───────────────  git fetch / pull
```

| 名词 | 含义 |
|---|---|
| `origin` | 远程仓库的默认别名，指 `https://github.com/Asion31/frontend-learning-notes.git` |
| `main` | 本地分支 |
| `origin/main` | 本地记录的"远程 main 上次看到的样子"，叫**远程追踪分支** |
| 追踪关系 | 本仓库 `main` 追踪 `origin/main`，所以直接 `git push` / `git pull` 不用写远程名和分支名 |

四条命令的区别（今天全跑过）：

| 命令 | 干什么 | 什么时候用 |
|---|---|---|
| `git clone <地址>` | 把远程仓库连历史一起完整复制到本地，自动配好 origin 和追踪关系 | 第一次拿代码、换电脑 |
| `git push` | 把本地新提交上传到远程 | 每天收工 |
| `git fetch` | **只下载**远程的新提交到本地缓存，不动工作区、不合并 | 想先看看远程有什么变化 |
| `git pull` | = `fetch` + `merge`，下载并合并进当前分支 | 开新工作时先同步 |

## 二、实测记录

### 1. clone（完整复制）

在 `D:\code\git-lab` 下执行 `git clone https://github.com/Asion31/frontend-learning-notes.git frontend-lab`，结果：

- 目录结构和远程完全一致（algo / docs / notes / stage0~7 / README.md / .gitignore）
- `git log --oneline` 显示 3 条历史：first commit、Day1、Day2 —— **历史也一起复制过来了**
- `git remote -v` 自动出现 origin 的 fetch / push 地址
- `git status -sb` 显示 `## main...origin/main`，代表追踪关系已经建好

### 2. 本地领先远程（ahead 1）

在副本里提交一条 `实验：本地提交但没有推送`，`git status -sb` 变成：

```
## main...origin/main [ahead 1]
```

`ahead 1` = 本地比远程多 1 条提交，**还没推上去**。这就是"提交了但没备份"的状态，换电脑就丢了。

### 3. fetch 与 pull

```
git fetch origin          → 无输出（远程没有新提交）
git log origin/main -n 1  → 还是 Day2 那条，说明远程最新就是这个
git pull origin main      → Already up to date.
```

关键点：`fetch` 只更新 `origin/main` 这个"远程快照"，工作区文件不会变；`pull` 才会真正把远程的新内容合并进本地文件。

## 三、.gitignore 到底在做什么

在副本里造了三个文件再 `git status`：

| 文件 | status 结果 | 说明 |
|---|---|---|
| `node_modules/fake-lib.js` | 不出现 | 被 `.gitignore` 挡住 |
| `dist/bundle.js` | 不出现 | 被 `.gitignore` 挡住 |
| `stage0-.../src/notes.txt` | `??` 出现 | 没被忽略，Git 会管它 |

`git status --short --ignored` 能看到被忽略的：`!! dist/`、`!! node_modules/` —— `!!` 就表示"我知道它存在，但我按规则不管它"。

### node_modules 为什么不能提交

1. **体积巨大**：一个 Vue 项目 node_modules 动辄几百 MB、上万个文件，推到 GitHub 又慢又占配额
2. **可完全重建**：只要 `package.json` + `package-lock.json` 在，任何人执行 `npm install` 都能装出一样的依赖
3. **会制造无意义冲突**：不同系统装的包有差异，提交了天天冲突
4. **dist/ 同理**：打包产物由源码构建出来，源码在就够了，产物不该进版本库

所以规则是：**只提交源码和配置文件，不提交可再生成的东西**。

## 卡点与解决（STAR 素材）

- **S 情境**：要验证 `.gitignore` 是不是真的生效，准备在仓库里手动建 `node_modules` 和 `dist` 两个目录
- **T 目标**：确认 Git 不会把它们纳入版本控制
- **A 做法**：在克隆出来的副本里造文件，跑 `git status --short`，发现两个目录都没出现；再加 `--ignored` 参数，输出 `!! node_modules/`、`!! dist/`，确认是"被规则忽略"而不是"没被扫描到"
- **R 结果**：理解了忽略不是删除文件，只是 Git 假装看不见；也确认本仓库的 `.gitignore` 配置是正确的（Vue/Vite 项目该忽略的都覆盖了）

## 代码位置

- 今日无代码，实验在临时目录 `D:\code\git-lab` 完成，已删除
- 正式仓库未产生任何改动（除本笔记）

## 待补 / 疑问

- 真冲突怎么解决：两台机器改同一行才叫真冲突，`pull` 会提示 `CONFLICT`，需要手动改文件再 `add` + `commit`。Day4 会在自己仓库里人为制造一次冲突来练
- `git pull` 默认是 merge 还是 rebase：本机默认 merge；团队里有人用 `--rebase` 保持线性历史，先按默认来
