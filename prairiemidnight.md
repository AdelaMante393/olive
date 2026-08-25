# ResourceBridge

ResourceBridge 是一个面向技术内容创作者、本地化工程师和数字归档工作者的轻量级资源导航与元数据聚合工具。该项目并非传统的爬虫或采集系统，而是一个基于静态规则引擎的链接状态监控与分类管理面板，用于帮助用户系统化整理、标注、分组各类外部网络资源链接，并自动生成结构化的 Markdown 目录与状态报表。

项目主要解决个人或小团队在维护大量外部参考链接时面临的归类混乱、失效不可知、复查成本高等问题。ResourceBridge 不存储任何第三方内容，仅记录用户自定义的标签、备注和访问状态快照，适用于技术文档库的外链治理、多媒体资源索引备份、以及研究型项目的参考条目管理。

## 功能概览

- **链接分组与标签系统**：支持为每条记录分配多个自定义标签，并基于标签自动生成动态分类视图，便于按主题或用途快速筛选。

- **批量导入与去重检测**：支持通过文本或 CSV 格式批量导入链接列表，系统自动检测重复条目并提示合并，减少人工校对时间。

- **定期状态探测**：内置轻量级 HTTP 头探测模块，可按用户设定周期（每日/每周）检查链接的可达性，并记录响应码与延迟变化趋势。

- **Markdown 目录生成器**：根据分组和标签自动生成层级清晰的 Markdown 格式文档，可直接用于项目 README 或 Wiki 页面的外链附录章节。

- **自定义元数据字段**：允许用户为每个链接添加“用途说明”、“维护人”、“关联项目”、“最后复核日期”等扩展字段，满足团队协作场景下的信息同步需求。

- **离线快照备注**：支持为链接附加本地离线存档路径或备注，便于关联内部存储文件而不破坏外部链接的独立性。

- **变更日志追踪**：自动记录每条链接的增删改操作日志，支持按时间范围回溯，便于审计和协作冲突排查。

## 应用场景

- **技术文档库的外链治理**：当开源项目文档中包含大量第三方参考链接时，维护者可通过 ResourceBridge 定期扫描链接有效性，并自动生成更新后的外链清单，避免文档中出现死链。

- **多媒体资源索引备份**：内容创作者或翻译团队在整理影视字幕、配音素材、在线播放源等外部资源时，可使用本工具统一记录链接来源、语言类型和访问状态，防止资源丢失后无法追溯原始地址。

- **研究项目的参考条目管理**：学术研究或行业调研过程中，研究人员往往需要收集数百个数据源链接。ResourceBridge 的自定义元数据功能可帮助标注数据来源、采集日期和访问权限，提高参考资料的规范化程度。

- **团队知识库的外链统一入口**：企业内部的 Confluence 或 Notion 知识库中常分散存放大量外部链接，通过 ResourceBridge 统一维护一个链接索引表，可减少重复引用和链接过期带来的信息偏差。

## 快速开始

以下步骤适用于 Linux / macOS / Windows WSL 环境，确保系统已安装 Git 和 Node.js 20.x 及以上版本。

```bash
# 克隆仓库
git clone https://github.com/resource-bridge/resource-bridge.git
cd resource-bridge

# 安装依赖
npm install

# 复制示例配置文件并修改
cp .env.example .env

# 运行状态探测示例（首次运行将生成初始数据目录）
npm run probe -- --target ./data/sample_links.csv

# 启动本地 Web 管理面板（开发模式）
npm run dev
```

生产环境部署请参考 `docs/deployment.md` 中的说明，使用 `npm run build` 构建静态文件并通过 Nginx 或 Caddy 提供服务。

## 安装要求

| 依赖项 | 必需版本 | 说明 |
|--------|----------|------|
| Node.js | 20.x 或更高 | 运行时环境，用于执行核心引擎和 Web 服务 |
| npm | 9.x 或更高 | 包管理器，用于安装依赖和执行脚本 |
| SQLite3 | 系统自带或自动安装 | 轻量级嵌入式数据库，用于存储链接元数据与状态历史 |
| Git | 2.30 或更高 | 用于克隆仓库和版本管理 |
| curl / wget | 任意稳定版本 | 状态探测模块的备用回退方案，当内置 HTTP 客户端不可用时自动调用 |
| 磁盘空间 | 至少 200 MB | 用于存放数据库文件、日志和临时缓存 |
| 内存 | 建议 512 MB 以上 | 常规使用下 256 MB 亦可运行，但批量导入或大规模探测时建议提升 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户指南 | `docs/user-guide/` | 如何导入链接、配置标签、执行状态探测和生成 Markdown 报告 |
| 配置手册 | `docs/configuration/` | 环境变量含义、探测间隔设置、自定义字段定义方法 |
| 开发者文档 | `docs/development/` | 项目模块划分、插件扩展接口、单元测试编写规范 |
| 运维参考 | `docs/operations/` | 生产环境部署、数据库备份策略、日志轮转与监控告警配置 |

