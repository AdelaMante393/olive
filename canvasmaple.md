# ResourceHarbor

ResourceHarbor 是一个面向技术内容创作者、本地化工程师与多媒体爱好者的高质量外链资源聚合平台。该项目不托管任何实体文件或数据流，仅作为公开可用、经过人工筛选与机器可用性验证的在线资源索引系统存在。项目定位为“技术的资源导航工具”，服务于需要快速定位特定语言字幕、视听素材辅助链接或在线播放辅助页面的开发型用户与小型团队，帮助其降低在复杂网络环境中人工查找可用资源的时间成本。

ResourceHarbor 通过定时可用性探测、来源域名信誉评估与内容类型标签化，将分散的在线资源入口整理为结构化数据，并以只读 API 与静态站点导航页的形式对外提供查询服务。项目本身不涉及版权内容存储、分发或代理转发，所有外链指向的资源均由其原始提供方负责内容维护与合法性。ResourceHarbor 只解决“能否找到”的问题，不解决“能否获取”或“是否授权”的问题。

## 功能概览

- **多维度资源标签索引**：按照资源类型、语言类别、内容格式与可用性状态建立标签体系，支持按标签组合筛选外链记录。
- **可用性主动监测**：针对收录的每一个外链域名或 URL 路径，项目后台任务会按可配置周期执行 HTTP HEAD/GET 轻量探测，记录响应状态码与响应时间，自动标记疑似失效节点。
- **只读查询 API 服务**：提供基于 RESTful 风格的只读接口，支持按域名关键词、标签、更新日期范围查询已收录的资源记录，返回结构化 JSON 数据。
- **静态导航页面生成**：项目内置模板引擎，可将资源索引数据渲染为纯静态 HTML 导航页面，便于直接部署至 CDN 或对象存储服务，实现低延迟访问。
- **人工审核工作流**：新资源提交或已有资源信息变更均进入审核队列，审核通过后方可对外可见，确保索引质量与来源可靠性。
- **变更历史追踪**：记录每条资源记录的创建时间、最近修改时间、可用性状态变化日志，支持回溯特定时间点的索引快照。
- **标签别名与合并**：支持对同义标签（如“字幕”与“subtitle”）进行别名映射或强制合并，避免索引碎片化，提升查询召回率。
- **批量导入与导出**：支持 CSV 与 JSON Lines 格式的资源记录批量导入，以及按条件筛选后的数据导出，便于与其他内部工具集成。

## 应用场景

- **本地化工程团队的辅助素材查找**：本地化项目经理在准备多语言媒体内容时，可通过 ResourceHarbor 快速检索特定语种的字幕相关在线资源入口，减少在搜索引擎中反复尝试不同关键词组合的时间。
- **开源多媒体播放器项目的测试链路补充**：播放器开发者需要定期验证播放器对在线字幕加载协议的支持情况，可利用 ResourceHarbor 获取持续可用的测试资源链接，用于集成测试环境中的模拟加载。
- **技术博客与教程中的外部引用示例**：技术作者在撰写关于流媒体协议、字幕格式解析或网络请求重试策略的文章时，可使用 ResourceHarbor 中稳定收录的示例链接作为代码示例中的占位 URL，避免文档中的示例链接快速失效。
- **小型内容聚合站点的数据上游**：个人开发者或小型团队搭建自己的影视资讯或资源推荐站点时，可将 ResourceHarbor 的只读 API 作为数据上游，定期同步公开索引记录，减少自身维护外链清单的工作量。
- **网络可用性监控系统的测试目标池**：运维人员可将 ResourceHarbor 提供的资源域名列表作为外部探测任务的测试目标池之一，用于模拟不同地域、不同运营商环境下的域名解析与连通性监控。

## 快速开始

以下步骤适用于 Linux / macOS / Windows WSL2 环境，需预先安装 Git、Node.js 18.x 或更高版本以及 pnpm。

