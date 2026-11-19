---
title: 【前端面试】五、包管理和Node
tags: 前端面试
index_img: /img/包管理/node.png
cover: /img/包管理/node.png
abbrlink: 323548657
date: 2026-11-19 20:07:34
---


# 一、包管理器

## 1、常见题

### **1. npm、yarn、pnpm 有什么区别？**

npm 是官方包管理器，扁平化把依赖 hoist 到顶层 `node_modules`；yarn（classic）早期以速度快与 `lockfile` 严格著称，后续有 Berry/PnP；pnpm 用**内容寻址存储 + 硬链接 + 符号链接**，磁盘占用小、安装快，并且依赖结构严格（只暴露直接依赖）。选型要点：pnpm 在 monorepo 与依赖治理上更优，npm 兼容性最好，yarn 生态在部分企业项目仍在使用。

### **2. pnpm 为什么既快又省空间？**

所有包按内容寻址存到全局 store（同一份内容只存一次），项目里的 `node_modules/.pnpm/` 只放**硬链接**指向 store；顶层 `node_modules` 里用**符号链接**只链出 `package.json` 声明的直接依赖。因此多个项目共享同一份文件（省磁盘）、安装时几乎只做链接（省时间），并且未声明的依赖无法被访问（避免幽灵依赖）。

### **3. `dependencies`、`devDependencies`、`peerDependencies` 有什么区别？**

`dependencies` 是运行时必需、会被安装到使用者项目；`devDependencies` 只在开发/构建时需要（测试、lint、构建工具），发布时不安装；`peerDependencies` 声明“我需要宿主提供某个包”（如组件库要求 React），用于避免同一库出现多份实例；`optionalDependencies` 允许安装失败而不中断（如平台相关的二进制包）。

### **4. semver 的规则要记住哪些？**

`^1.2.3` → `>=1.2.3 <2.0.0`（锁主版本）；`~1.2.3` → `>=1.2.3 <1.3.0`（锁次版本）；`1.x`、`*`、`>=` 等比较符；**`0.x` 特殊**：`^0.2.3` 只允许 `<0.3.0`（因为 0.x 被视为不稳定，次版本变化即视为破坏性）；预发布版本（`1.0.0-beta.1`）默认不满足普通范围，需要显式声明。

### **5. lockfile 有什么用，要不要提交？**

锁定整棵依赖树的**精确版本与下载地址**（含哈希），保证“本地、CI、生产”安装出完全一致的依赖，避免“今天装和明天装结果不同”的问题。必须提交到仓库、**不允许手改**（冲突时应删掉重新生成）。对应文件：`package-lock.json`、`yarn.lock`、`pnpm-lock.yaml`。

### **6. `npm install` 和 `npm ci` 有什么区别？**

`npm install` 会读 `package.json` 解析依赖、可能更新 lockfile、可增量安装；`npm ci` 严格按 lockfile 安装、先删除 `node_modules`、lockfile 与 `package.json` 不一致时直接报错、速度更快。CI/CD 一律用 `npm ci`（pnpm 对应 `pnpm install --frozen-lockfile`），保证可复现。

### **7. `npm install` 具体做了什么？**

读取 `package.json` → 解析依赖树（考虑版本范围、`peerDependencies`、冲突）→ 查本地缓存或向 registry 请求元数据与 tarball → 校验完整性（`integrity` 字段）→ 解压到 `node_modules` 并按规则 hoist → 执行 `preinstall/install/postinstall` 等生命周期脚本 → 更新 lockfile。排查安装问题要按这条链路分段定位（解析错、网络错、脚本错）。

### **8. `peerDependencies` 冲突是什么情况，怎么处理？**

组件库声明 `react: ^17 || ^18`，使用方装的是 React 19 时，npm 7+ 会尝试自动安装 peer 并在不满足时报 `ERESOLVE`；pnpm 严格模式下直接报错提示缺失 peer。处理方式：升级依赖或调整 peer 范围（首选）、临时用 `--legacy-peer-deps`（npm 回退旧行为，风险是抑制了真实冲突）、或用 `overrides` 强制统一版本（需验证运行时兼容）。