## 资源列表

以下为项目官方维护或推荐的关联资源地址，按类别分组展示。所有地址均按原始格式原样列出。

官方与社区

<code>zaixianbofangw.org.cn</code>

<code>zhubofuliw.org.cn</code>

中文多媒体资源参考

<code>zhongwenzimudianyingw.org.cn</code>

<code>zhongwenzimuwangzhanw.org.cn</code>

<code>zhongwenzimuzaixianmianfeikanw.org.cn</code>

在线播放与观影辅助

<code>zaixianguankanwangyeshipinw.org.cn</code>

<code>zaixianzhongwenzimuw.org.cn</code>

## 项目结构

```
resource-bridge/
├── src/
│   ├── core/                   # 核心引擎模块
│   │   ├── index.ts            # 主入口，初始化数据库和路由
│   │   ├── linker.ts           # 链接解析、标准化与去重逻辑
│   │   └── probe.ts            # HTTP/HTTPS 状态探测与超时控制
│   ├── storage/                # 存储层
│   │   ├── database.ts         # SQLite3 连接池与建表语句
│   │   ├── repository.ts       # CRUD 操作封装
│   │   └── migration.ts        # 版本迁移脚本
│   ├── web/                    # Web 管理面板
│   │   ├── routes/             # Express 路由定义
│   │   ├── middleware/         # 鉴权与日志中间件
│   │   └── static/             # 前端静态资源（Vue 构建输出）
│   ├── generator/              # 文档生成器
│   │   ├── markdown.ts         # Markdown 目录树生成逻辑
│   │   └── formatter.ts        # 自定义模板渲染器
│   └── cli/                    # 命令行工具
│       ├── probe-cmd.ts        # 手动触发探测
│       └── import-cmd.ts       # 批量导入链接
├── data/                       # 用户数据目录（自动创建）
│   ├── links.db                # SQLite 主数据库
│   └── logs/                   # 操作日志与探测历史
├── config/                     # 配置文件模板
│   ├── default.yaml            # 默认配置项
│   └── schema.json             # 配置结构校验
├── tests/                      # 单元测试与集成测试
│   ├── unit/
│   └── integration/
├── docs/                       # 项目文档（含用户指南、API 参考）
├── .env.example                # 环境变量示例
├── package.json                # npm 依赖与脚本
├── tsconfig.json               # TypeScript 编译配置
└── README.md                   # 本文件
```

## 贡献指南

1. 查阅 `docs/development/CONTRIBUTING.md` 了解整体开发流程和编码规范，确保代码风格与项目保持一致（使用 ESLint + Prettier）。

2. 在 GitHub Issues 中查找标记为 `help wanted` 或 `good first issue` 的任务，或提交新 Issue 说明你希望修复的问题或新增的功能，等待维护者确认。

3. Fork 本仓库并创建本地特性分支，例如 `feature/add-export-format` 或 `fix/probe-timeout`，提交代码时请附带相应的单元测试和文档更新。

4. 确保所有现有测试通过（`npm run test`），并新增至少一个用例覆盖你的改动，提交 Pull Request 到 `main` 分支，描述中注明关联的 Issue 编号。

5. Pull Request 经过至少一名维护者 Code Review 后，将合并进入主干，并自动触发 CI 构建和预发布包生成。

## 常见问题

**问：状态探测模块是否会对外部站点造成过大请求压力？**

答：系统默认采用单线程顺序探测，且每个请求之间强制间隔 500 毫秒，同时支持用户配置并发上限（默认 3）。对于重复探测同一域名，系统会缓存 DNS 解析结果 5 分钟，避免频繁解析。建议用户根据自身网络环境调整探测间隔，避免对目标站点造成不必要的影响。

**问：如何迁移现有的书签或收藏夹数据到 ResourceBridge？**

答：项目支持导入 CSV 格式文件，列头需包含 `url`、`title`、`tags` 和 `note`（均为可选）。主流浏览器导出的 HTML 书签可通过第三方工具转换为 CSV，或使用项目 `tools/` 目录下提供的转换脚本。导入前可使用 `--dry-run` 参数预览匹配结果，确认无误后再执行正式导入。

**问：Web 管理面板是否支持多用户登录和权限分级？**

答：当前版本仅提供单用户本地访问模式，默认监听 `127.0.0.1`，不对外暴露服务。如需团队协作，建议通过反向代理配置基础认证（Basic Auth）或使用 OAuth2 Proxy 等网关层方案。多用户原生支持已列入后续里程碑规划。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-25 21:59:37
