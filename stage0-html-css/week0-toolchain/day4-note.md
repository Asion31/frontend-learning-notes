# stage0-html-css / week0-toolchain / Day4 笔记

> 日期：2026-11-22

## 今日任务

分支与冲突：branch、checkout、merge、一次真实的合并冲突演示与解决；综合练习：把已有练习仓库完整走一遍（建分支 → 改代码 → 提交 → 合并 → 推送）

## 一、分支是什么，为什么要用

分支 = **指向某次提交的可移动指针**。在分支上做的提交，不影响其他分支，直到你主动合并。
好处：新功能 / 实验性改动在独立分支上做，出问题直接删分支，主干始终可用。

| 命令 | 作用 | 今天实际用了什么 |
|---|---|---|
| `git switch -c <分支名>` | 创建并切换到新分支（老写法 `git checkout -b`） | `git switch -c feature/day4-lab` |
| `git branch -vv` | 列出所有分支和它们指向的提交 | 看到 feature 和 main 指向同一个提交，后来各自前进 |
| `git switch main` | 切回 main 分支 | 关键：切分支会**换掉工作区文件** |
| `git merge <分支>` | 把指定分支合并进**当前分支** | `git merge feature/day4-lab` |
| `git branch -d <分支>` | 删除已合并的分支（`-D` 是强制删未合并的） | 合并成功后删掉 feature 分支 |
| `git log --graph --oneline` | 用图形看分支与合并关系 | 看到了分叉和回合的图形 |

## 二、冲突是怎么产生的

今天完整走了一遍：

1. main 上建实验文件并提交（`f5bbd8b`）
2. 从它开出 `feature/day4-lab` 分支，把文件第 2 行改成"这是 feature 分支的写法"，提交（`31cc221`）
3. 切回 main，把**同一行**改成"这是 main 分支的写法"，提交（`d4b1085`）
4. 在 main 上合并 feature → **冲突**

```
$ git merge feature/day4-lab
Auto-merging stage0-html-css/week0-toolchain/src/day4-branch-lab.md
CONFLICT (content): Merge conflict in stage0-html-css/week0-toolchain/src/day4-branch-lab.md
Automatic merge failed; fix conflicts and then commit the result.
```

原因：两个分支从同一个起点出发，**改了同一行**。Git 能自动合并"你改 A 行、我改 B 行"这种情况，但同一行改出两个版本，它不知道该留哪个，只能停下等人决定。

## 三、冲突文件里的三个标记

```markdown
<<<<<<< HEAD                 ← 当前分支（合并时你所在的分支）的版本开始
- 第二行：这是 main 分支的写法
=======                      ← 分隔线
- 第二行：这是 feature 分支的写法
>>>>>>> feature/day4-lab     ← 被合并分支的版本结束
```

解决有三条路，任选其一：

1. **只留自己的**（HEAD 那段），删掉另一半和三个标记
2. **只留对方的**，同理
3. **两边都留**（今天用这个），把两行都保留，只删标记

我最后写成了：

```markdown
- 第二行（main 的版本）：这是 main 分支的写法
- 第二行（feature 的版本）：这是 feature 分支的写法
- 结论：冲突就是 Git 不知道该留哪行，需要人决定；这里保留了两边的写法
```

然后 `git add .` → `git commit`（这次不用写 `-m` 也行，Git 会给出默认的 merge 提交信息）→ 合并完成。

## 四、最终历史长这样

```
*   ad31f4a 【练习】Day4 解决合并冲突：保留两边写法
|\
| * 31cc221 【练习】feature 分支改写第二行
* | d4b1085 【练习】main 分支改写第二行
|/
* f5bbd8b 【练习】Day4 建立分支冲突实验文件
```

两条线从同一个点分出去，最后在 `ad31f4a` 汇合 —— 这就是"合并提交"（merge commit），它的特点是有两个父提交。

## 五、两条保命命令

| 情况 | 命令 |
|---|---|
| 合并到一半发现搞错了，想回到合并前 | `git merge --abort` |
| 冲突太复杂，想从头来 | 同上，先 abort 再说 |

冲突时不要慌：文件里所有 `<<<<<<<`、`=======`、`>>>>>>>` 必须全部删干净，否则会留在代码里（这是最常见的低级错误）。

## 卡点与解决（STAR 素材）

- **S 情境**：在自己仓库里人为制造一次合并冲突：两个分支修改同一行，再合并
- **T 目标**：亲手走完"冲突出现 → 读懂标记 → 手动解决 → 提交完成"的完整流程
- **A 做法**：在 main 建实验文件 → 开 feature 分支改第 2 行 → 切回 main 改同一行 → merge 时 Git 报 `CONFLICT (content)` 并拒绝自动合并；打开文件看到 HEAD 与 feature 两段用 `<<<<<<<`、`=======`、`>>>>>>>` 分隔，手工改成"两边都保留"，`git add .` 后 commit 完成合并
- **R 结果**：`git log --graph` 能看到两条分支线在合并提交处汇合，`git status` 恢复 clean；以后再遇到冲突不会慌，知道先 abort 兜底、再按需选版本

## 代码位置

- `stage0-html-css/week0-toolchain/src/day4-branch-lab.md`（实验文件，保留作为练习痕迹）

## 待补 / 疑问

- `merge` 与 `rebase` 的区别：merge 保留分叉历史（有 merge commit），rebase 把提交挪到对方后面、历史是一条直线。团队里 push 过的分支不要 rebase
- 冲突解决后 `git add` 是"标记冲突已解决"，不是重新暂存文件内容 —— 但本质上对 Git 是同一步
