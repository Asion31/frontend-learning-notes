# stage0-html-css / week0-toolchain / Day1 笔记

> 日期：2026-11-22

## 今日任务

命令行基础：cd / dir(ls) / 相对路径与绝对路径、PATH 环境变量概念；Git 安装与全局配置 user.name / user.email

## 做了什么

- 在 PowerShell 里练习：`cd` 切换目录、`Get-Location` 看当前位置、`Get-ChildItem`（等价 `dir` / `ls`）列目录
- 分清两种路径写法：
  - 绝对路径：`D:\code\frontend-learning-notes`，从盘符开始，在哪都能定位
  - 相对路径：`stage0-html-css\week0-toolchain`，以当前所在目录为起点；`..` 表示上一级
- 排查并解决了一个真实问题：`git` 命令不可用（详见下面 STAR）
- 把 `C:\Program Files\Git\cmd` 追加到用户 Path，新开终端后 `git --version` 输出 `2.45.1`
- 确认仓库状态：分支 `main`、远程 `origin` 指向 GitHub、全局身份 `Asion31 / 583334017@qq.com` 已配置

## 卡点与解决（STAR 素材）

- **S 情境**：在终端里敲 `git --version`，提示 `'git' 不是内部或外部命令`，但 Git 明明已经装过；同一台电脑的 GitHub Desktop 却能用
- **T 目标**：让 `git` 命令在任何目录下都能直接调用
- **A 做法**：先确认程序是否存在——`C:\Program Files\Git\cmd\git.exe` 确实在；再查 `$env:PATH` 里没有这个目录。原因清楚了：敲命令时系统只会去 PATH 列出的文件夹里找同名程序。于是把该目录追加进用户级环境变量 Path，并注意"已打开的终端不会自动刷新环境变量，必须新开窗口"
- **R 结果**：新终端里 `git --version` 正常返回版本号，`git status` 能正确读出仓库状态，问题彻底解决

## 今日记住的三句话

1. `cd` 换目录，`Get-ChildItem` 看目录内容，`pwd` / `Get-Location` 看自己在哪
2. 相对路径靠"现在所在的位置"推出来，`..` 往上跳一级
3. 命令能直接用，本质是"程序所在目录进了 PATH"；改完 PATH 要重开终端

## 代码位置

- 今日无代码练习（纯命令行），练习记录在本笔记

## 待补 / 疑问

- cmd 和 PowerShell 命令不完全一样（`dir` 两边都能用，但 `ls` 只在 PowerShell 里能当别名）——先统一用 PowerShell，后面遇到差异再记