```bash
# 1. 克隆项目仓库
git clone https://github.com/resource-harbor/resource-harbor.git
cd resource-harbor

# 2. 安装项目依赖
pnpm install

# 3. 复制环境变量模板并填充必要配置
cp .env.example .env.local

# 4. 初始化本地 SQLite 数据库并导入预置的资源索引种子数据
pnpm run db:migrate
pnpm run db:seed

# 5. 启动开发服务器（包含 API 服务和静态导航页预览）
pnpm run dev
```

执行完毕后，本地开发服务器默认监听 3000 端口。可访问 <code>http://localhost:3000/api/resources</code> 验证 API 服务是否正常返回 JSON 数据，或访问 <code>http://localhost:3000</code> 预览静态导航页面。

## 安装要求

| 依赖组件 | 必需版本或规格 | 说明 |
|---|---|---|
| Node.js | 18.x 或 20.x LTS 版本 | 项目运行时与构建工具链的基础环境，推荐使用官方二进制分发或 nvm 管理 |
| pnpm | 8.x 或 9.x | 替代 npm 的包管理工具，利用硬链接机制加速依赖安装并节省磁盘空间 |
| SQLite | 3.35.0 或更高版本 | 默认内嵌数据库引擎，用于存储资源索引、标签、探测记录与审核状态，无需额外安装服务进程 |
| Git | 2.30 或更高版本 | 用于克隆代码仓库及后续拉取项目更新，建议配置 SSH 密钥以简化认证操作 |
| 可用磁盘空间 | 至少 500 MB | 用于存放项目源码、node_modules 依赖包、SQLite 数据库文件及构建生成的静态页面产物 |
| 内存 | 至少 1 GB 可用 | 开发模式下内存占用峰值约 800 MB，生产构建模式约 1.2 GB，推荐 2 GB 以上以获得流畅体验 |
| 操作系统 | Linux (glibc 2.28+) / macOS (11+) / Windows 10+ (WSL2) | 项目未针对原生 Windows CMD 进行充分测试，强烈建议使用 WSL2 或类 Unix 环境 |
| 网络访问 | 能够访问公共 npm 仓库与 GitHub Releases | 用于下载依赖包及可选的外部探测目标，部分探测任务可能需要目标域名的 DNS 解析权限 |

## 文档导航

| 层面 | 目录 / 文档路径 | 回答的问题 |
|---|---|---|
| 用户手册 | <code>docs/user-guide/query-syntax.md</code> | 如何使用 API 的过滤参数组合来精确查找资源记录？标签筛选与关键词搜索的优先级如何工作？ |
| 运维手册 | <code>docs/ops/deployment.md</code> | 如何将 ResourceHarbor 部署至生产环境？支持哪些部署方式（独立进程、容器化、Serverless）？环境变量有哪些关键项？ |
| 开发指南 | <code>docs/dev/architecture.md</code> | 项目的整体分层架构是怎样的？核心模块（采集器、探测器、索引器、API 层）之间的依赖关系和数据流向如何？ |
| 数据格式规范 | <code>docs/specs/resource-schema.md</code> | 资源记录的完整 JSON Schema 定义包含哪些字段？每个字段的校验规则和默认值是什么？自定义标签的命名约束有哪些？ |
| 测试策略 | <code>docs/testing/integration-test.md</code> | 如何运行集成测试套件？Mock 外部 HTTP 依赖的标准做法是什么？新增探测目标时如何编写对应的测试用例？ |
| 扩展开发 | <code>docs/extend/custom-probe.md</code> | 如果默认的 HTTP 探测逻辑不满足需求，如何实现自定义探测策略（例如基于 Puppeteer 的渲染探测）并注册到探测调度器中？ |

## 资源列表

以下为 ResourceHarbor 项目当前收录并持续监测的全部外部资源入口。所有 URL 均按照用户原始提供形式原样列示，项目本身不对这些资源的可用性、内容合法性或访问速度做出任何明示或暗示的保证，仅作为公开可访问的地址索引。