### **9. 什么是幽灵依赖（phantom dependency）？**

代码里 `import` 了某个包，但 `package.json` 里并没有声明，只因为 npm 把它 hoist 到了顶层 `node_modules` 而“碰巧能用”。风险是：别人升级或调整依赖后突然找不到了，且不同包管理器（pnpm）下直接报错。根治方式是显式声明所有直接使用的包，或用 pnpm 的严格结构提前暴露问题。

### **10. 版本冲突导致同一个包装了两份，会有什么后果？**

后果是**多实例问题**：`React` 两份实例会导致 `invalid hook call`；`dayjs` 两份导致插件注册失效；单例（`EventBus`、`axios 实例`、`class 静态状态`）不共享。排查用 `npm ls <pkg>`、`pnpm why <pkg>` 看谁引入了不同版本，再用 `overrides/resolutions` 统一版本或让上游放宽依赖范围。

### **11. `overrides` / `resolutions` 是干什么的？**

在不修改上游包的前提下，强制整棵依赖树里某个包使用指定版本：npm 用 `overrides`、yarn 用 `resolutions`、pnpm 用 `pnpm.overrides`（可写进 `pnpm-workspace.yaml`）。常用于统一安全修复版本或消解多实例；风险是绕过了上游声明的兼容范围，可能引入隐性破坏，必须配合回归验证。

### **12. `npx` 是干什么的？**

临时下载并执行一个包提供的命令行（装到缓存而非项目），用于一次性工具（`npx create-vite`、`npx serve`）。npm 7 之后由 `npm exec` 提供能力。风险是执行来源不明的包（会跑 `postinstall`），所以生产环境要用固定版本并在受控环境执行。

### **13. 私有源和企业内网怎么配？**

`.npmrc` 配置 `registry` 与按 scope 分流：`@company:registry=[https://npm.company.com](https://npm.company.com)`，`always-auth=true`（带鉴权）、`strict-ssl` 与自签证书处理。常见方案是 Verdaccio（轻量自建）、Nexus/Artifactory（企业制品库）。要点是**scope 前缀**必须统一，避免公网包与内网包同名。

### **14. monorepo 里 workspace 怎么用？**

pnpm 用 `pnpm-workspace.yaml` 声明 packages 目录，包之间用 `workspace:*` 互相引用（发布时自动替换为实际版本号）；用 `--filter` 精准执行（`pnpm --filter @app/web build`）；共享依赖提升到根 `package.json` 以便统一版本，`pnpm -r` 做递归任务。要点是“依赖能复用、但边界要清晰”。

### **15. `package.json` 里 `main`、`module`、`exports`、`types` 分别干什么？**

`main` 是 CJS 入口（老工具链依赖）、`module` 是 ESM 入口（供 Rollup/Webpack 优先选）、`exports` 是现代**唯一权威**的子路径导出与条件导出（`import/require/types`，一旦声明就屏蔽其他路径）、`types` 指向类型声明、`files` 控制发布哪些文件（白名单）、`sideEffects` 影响 tree shaking、`engines` 约束 Node 版本。

### **16. 依赖安全要注意什么？**

`npm audit` 基于漏洞库检测已知问题（`--fix` 可能引入破坏性升级，`--force` 需人工评估）；更关键的是**供应链风险**：恶意 `postinstall` 脚本、typosquatting（近似包名）、依赖劫持、维护者账号被盗。实践：锁版本 + 提交 lockfile、CI 用 `--ignore-scripts` 处理不可信依赖、内部源开启白名单与制品扫描、关注 `npm publish` 的 2FA。

### **17. 安装慢/失败怎么排查？**

