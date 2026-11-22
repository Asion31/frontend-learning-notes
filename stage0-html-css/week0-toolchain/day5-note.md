# stage0-html-css / week0-toolchain / Day5 笔记

> 日期：2026-11-22

## 今日任务

浏览器 DevTools 面板：Elements 改样式看结构、Console 调试、Sources 打断点单步执行、Network 看懂请求与耗时、Application 看本地存储；用 Network 面板观察一次请求

## 一、五个面板各管什么

| 面板 | 中文名 | 管什么 | 什么时候用 |
|---|---|---|---|
| Elements | 元素 | 看 DOM 结构 + 改样式（临时） | 调布局、查某个样式到底被谁覆盖 |
| Console | 控制台 | 看 `console.log`、直接敲 JS、看报错 | 打印变量、试一段代码、读报错行号 |
| Sources | 源代码 | 打断点、单步执行、看调用栈 | 逻辑跑错了，想知道每一步变量是什么 |
| Network | 网络 | 所有请求的状态码、大小、耗时、响应内容 | 接口报错、页面白屏、加载慢 |
| Application | 应用程序 | localStorage / Cookie / 缓存 | 看本地存了什么、登录态、缓存是否生效 |

打开方式：`F12`，或在页面右键 → 检查。

## 二、实测记录（用自己写的练习页跑出来的真实数据）

练习页：`stage0-html-css/week0-toolchain/src/day5-devtools-demo.html`
页面功能：点按钮 → 往 localStorage 写 `day5_clicks` 计数 → 发一个 GitHub API 请求 → 把结果显示到页面。

### Network 面板（截图实测）

| 名称 | 状态 | 类型 | 发起程序 | 大小 | 时间 |
|---|---|---|---|---|---|
| day5-devtools-demo.html | **200** | document | 其他 | 2.3 kB | 2 ms |
| frontend-learning-notes（GitHub API） | **304** | fetch | day5-devtools-demo.html:73 | 1.2 kB | 773 ms |

底部汇总：2 次请求、已传输 3.5 kB、8.3 kB 资源、DOMContentLoaded 14 ms。

三个要能读懂的点：

1. **200 vs 304**：200 是"服务器返回了完整内容"；**304 是"你缓存里的还是最新的，我不重复发了"**。第一次访问是 200，再次访问同一接口，浏览器带上 `If-None-Match`，服务器判断内容没变就回 304，省流量。页面上照样拿到数据（用缓存那份）。
2. **大小 1.2 kB**：304 只回了响应头，没回正文，所以传输量远小于真实 JSON 体积。
3. **773 ms vs 2 ms**：本地文件 2 ms；跨网络请求要 773 ms。这就是"为什么本地文件秒开、接口慢半天"。

另外要会看两种异常：**404**（地址对了但资源不存在）、**failed**（压根没连上，DNS/断网/被墙）。失败请求在 Network 里一样能看到，这是排查"接口挂了"第一步要看的地方。

### Application 面板（截图实测）

左侧 `本地存储 → file://`，右侧两条记录：

| 密钥 | 值 |
|---|---|
| day5_clicks | 9 |
| token | mock-token-2026 |
| userName | 系统管理员 |

- `day5_clicks` 是练习页点按钮写进去的，点了几次就是几
- **`token` / `userName` 不是这个页面写的**，是之前别的本地页面留下的 —— 因为 `file://` 协议下所有本地 HTML **共用同一个存储空间**，所以能看到别人写的东西

这条是个真坑：本地双击打开 html 时，localStorage 是所有本地页面共享的；等以后跑 `http://localhost` 或真实域名，才是每个"源"（协议 + 域名 + 端口）各自独立的一套房。所以本地调试看到陌生键不用慌，是别的页面留的。

### Elements 面板要点

- 双击右侧 Styles 里的 `color: #2f5597` 改成 `red`，标题立刻变红
- **只在浏览器里生效，刷新就还原，源文件没变** —— 调样式阶段用它快速试验，定下来再写回文件
- Styles 下方的盒模型图：鼠标移上去，页面对应区域会高亮（margin 绿 / padding 黄 / 内容蓝）

### Console 面板要点

```js
1 + 1                                    // → 2
document.getElementById('title').textContent   // → 取到标题文字
```

页面点按钮后输出两条日志，**后面带文件:行号**（`day5-devtools-demo.html:73`），点这个行号能直接跳到 Sources 对应位置 —— 这是最常用的跳转技巧。

故意输入 `foo()` 会得到红色报错，报错第一行就写着错误类型和位置，调试从这儿开始。

### Sources 面板要点

1. 左侧文件树点到 `day5-devtools-demo.html`
2. 在 `function log(msg) {` 这一行点行号，出现蓝点 = 断点
3. 回页面点按钮 → 执行会在这里**暂停**，右侧 Scope 能看到 `msg` 的值
4. 顶部按钮：▶ 继续（F8）、单步跳过（F10）、单步进入（F11）
5. 暂停时在 Console 里输入变量名，能取到当时的真实值

## 卡点与解决（STAR 素材）

- **S 情境**：用自己写的练习页跑 Network 和 Application 面板，观察按钮触发的一次跨域请求和本地存储写入
- **T 目标**：看懂请求的状态、大小、耗时，并确认 localStorage 真的写进去了
- **A 做法**：开着 Network 刷新页面，看到 html 是 200 / 2.3 kB / 2 ms；点按钮后看到 GitHub API 那条是 **304** / 1.2 kB / 773 ms。一开始以为 304 是"请求失败"，查了才明白是"内容未修改、用缓存"，页面上的数据其实拿到了。转头去 Application 看 localStorage，确认 `day5_clicks` 已经写入（值 9），还意外发现 `token`、`userName` 两条陌生记录
- **R 结果**：搞清楚了 200 和 304 的区别，也搞懂 `file://` 下所有本地页面共用一套 localStorage；以后接口调试会先看 Network 的 Status 和 Time，不再靠猜

## 代码位置

- `stage0-html-css/week0-toolchain/src/day5-devtools-demo.html`（练习页，保留）

## 待补 / 疑问

- 为什么要跨域：`file://` 页面请求 `api.github.com` 属于跨域，GitHub API 允许匿名读所以成功了；换成不允许跨域的接口会被 CORS 挡住，这个留到 stage2 讲网络时再说
- `Network` 里 `Preserve log` 的作用：勾上以后刷新页面不清空记录，排查"点完按钮页面跳走了"的情况特别有用