**类别：在线字幕资源（域名入口）**

<code>zuixinzhongwenzimuzaixian.org.cn</code>

<code>zhongwenzaixianguankanshipin.org.cn</code>

<code>renqizaixianmianfeishipin.org.cn</code>

<code>zhongwenzimuzaixianyingyuan.org.cn</code>

<code>zhongwenzimuzaixiankanpian.org.cn</code>

<code>mianfeishipinzhongwenzimu.com.cn</code>

<code>zaixianzhongwenzimuwangzhan.com.cn</code>

## 项目结构

```
resource-harbor/
├── apps/
│   ├── api/                         # RESTful API 服务应用（Fastify 框架）
│   │   ├── src/
│   │   │   ├── routes/              # 路由定义：resources, tags, probes, audit
│   │   │   ├── controllers/         # 请求处理器，包含参数校验与响应格式化
│   │   │   ├── services/            # 核心业务逻辑：查询构建、标签聚合、状态计算
│   │   │   └── plugins/             # 插件：数据库连接、CORS、请求日志、错误处理
│   │   └── package.json
│   ├── web/                         # 静态导航页面生成器（Vite + React 服务端渲染）
│   │   ├── src/
│   │   │   ├── pages/               # 页面组件：首页、资源列表页、详情页、关于页
│   │   │   ├── components/          # 可复用 UI 组件：标签云、资源卡片、探测状态徽章
│   │   │   ├── hooks/               # 数据获取与状态管理 hooks
│   │   │   └── utils/               # 日期格式化、URL 安全校验、分页计算工具
│   │   └── package.json
│   └── probe/                       # 后台可用性探测调度器（独立 Worker 进程）
│       ├── src/
│       │   ├── scheduler/           # 基于 node-cron 的周期性任务调度
│       │   ├── checker/             # 具体探测执行器：HTTP 请求、超时处理、重试策略
│       │   ├── storage/             # 探测结果写入数据库的适配器
│       │   └── metrics/             # 探测成功率、响应时间分位数统计
│       └── package.json
├── packages/
│   ├── db/                          # 数据库模型、迁移脚本与种子数据（Knex.js + SQLite）
│   │   ├── migrations/              # 按时间戳命名的 schema 变更文件
│   │   ├── seeds/                   # 初始资源记录、预置标签与审核状态数据
│   │   └── models/                  # 表结构映射与基础 CRUD 操作封装
│   ├── shared-types/                # TypeScript 类型定义与 Zod 校验模式，供 API 与 Web 共享
│   │   └── src/
│   │       ├── resource.ts          # Resource 实体类型定义
│   │       ├── probe-result.ts      # 探测结果记录类型
│   │       └── audit-log.ts         # 审核日志类型
│   └── utils/                       # 通用工具库：日志、重试、延迟、URL 规范化、域名解析辅助
├── docs/                            # 全部技术文档与用户手册（Markdown 格式）
│   ├── user-guide/
│   ├── ops/
│   ├── dev/
│   ├── specs/
│   └── testing/
├── scripts/                         # 运维辅助脚本：数据库备份、种子数据刷新、健康检查
├── .env.example                     # 环境变量配置模板
├── docker-compose.yml               # 本地开发与生产镜像编排（包含 Redis 缓存可选依赖）
├── Dockerfile                       # 多阶段构建镜像，用于 API 和 Web 服务的容器化部署
├── package.json                     # 根目录工作区配置（pnpm workspace）
├── tsconfig.base.json               # 基础 TypeScript 编译配置
└── README.md                        # 本文件
```

## 贡献指南

ResourceHarbor 欢迎外部贡献者提交新的资源收录建议、现有记录的可用性反馈以及项目代码本身的改进。所有贡献均需遵循以下流程：