先看源（`npm config get registry`，是否被代理或私有源覆盖）；再网络（代理、`strict-ssl`、公司证书）；再依赖本身（`optionalDependencies` 里的二进制包在平台不匹配、`postinstall` 编译失败需要构建工具链）；同时注意版本解析耗时（过宽的 semver 范围会拉大量元数据）。手段：`--verbose` 看细节、`npm cache verify` 清缓存、优先用 pnpm 的硬链接减少 IO。

### **18. 发包流程要注意什么？**

版本语义（`npm version patch/minor/major` 按变更性质选）、`files` 白名单避免把源码/测试/大资源发出去、`prepublishOnly` 里跑构建与测试、私有包加 scope 与访问控制、用 `changesets`/`lerna` 管理多包版本与 changelog、先 `npm pack` 检查产物再 `publish`、必要时用 `--dry-run` 与 `--tag next` 灰度。

## 2、难题

### **1. pnpm 的目录结构为什么能同时解决幽灵依赖与路径兼容？**

实际包体放在 `node_modules/.pnpm/<name>@<version>/node_modules/<name>`，同一版本的多个使用者共享这一份（通过硬链接复用到全局 store）；每个包的依赖通过**它自己目录下的符号链接**指向兄弟目录，因此“某包只能访问自己的声明依赖”；项目顶层 `node_modules` 只符号链接直接依赖，所以未声明就找不到（消除幽灵依赖）。同时因为路径仍是 `node_modules` 下的真实位置，绝大多数工具无需特殊适配（相比之下 Yarn PnP 要改解析器）。

### **2. npm 7+ 自动安装 peer 带来了什么新问题？**

自动安装让“忘记装 peer”不再报缺失，但会引入两类问题：一是**版本冲突放大**（多个包对同一 peer 要求不同，直接 `ERESOLVE` 失败）；二是**隐式提升版本**（自动装了最新的 peer，与你项目显式版本不一致，导致多实例）。应对：优先修上游的 peer 范围、用 `--legacy-peer-deps` 仅作兜底、或明确把 peer 提升为项目 `dependencies` 并统一版本。

### **3. semver 范围有哪些反直觉的陷阱？**

`^0.0.x` 几乎等于固定版本（`^0.0.3` 只允许 `<0.0.4`）；`~` 与 `^` 在 0 主版本下行为差异更明显；预发布版本不会被普通范围选中（`^1.0.0` 不会装 `1.1.0-beta`）；`*` 与 `latest` tag 会让构建不可复现；同一依赖被不同的范围约束时，解析器可能保留多份以满足所有约束（这也是重复依赖的根源）。所以“锁 lockfile + 收窄范围”才是可复现的根本。

### **4. lockfile 在多人协作中为什么容易出错？**

它记录整棵树的精确解析结果，只要有人改动 `package.json` 或换包管理器/换 registry，就会产生大范围 diff；手工解决冲突几乎必然产生不一致的树（甚至出现“本地能跑、CI 报错”）。规范做法：统一包管理器与版本（`packageManager` 字段 + `corepack`）、冲突时接受任意一侧后重新执行安装生成、CI 用 `--frozen-lockfile` 强制校验，禁止手改。

### **5. `workspace:\*` 发布时会怎么处理？**

在 monorepo 内部用 `workspace:*`（或 `workspace:^1.2.3`）表示“引用本地包”，安装时走符号链接、不走 registry；打包发布时由包管理器把该协议替换成解析后的真实版本范围（如 `^1.2.3`），从而发布出去的包能被外部正常安装。若工具链不识别该协议（旧 npm 或某些发布脚本），会出现“发布产物里残留 `workspace:*`”的事故，所以发布前要检查 `npm pack` 的 `package.json`。

### **6. dependency confusion 是怎么回事，怎么防？**

