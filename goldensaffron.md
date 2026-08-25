# NexusIndex

NexusIndex 是一个面向技术内容创作者、本地化团队与数字归档工作者的轻量级外链资源聚合系统。项目定位为“可自托管的资源导航中间件”，用于解决多源、多格式、多语言外部参考链接在项目协作中的分散、失效与检索效率低下问题。目标用户包括开源文档维护者、跨国软件本地化协调员、技术媒体编辑以及个人知识库管理者。系统通过结构化元数据模板、自动可用性检测与标签化分类，将零散的外部链接转化为可复用、可审计、可共享的组织资产，而非简单收藏夹。

## 功能概览

- **链接元数据模板引擎**：支持为每条外链附加自定义属性，包括语种、内容类型、版权状态、最后验证时间与维护责任人，便于团队统一管理标准。

- **自动可用性健康检查**：内置轻量级 HTTP 状态检测器，可按每日/每周周期自动探测链接可达性，并在仪表板中标红失效链接，减少文档中的“死链污染”。

- **多维度标签与全文检索**：支持为链接打上多级标签（如“字幕”“影视”“在线播放”“镜像站”），并基于标题、描述、标签与备注字段进行中文分词检索，响应时间低于 200 毫秒。

- **批量导入与导出**：支持 CSV 与 JSON 格式的链接批量导入，同时可导出为结构化 Markdown 报告或静态 HTML 目录，便于嵌入项目 Wiki 或 CI 生成站点。

- **访问统计与热度排序**：记录每条链接的点击次数与最后访问时间，支持按周/月聚合热度，辅助团队识别高频使用资源，优化导航层级。

- **权限分级与审计日志**：提供管理员、编辑者、访客三级角色控制，所有增删改操作记录详细审计日志，满足企业级合规要求。

- **RESTful API 与 Webhook 通知**：提供完整的只读与写入 API，可对接外部自动化脚本；支持配置失效链接的邮件或企业微信 Webhook 告警。

## 应用场景

- **技术文档本地化协作**：当多语言文档团队需要引用不同语种的术语库、风格指南或参考视频时，团队可将所有外链统一录入 NexusIndex，并为每个链接标注适用语种与审核状态，避免翻译人员自行搜索导致的版本不一致。

- **开源项目外部依赖归档**：开源项目往往依赖大量第三方教程、API 参考或演示视频。维护者可将这些链接纳入 NexusIndex 并开启自动健康检查，当上游文档迁移或域名变更时，系统提前预警，帮助项目快速更新 README 或官网引用。

- **数字内容策展与选题调研**：技术编辑或自媒体作者在策划系列专题时，需收集大量案例、工具与数据源。NexusIndex 的标签与检索功能可帮助策展人按主题快速重组链接库，并导出为带注释的参考列表附于文章末尾。

- **企业内部培训资源导航**：企业培训部门可将分散在不同部门共享盘或云笔记中的视频教程、文档站点与工具地址统一迁移至 NexusIndex，配合权限分级，仅向对应岗位开放相关链接，同时审计日志可追踪资源使用情况。

## 快速开始

以下步骤将在本地环境启动 NexusIndex 开发实例，默认使用 SQLite 数据库，便于快速体验。

```bash
# 克隆代码仓库
git clone https://github.com/nexusindex/nexusindex.git
cd nexusindex

# 安装依赖（使用 Python 虚拟环境）
python -m venv venv
source venv/bin/activate  # Windows 下使用 venv\Scripts\activate
pip install -r requirements.txt

# 初始化数据库与默认配置
python manage.py migrate
python manage.py loaddata initial_links.json

# 启动开发服务器
python manage.py runserver 0.0.0.0:8000
```