1. **提交资源收录建议**：通过项目 GitHub Issues 页面新建一个 “Resource Suggestion” 类型的 Issue，填写资源入口 URL（必须与用户原始提供格式一致）、资源语言类型、内容格式标签以及简短的来源说明。项目维护者将在 5 个工作日内进行初审与可用性验证，通过后合并至种子数据并进入常规探测队列。

2. **报告失效资源或状态异常**：若发现某个已收录资源连续不可用或响应内容发生明显变化，请提交 “Broken Resource Report” 类型的 Issue，附上最近可用的时间点、当前返回的 HTTP 状态码或错误信息截图。探测调度器会在收到报告后触发一次额外的即时探测，并将结果记录至审计日志。

3. **改进文档或翻译**：文档更新通过 Pull Request 方式提交，请基于 <code>docs</code> 目录下的对应文件进行修改，并在 PR 描述中说明本次改动解决的具体阅读障碍或信息缺失问题。所有文档变更需通过 Markdown 语法检查与链接有效性检查。

4. **代码修复与功能增强**：代码贡献需先 fork 主仓库，在本地功能分支上完成开发并通过全部单元测试与集成测试后，提交 Pull Request 至 <code>main</code> 分支。提交前请确保已运行 <code>pnpm run lint</code> 与 <code>pnpm run test</code>，且新增代码覆盖率达到 80% 以上。PR 描述中应引用相关的 Issue 编号。

5. **安全漏洞报告**：如发现任何与资源链接处理、用户输入校验或探测执行相关的安全风险，请勿在公开 Issue 中透露细节，而是发送加密邮件至项目维护组的安全联络邮箱，项目方将在 48 小时内确认接收并开始修复流程。

## 常见问题

**Q：ResourceHarbor 是否存储或缓存所指向资源的实际内容（例如字幕文件或视频流）？**

A：不存储。ResourceHarbor 仅维护外链 URL 的元数据（地址、标签、最近探测时间与状态），在任何情况下都不会下载、缓存、转发或代理所指向资源的实际数据内容。所有探测任务仅执行轻量级的 HTTP HEAD 请求或极小字节范围的 GET 请求，仅用于验证目标服务器是否可达以及响应是否正常，不会拉取完整文件。用户通过 ResourceHarbor 获取的任何外链地址，其访问行为与直接访问原始提供方无异。

**Q：为什么某些被收录的资源链接在探测状态中显示为不可用，但仍然保留在索引列表中？**

**A：保留不可用记录是为了提供索引的连续性观察。ResourceHarbor 的策略是标记失效而非立即剔除，因为部分资源提供方可能存在临时维护、区域访问限制或周期性上下线的情况。项目会保留最近 30 天的探测历史记录，并持续以递减频率（从每 30 分钟逐步降低至每 24 小时）重新探测。若连续 7 天均不可用，资源记录会被移至“长期失效”分区并从默认查询结果中隐藏，但保留通过显式参数查询的历史追踪能力，方便用户了解该资源过去的可用模式。如果资源提供方希望彻底删除其域名记录，可通过贡献指南中提到的审核流程提交移除请求。**

**Q：如何在本地开发环境中模拟外部资源的探测超时或错误响应，以测试探测调度器的重试和告警逻辑？**

**A：项目内置了测试辅助工具。在 <code>apps/probe/src/checker/__tests__/mock-server.ts</code> 中提供了一个基于 <code>node:http</code> 的轻量模拟 HTTP 服务器，支持通过查询参数动态配置响应状态码、响应延迟（毫秒级）以及是否返回特定响应头。在运行集成测试时，可以通过设置环境变量 <code>PROBE_MOCK_MODE=true</code> 来使探测调度器将所有外部请求重定向至该模拟服务器的本地地址，从而任意构造各类失败场景（超时、连接拒绝、500 错误、慢响应等）。详细使用方法参见 <code>docs/testing/integration-test.md</code> 中的“Mock 模式”章节。**

## 许可证

MIT License

Copyright (c) 2026 ResourceHarbor Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-08-25 22:00:12