内网使用了一个未加 scope 的包名（如 `internal-utils`），而公网上恰好有同名包（恶意者抢注），当解析器按“先公网后私有”或配置错误时，就会安装到公网恶意版本并执行其 `postinstall`。防御：私有包强制加 `@company/` scope 并在 `.npmrc` 明确 `@company:registry`、源配置里把私有源置于优先、CI 中禁止匿名访问公网同名包、开启依赖白名单与制品扫描。

### **7. Yarn PnP（Plug'n'Play）的原理与取舍？**

它取消 `node_modules`，把依赖以 zip 存进 `.yarn/cache`，用 `.pnp.cjs` 维护“哪个包能解析到哪个文件”的映射表，通过 patch Node 的解析器生效。优点是安装极快、零 IO 展开、严格（不可能出现幽灵依赖）、缓存可版本化；代价是生态兼容性差（大量工具直接读 `node_modules` 路径而失败，需要 `pnpify`/SDK）、调试与报错路径不直观、要求团队统一工具链。

### **8. 为什么不该随意加 `overrides`？**

`overrides` 会强行绕过上游声明的 semver 范围，等于替上游做了兼容性承诺：可能修好了冲突，却引入 API 变化导致的运行时错误（常见于 `lodash`、`@types/*`、`axios` 这类跨版本行为有差异的包）。使用原则：只用来修安全问题或消解已确认的多实例问题、每次加都要写明原因与到期时间、配合完整回归与依赖扫描，能通过升级上游解决时优先升级。

# 二、Node

## 1、常见题

### **1. Node.js 是什么，和浏览器里的 JS 有什么不同？**

Node 是基于 V8 的**服务端运行时**：同样跑 JS，但没有 DOM/BOM，提供 `fs`、`net`、`http`、`process` 等能力；全局对象是 `global` 而非 `window`；模块体系长期以 CJS 为主（现在 ESM 已完整支持）；事件循环实现也不同（有明确的阶段划分与 `setImmediate`、`process.nextTick`）。所以“JS 是同一门语言，运行环境不同”。

### **2. 为什么说 Node 是“单线程”的，又怎么能高并发？**

单线程指的是**主线程执行 JS 是单线程**（没有多线程竞争与锁）。高并发靠：非阻塞 IO（发起后立即返回，由内核或 libuv 线程池完成实际工作）+ 事件循环回调 + 每个连接的内存开销远小于线程模型。所以它擅长 IO 密集（网关、API 聚合、实时通信），不擅长 CPU 密集（会阻塞整条事件循环）。

### **3. 事件循环有哪些阶段？**

`timers`（`setTimeout/setInterval` 到期回调）→ `pending callbacks`（部分系统级 IO 错误回调）→ `idle/prepare`（内部准备）→ `poll`（等待并执行 IO 回调；没有其他任务时会在此等待）→ `check`（`setImmediate`）→ `close callbacks`（`socket.on('close')`）。每个阶段之间会清空**微任务队列**（先 `process.nextTick` 队列，再 Promise 队列）。

### **4. `process.nextTick`、`setImmediate`、`setTimeout` 有什么区别？**

`nextTick` 不属于任何阶段，在当前操作结束后**立即**执行，优先级高于 Promise 微任务，递归调用会饿死 IO；`setImmediate` 在 `check` 阶段执行，即本轮循环末尾；`setTimeout(fn, 0)` 在 `timers` 阶段执行（实际受最小延迟与系统时钟影响）。在主模块里 `setImmediate` 与 `setTimeout(fn,0)` 顺序不确定，但在 IO 回调内部 `setImmediate` 一定先执行。

### **5. 阻塞与非阻塞、异步 IO 的原理？**

阻塞调用会占住线程直到完成；Node 的异步 IO 分两类：一类走**内核异步机制**（Linux 的 epoll、macOS 的 kqueue、Windows 的 IOCP，用于网络），一类走 **libuv 线程池**（`fs`、`dns.lookup`、`crypto`、`zlib` 等没有内核异步接口的操作，默认 4 个线程）。理解这条分界能解释“为什么读文件默认是异步的、但某些加密操作会拖慢并发”。