访问 http://127.0.0.1:8000 即可进入仪表板。默认管理员账号 admin / admin123，首次登录强制修改密码。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|----------|----------|------|
| Python | 3.9 - 3.11 | 核心运行时，3.12 暂不支持部分依赖库 |
| PostgreSQL | 13.0 及以上 | 生产环境推荐，开发环境可使用 SQLite 替代 |
| Redis | 6.0 及以上 | 用于缓存检索结果与任务队列，非必需但强烈建议 |
| Node.js | 18.x LTS | 仅用于前端资源构建，后端运行无需 Node |
| Nginx | 1.20 及以上 | 生产环境反向代理与静态文件服务推荐 |
| Supervisor | 4.2 及以上 | 进程守护工具，用于保持 Celery 工作进程持续运行 |
| Docker | 20.10 及以上 | 如需容器化部署，提供官方 Dockerfile 与 Compose 模板 |
| Git | 2.25 及以上 | 用于版本管理与补丁应用 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户指南 | /docs/user-guide/ | 如何注册、添加链接、创建标签、设置健康检查策略 |
| 管理员手册 | /docs/admin-handbook/ | 如何配置权限、调整检测频率、迁移数据库、恢复审计日志 |
| API 参考 | /docs/api-reference/ | 所有 RESTful 端点的请求/响应格式、鉴权方式与错误码 |
| 部署运维 | /docs/deployment/ | 生产环境架构建议、容器化部署步骤、监控指标与备份方案 |
| 贡献者指引 | /docs/contributor/ | 代码风格规范、提交信息格式、测试用例编写与 PR 流程 |
| 常见问题 | /docs/faq/ | 收录社区高频问题，涵盖安装报错、性能调优与自定义开发 |

## 资源列表

本批次收录外部参考链接共计 7 条，按内容主题划分为影视字幕与在线播放辅助类，供系统初始化或测试导入使用。

**影视字幕与在线播放辅助资源**

<code>zaixianbofangw.org.cn</code>

<code>zhubofuliw.org.cn</code>

<code>zhongwenzimudianyingw.org.cn</code>

<code>zhongwenzimuwangzhanw.org.cn</code>

<code>zhongwenzimuzaixianmianfeikanw.org.cn</code>

<code>zaixianguankanwangyeshipinw.org.cn</code>

<code>zaixianzhongwenzimuw.org.cn</code>

## 项目结构

```
nexusindex/
├── manage.py                # Django 项目管理入口
├── requirements.txt         # Python 后端依赖列表
├── docker-compose.yml       # 容器化编排模板（含 Postgres+Redis）
├── .env.example             # 环境变量配置模板（密钥/数据库/缓存）
│
├── backend/                 # 核心应用目录
│   ├── settings/            # 多环境设置（开发/测试/生产）
│   │   ├── base.py          # 通用配置（时区/语言/中间件）
│   │   ├── dev.py           # 开发调试配置（DEBUG=True，SQLite）
│   │   └── prod.py          # 生产配置（DEBUG=False，Postgres+Redis）
│   ├── urls.py              # 主路由分发（API 与前端页面）
│   ├── wsgi.py              # 生产 WSGI 入口
│   └── celery.py            # Celery 应用实例（定时任务配置）
│
├── apps/                    # 功能模块
│   ├── links/               # 链接管理核心模块
│   │   ├── models.py        # Link, Tag, CheckRecord 数据模型
│   │   ├── views.py         # 列表/详情/搜索/导入导出视图
│   │   ├── serializers.py   # DRF 序列化器
│   │   ├── health_check.py  # 可用性探测逻辑（并发请求+超时控制）
│   │   └── tasks.py         # Celery 定时健康检查任务
│   ├── accounts/            # 用户与权限模块
│   │   ├── models.py        # 扩展 User 模型（角色/部门）
│   │   ├── permissions.py   # 自定义权限类（基于角色+资源类型）
│   │   └── backends.py      # 邮箱/用户名双因子认证后端
│   ├── stats/               # 访问统计与热度模块
│   │   ├── models.py        # ClickLog, DailyAggregate
│   │   ├── middleware.py    # 请求拦截埋点中间件
│   │   └── filters.py       # 时间范围/标签过滤实现
│   └── api/                 # RESTful API 聚合入口
│       ├── v1/              # API 版本 v1（支持分页/排序/字段选择）
│       └── throttles.py     # 基于用户角色的访问频率限制
│
├── frontend/                # 前端资源（Vue 3 + Vite）
│   ├── src/
│   │   ├── components/      # 可复用组件（链接卡片/搜索栏/图表）
│   │   ├── views/           # 页面级组件（仪表板/列表/详情/设置）
│   │   ├── stores/          # Pinia 状态管理（链接列表/用户会话）
│   │   └── utils/           # 日期格式化/错误拦截/路由守卫
│   └── dist/                # 生产构建输出目录（由 CI 自动生成）
│
├── tests/                   # 单元测试与集成测试
│   ├── unit/                # 模型/序列化器/工具函数测试
│   ├── integration/         # API 端点端到端测试（含鉴权流程）
│   └── fixtures/            # 测试用初始数据（链接/标签/用户）
│
├── scripts/                 # 运维与辅助脚本
│   ├── backup_db.sh         # 数据库每日备份脚本（pg_dump）
│   ├── migrate_links.py     # 从旧版 CSV 结构迁移至新版模板
│   └── seed_demo_data.py    # 生成演示数据（用于快速展示）
│
└── docs/                    # 项目文档（Markdown 源码）
    ├── user-guide/          # 用户操作手册（含截图）
    ├── admin-handbook/      # 管理员运维手册
    ├── api-reference/       # API 详细说明（OpenAPI 导出）
    └── contributor/         # 贡献者开发指引
```

