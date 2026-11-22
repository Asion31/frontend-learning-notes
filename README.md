# frontend-learning-notes

> 前端学习打卡仓库，主线目标：**Vue3 + Vite + 前端工程化**
> 结构风格与 Java 学习仓库保持一致：`stageX-weekY`，每个 week 独立 `src` + 每日笔记。

## 一、学习主线

| 阶段 | 内容 | 周数 |
|---|---|---|
| stage0 | 工具链（Git / 命令行 / DevTools）+ HTML + CSS 基础 | 5 |
| stage1 | JavaScript 基础（类型 / 函数 / 原型 / 内置对象） | 5 |
| stage2 | DOM 与 BOM（事件机制、定时器、小案例） | 3 |
| stage3 | ES6+（解构、Promise、模块化、Set/Map） | 3 |
| stage4 | Vue 3 核心（响应式、组件通信、Router、Pinia） | 6 |
| stage5 | 工程化与生态（Vite 配置、Scss/Tailwind、Axios） | 3 |
| stage6 | 综合项目实战（立项 → 开发 → 优化 → 部署复盘） | 4 |
| stage7 | 进阶与面试（TS、浏览器原理、手写题、八股） | 4 |

合计 33 周（stage0 含 week0 工具链准备），按周推进。

## 二、目录结构

```
frontend-learning-notes
├─ README.md                 仓库总说明（本文件）
├─ .gitignore
├─ docs/                     拓展文档、思维导图
├─ notes/                    通用笔记、摘抄、面试卡片
├─ algo/                     前端算法（JS 版）
│   └─ by-pattern.md         题型索引：题目 / 卡点 / 重刷标记
├─ stage0-html-css/
│   ├─ README.md             阶段说明 + 周次清单
│   ├─ week0-toolchain/      工具链准备（Git / 命令行 / DevTools）
│   ├─ week1-html-basic/
│   │   ├─ README.md         本周打卡清单（带勾选框）
│   │   ├─ src/              本周练习代码
│   │   └─ day1-note.md ... day6-note.md
│   └─ week2-css-selector/ ...
├─ stage1-js-basic/
├─ stage2-dom-bom/
├─ stage3-es6plus/
├─ stage4-vue3-core/
├─ stage5-engineering/
├─ stage6-project-practice/
└─ stage7-advance/
```

## 三、每日流程（照这个走就行）

1. 打开本周 `README.md`，勾掉当天任务
2. 代码写在 `weekX-xxx/src/` 下（Vue 阶段直接是完整工程目录，就放在该 week 文件夹里）
3. 当天笔记写 `dayX-note.md`（和 src 同级），记：做了什么 / 卡点 / 怎么解决
4. JS 算法 0~2 题（可选），登记到 `algo/by-pattern.md`
5. 提交：`【frontend/<stage>/<week>】<一句话说明>`

## 四、规则

- **Java 主线优先**：Java 当天任务没完成，前端直接跳过
- 每日算法是**可选**，状态好写 1~2 题，状态差写 0 题，不硬凑
- 笔记只写自己的话，不抄大段文档
- 每个 week 结束时补一次复盘，把踩过的坑写进 notes/

## 五、当前进度

| 阶段 / 周 | 状态 |
|---|---|
| stage0-week0-toolchain | 进行中（Day1） |
| 其余 32 周 | 未开始 |

每周的具体打卡项看该周 `README.md`，完成一天就勾掉一天。

## 六、Git 工作流（本仓库约定）

```powershell
git status                 # 看有没有改动
git add -A                 # 存到暂存区
git commit -m "【frontend/stage0-week0】Day1 命令行与 Git 配置"
git push                   # 推送到 GitHub
```

- 一天一次提交，message 带 `【frontend/<阶段>-<周>】`
- 分支只用一个 `main`，暂时不搞复杂分支策略
- `node_modules/`、`dist/` 一律不提交（已在 `.gitignore` 里）