### **6. CJS 与 ESM 在 Node 中的区别？**

CJS：`require` 同步加载、运行时解析、有 `module.exports/exports`、可用 `__dirname`、可被条件加载；ESM：`import` 静态解析、异步加载、顶层 `this` 为 `undefined`、无 `__dirname`（用 `import.meta.url` 推导）、必须写扩展名、支持顶层 `await`。两者混用需注意：ESM 可 `import` CJS，CJS 引用 ESM 需要 `import()`（新版 Node 已支持部分场景的 `require(esm)`，仍建议项目内统一）。

### **7. `require` 的查找与缓存机制？**

查找顺序：内置模块 → 以 `/` `./` `../` 开头的相对或绝对路径（补全扩展名与目录 `index`）→ `node_modules` 逐级向上查找（先看 `package.json` 的 `main`，再看 `index`）。加载后按**解析后的绝对路径**缓存到 `require.cache`，同一模块只执行一次；删除缓存项可实现热重载式重新加载（生产不建议）。

### **8. CJS 的循环依赖会怎样？**

A `require` B，B 又 `require` A 时，B 拿到的 A 是**尚未执行完的 `module.exports`**（部分导出），此时访问未赋值的属性得到 `undefined`。表现为“有时能拿到、有时是空对象”。对策：把共享依赖抽到第三个模块、把使用延后到函数调用时（而非模块顶层），或用依赖注入。

### **9. Buffer 是什么，为什么需要它？**

Buffer 是 Node 用于处理二进制数据的类型（底层是堆外的定长内存，继承自 `Uint8Array`），因为 JS 字符串只适合文本、且 `TypedArray` 原生支持不够早。常用：`Buffer.from`、`alloc/allocUnsafe`（后者不初始化，更快但可能读到旧数据）、`concat`、`toString`、`byteLength`。注意大 Buffer 会占用外部内存，可能触发 GC 压力。

### **10. Stream 有哪几种，解决什么问题？**

四种：`Readable`（可读）、`Writable`（可写）、`Duplex`（双向，如 socket）、`Transform`（读写间转换，如压缩）。价值是**流式处理**：大文件、大响应不需要一次性读进内存，边读边处理，内存恒定。配合 `pipeline`（`stream/promises`）自动处理错误传播与销毁，比手写 `pipe` + 监听更可靠。

### **11. 背压（backpressure）是什么？**

消费者处理速度跟不上生产者时，如果没有节制，数据会在内存中堆积。Stream 用 `highWaterMark` 控制缓冲阈值：`writable.write()` 返回 `false` 表示缓冲已满，生产者应暂停，等 `'drain'` 事件再继续；`Readable` 侧对应 `pause/resume` 与 `pipe` 的自动调节。工程含义是“流式接口必须处理背压，否则内存会涨到 OOM”。

### **12. Node 的错误处理有哪些要点？**

同步错误用 `try/catch`；回调风格遵循“错误优先”（`(err, data)`）；Promise 用 `catch` 或 `unhandledRejection` 监听兜底；`uncaughtException` **只应记录日志后退出**（此时进程状态可能不一致，继续运行不可靠）；异步链用 `pipeline`/`domain`（已废弃）/`AsyncLocalStorage` 追踪上下文。原则是：错误要能在请求边界被捕获并转成响应，而不是让进程崩溃。

### **13. `child_process` 的 `spawn/exec/fork` 有什么区别？**

`spawn` 适合长时间运行、输出流式处理、参数不经 shell（更安全）；`exec` 走 shell、有缓冲区上限（超限会截断/报错），适合简单命令；`fork` 专门启动一个 Node 子进程并建立 IPC 通道（可 `send/on('message')`）。执行外部命令时优先 `spawn` + 参数数组，避免命令注入。

### **14. `worker_threads` 和 `cluster` 分别解决什么？**