## 贡献指南

我们欢迎并感谢任何形式的贡献，包括但不限于代码、文档、测试用例与问题反馈。请遵循以下步骤：

1. 在 GitHub Issues 中查找或新建一个议题，简要描述您希望解决的问题或新增的功能，等待维护者确认方向，避免重复劳动或偏离项目路线图。

2. Fork 本仓库，将代码克隆至本地，并按照快速开始章节搭建开发环境。请确保您的开发分支基于最新的 main 分支，且分支命名遵循 `feature/` 或 `fix/` 前缀规范。

3. 编写代码或文档时，请严格遵守项目根目录下的 `.editorconfig` 和 `.flake8` 配置。所有 Python 代码须通过 flake8 与 black 格式化检查，JavaScript/Vue 代码须通过 ESLint 校验。新增功能必须附带单元测试，测试覆盖率不低于 80%。

4. 提交代码时，使用约定式提交格式（Commitizen），例如 `feat(links): add batch import progress bar` 或 `fix(health): handle timeout exception for IPv6 addresses`。确保每个提交逻辑独立、可回滚。

5. 创建 Pull Request 至 main 分支，并填写 PR 模板中的检查清单。维护者将在 3 个工作日内进行 Code Review，通过后即可合并。重大变更需提前在 Issues 中讨论并获得核心维护者批准。

## 常见问题

**Q：系统支持同时管理多少个链接而不影响性能？**

A：在默认 SQLite 配置下，建议链接总数不超过 5000 条，检索响应时间约 150ms。若链接数量超过 2 万条，强烈建议切换至 PostgreSQL 并开启全文索引（GIN），此时可支撑 10 万级链接量，检索时间仍可控制在 300ms 以内。健康检查任务建议使用 Celery 配合 Redis 队列，避免阻塞主请求。

**Q：如何导入我自己现有的书签文件或浏览器收藏夹？**

A：系统目前不直接支持浏览器 HTML 书签格式，但您可以将书签导出为 CSV 文件（包含标题、URL、描述三列），然后在后台“批量导入”功能中上传。我们提供了 CSV 模板下载，请严格按照模板列顺序填充。对于复杂嵌套目录结构，建议先打平为标签体系后再导入。

**Q：链接失效检测会频繁请求外部站点，是否会被目标服务器封禁？**

A：检测模块默认采用单线程顺序请求，间隔至少 5 秒，且 User-Agent 设置为主流浏览器标识。您可以在配置文件中调整 `HEALTH_CHECK_INTERVAL`（默认 24 小时）和 `HEALTH_CHECK_TIMEOUT`（默认 10 秒）。若目标站点有明显反爬策略，您可以将该链接加入白名单，跳过自动检测，改为手动验证。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-25 22:00:15