`worker_threads` 解决**CPU 密集**：在进程内创建真正的并行线程，可通过 `SharedArrayBuffer` 共享内存、用 `MessagePort` 通信，适合图像处理、加解密、大计算。`cluster` 解决**多核利用与高可用**：起多个进程共享同一端口（主进程监听后分发连接），任一进程崩溃不影响其他，适合无状态 Web 服务。容器化部署时常改用“多副本 + 外部负载均衡”代替 cluster。

### **15. Express 与 Koa 的中间件模型有什么区别？**

Express 是**线性回调链**：`(req, res, next)`，调用 `next()` 进入下一个，错误用 `next(err)` 跳到错误中间件；Koa 是**洋葱模型**：`async (ctx, next) => { await next(); ... }`，`await next()` 之前的逻辑下行、之后的上行，请求耗时统计、统一响应包装非常自然。Koa 更轻（不内置路由），Express 生态更大、约定更“老派”。

### **16. 数据库连接池与事务怎么用？**

连接池（`mysql2` 的 `createPool`）复用连接，关键参数是 `connectionLimit`（匹配数据库上限与并发量）、`acquireTimeout`、`idleTimeout`。事务必须在**同一个连接**上执行：从池中取连接 → `beginTransaction` → 执行多条语句 → `commit/rollback` → `release`；用 `try/finally` 保证异常时回滚并归还连接（否则连接泄漏，池会被耗尽）。

### **17. 文件上传怎么处理？**

前端 `multipart/form-data` 提交；服务端用 `multer`/`busboy` 流式解析，避免把整个文件读进内存；落盘时用流写入（`createWriteStream`）或直接转存对象存储；校验文件大小、类型（不能只信 `Content-Type`）、文件名（防路径穿越）；分片上传则按片写入临时目录后合并。要点是“全程流式 + 严格校验”。

### **18. 如何做优雅关闭？**

收到 `SIGTERM`（容器停止信号）后：停止接受新连接（`server.close()`）、等待在途请求完成（设置超时强制关闭）、清理资源（关闭数据库连接池、Redis、定时器、消息消费者）、最后 `process.exit(0)`。配套要有健康检查端点（`/healthz`）让负载均衡提前摘流量；`SIGINT` 用于本地调试，`SIGKILL` 不可捕获。

### **19. Node 性能优化从哪里下手？**

先确认瓶颈类型：CPU（看 `--prof`/火焰图，是否某段计算阻塞）还是 IO（等待时间占比）。常见手段：避免在主线程做同步调用（`readFileSync`、大 JSON 解析）、把 CPU 任务移到 `worker_threads`、用连接池与批量查询消除 N+1、加缓存（内存/Redis）、开启压缩与 keep-alive、调 `UV_THREADPOOL_SIZE`（IO 密集型且大量 `fs/crypto` 时可适当增大）、用流式响应降低内存峰值。

### **20. Node 服务常见的安全风险有哪些？**

SQL 注入（必须参数化查询，不拼字符串）、命令注入（`spawn` 传参数数组、不经 shell）、路径穿越（`path.resolve` 后校验是否在允许目录内）、SSRF（代理类接口要限制目标白名单）、越权（不要只依赖前端传的 userId）、敏感信息泄露（日志脱敏、错误响应不返回堆栈）、暴力请求（限流与验证码）、反序列化与 `eval`（避免执行不可信输入）。

## 2、难题

### **1. 为什么 `process.nextTick` 可能“饿死”IO？**

`nextTick` 队列在每个阶段之间、甚至每个回调之后都会被完整清空，且优先级高于 Promise 微任务。若回调里递归调用 `nextTick`，队列永远清不完，事件循环就无法推进到下一阶段（`poll` 阶段永远进不去），表现为服务完全不响应但 CPU 满载。同理，长微任务链也会阻塞。改用 `setImmediate` 可以把任务推到下一轮，避免独占。

### **2. libuv 线程池的大小如何影响并发？**

默认 4 个线程处理 `fs`、`dns.lookup`、`crypto`（如 `pbkdf2`、`scrypt`）、`zlib`。若业务大量依赖这些操作（比如每次请求都读文件、做密码哈希），请求会在线程池排队，延迟随并发线性恶化。可通过 `UV_THREADPOOL_SIZE` 调大（启动前设置，过大反而增加上下文切换）；也可以改用异步 IO 替代（`dns.resolve` 走内核、缓存哈希结果、减少同步依赖）。

### **3. Node 里怎么实现真正的并行计算？**

用 `worker_threads`：每个 worker 有独立的事件循环与 V8 实例，可并行执行 CPU 任务；数据通过 `postMessage`（结构化克隆，开销随数据量增长）传递，大数组可用 `SharedArrayBuffer` + `Atomics` 零拷贝共享。实践要点：worker 池化复用（避免反复创建）、任务切分粒度合理（太小则通信开销占比过高）、错误与超时要有兜底（worker 崩溃要能重启）。

### **4. Stream 的背压机制在代码层面是怎么运作的？**

`Writable` 维护内部缓冲，`write()` 同步返回布尔值：返回 `false` 表示缓冲已达到 `highWaterMark`，此时生产者应停止写入并监听 `'drain'`；`pipe()` 内部自动处理这个信号（`false` 时 `pause()` 源，`drain` 时 `resume()`）。`Readable` 侧通过 `_read` 按需拉取。问题常出在自定义 Stream 没用 `_write` 的 `callback` 正确回调，导致“看起来没背压、实际内存暴涨”。

### **5. `cluster` 为什么能让多个进程共享同一端口？**

主进程调用 `listen()` 后，把监听句柄（文件描述符）通过 IPC 传给子进程，子进程直接在该句柄上 `accept`，因此多个进程可以监听同一端口（非 Windows 平台默认用轮询方式分发连接）。这也意味着“连接分配策略”不在你的控制之下（可能不均匀），需要靠连接数监控与压测确认；容器化场景通常改为多副本 + Service 负载均衡。

### **6. 内存泄漏在 Node 里通常怎么产生、怎么查？**

常见来源：全局变量/单例缓存无上限、闭包持有大对象（如把整份响应挂在长期存活的回调上）、事件监听器未移除（`MaxListenersExceededWarning` 就是信号）、定时器未清理、Buffer/大数组在队列里堆积、`Map` 用对象做键但从不删除。排查：`process.memoryUsage()` 观察趋势 → 用 `heap snapshot` 对比多次快照找增长对象 → `--inspect` 接 Chrome DevTools 看 retainers → 结合 `AsyncLocalStorage`/请求 id 定位泄漏源。

### **7. 为什么 Node 服务里“同步代码”代价这么大？**

主线程单线程，任何同步阻塞（`readFileSync`、大循环、`JSON.parse` 巨大字符串、复杂正则回溯、同步 `crypto`、`while` 轮询）都会让事件循环停摆，此时所有并发请求一起排队，表现为“并发一高，P99 直接爆炸”。判断标准是“这段代码的最坏耗时 × QPS”是否可接受；超过几毫秒就该考虑异步化、分片或搬到 worker。

### **8. Node 的事件循环与浏览器的事件循环有什么不同？**

浏览器把任务粗分为宏任务/微任务，渲染穿插其中；Node 有明确的**六阶段**（timers / pending / poll / check / close 等），并额外有 `process.nextTick` 队列（优先于 Promise 微任务），以及 `setImmediate` 专有阶段；Node 没有渲染阶段，但有 `poll` 阶段的“空闲等待”行为。另外 Node 的 `setTimeout(fn, 0)` 最小延迟在实践中更接近 1ms，且 `setImmediate` 与 `setTimeout` 的顺序在主模块里不确定。面试常考的就是这种“同名 API 语义不同”。