# TechStack Navigator

TechStack Navigator 是一个面向开发者与技术决策者的技术资源导航与聚合平台。该项目不直接生产内容，而是通过人工筛选与社区贡献，系统性地整理互联网上优质的技术文档、开发工具、学习路径与架构案例，帮助技术团队在选型与调研过程中快速定位高价值信息源。

该项目主要解决以下问题：技术生态碎片化导致信息过载，优质内容分散于不同语言与地区的站点，缺乏统一索引；新手开发者难以从海量资料中识别权威与时效性兼具的参考材料；技术管理者在架构评审与技术选型时缺乏可追溯的对比依据。TechStack Navigator 通过主题分类、版本追踪与社区评分机制，为上述场景提供结构化支持。

## 功能概览

- **多维度资源索引**：按编程语言、基础设施、前端框架、数据工程、运维监控等十余个技术域建立分类目录，每个目录下聚合至少二十条精选外链，附带简注与适用版本范围。

- **版本兼容性矩阵**：针对常见技术栈组合（如 Spring Boot + MySQL、React + Next.js、Kubernetes + Istio），以表格形式列出各版本组合的已知兼容性问题与官方测试报告链接。

- **社区活跃度看板**：集成 GitHub 星标趋势、Stack Overflow 问题增长率、邮件列表响应时长等外部指标，帮助判断项目的维护健康度。

- **自定义收藏集**：用户可创建公共或私有收藏集，用于团队内部共享调研结论或学习路径，支持批量导出为 Markdown 或 JSON 格式。

- **变更订阅通知**：对关注的资源链接提供变更检测，当目标文档版本号更新、页面 404 或重定向时，通过邮件或 Webhook 发送告警。

- **内容快照归档**：对关键文档（如官方入门指南、API 参考）提供定期快照，防止原文下线导致信息丢失，快照存储于分布式对象存储中。

- **标签与全文检索**：支持中文与英文分词检索，可按标签、分类、更新时间、贡献者多条件过滤，检索响应时间控制在 200 毫秒以内。

## 应用场景

- **技术选型前期调研**：架构师在引入新中间件时，可通过本项目的分类目录快速获取官方文档、社区最佳实践、性能对比报告以及已知缺陷清单，将调研周期从数天压缩至数小时。

- **新员工技术 onboarding**：团队为新入职开发者制定学习计划时，可直接引用本项目中的学习路径收藏集，覆盖从语言基础到生产级部署的完整链条，避免重复整理资料。

- **离线环境依赖预置**：对于部署在隔离网络环境下的生产系统，运维团队可参考本项目的依赖清单与版本校验和，预先准备所需的二进制包与容器镜像，减少上线过程中的网络与权限问题。

- **技术文档版本追溯**：当上游文档突然更新且未保留历史版本时，可通过本项目的快照归档功能回溯至上一版本，用于对比变更差异，评估升级风险。

- **社区方案横向对比**：在多个开源项目间进行对比时（如 API 网关选型、日志收集器选型），可利用本项目维护的对比矩阵，覆盖功能特性、性能基线、商业支持情况等维度。

## 快速开始

以下操作以 Linux/macOS 环境为例，Windows 用户可使用 WSL2 或 Git Bash 执行。

```bash
# 1. 克隆仓库
git clone https://github.com/techstack-navigator/navigator.git
cd navigator

# 2. 安装依赖（使用 Python 3.10+ 与 pipenv）
pip install pipenv
pipenv install --deploy

# 3. 初始化本地数据库与索引
pipenv run python manage.py migrate
pipenv run python manage.py build_index --full

# 4. 启动开发服务
pipenv run python manage.py runserver --host 0.0.0.0 --port 8080
```

启动后访问 http://localhost:8080 即可浏览本地实例。生产环境部署请参考 `docs/deployment.md` 中的容器化与反向代理配置。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.10, 3.11, 3.12 | 核心运行环境，低于 3.10 不支持模式匹配语法 |
| PostgreSQL | 14.x 或 15.x | 主数据库，存储资源元数据、用户收藏及评分，需启用 pg_trgm 扩展以支持中文模糊检索 |
| Redis | 7.0+ | 用于会话缓存、频率限制和异步任务队列，需开启持久化 |
| Elasticsearch | 8.5+ | 全文检索引擎，索引字段需配置 ik 分词器（中文）与 standard 分词器（英文） |
| MinIO / S3 | 兼容 AWS S3 API | 用于存储文档快照及用户上传的附件，需提供 bucket 读写权限 |
| Node.js | 18.x 或 20.x | 仅用于前端静态资源构建（Vite + React），生产环境无需运行 |
| Nginx | 1.22+ | 推荐作为反向代理，处理 TLS 终结与静态资源缓存，非强制但强烈建议 |
| Docker Compose | 2.20+ | 用于本地开发环境一键拉起所有依赖服务，生产环境可选用 |

## 文档导航

| 层面 | 目录路径 | 回答的问题 |
|---|---|---|
| 用户手册 | `docs/user-guide/` | 如何注册、创建收藏集、订阅更新、使用检索与筛选功能 |
| 管理员指南 | `docs/admin-guide/` | 如何审核新提交的资源链接、管理分类标签、处理举报与快照清理策略 |
| 开发者文档 | `docs/developer-guide/` | 如何扩展新的资源源（如 RSS 抓取）、自定义评分算法、编写迁移脚本 |
| 架构设计 | `docs/architecture/` | 系统整体模块划分、数据流图、扩展性设计、灾备与降级方案 |
| API 参考 | `docs/api-reference/` | RESTful API 端点列表、请求/响应示例、鉴权方式与限流阈值 |
| 运维手册 | `docs/operations/` | 监控指标暴露、日志采集配置、备份恢复步骤、容量规划建议 |
| 贡献规范 | `CONTRIBUTING.md` | 代码风格、提交信息格式、PR 流程、测试覆盖率要求 |

## 资源列表

以下为本项目当前收录的原始资源链接，按类别分组展示。所有链接均保持用户提供的原始格式，未做任何修改。

分类：在线视频播放与资源站点

- <code>zaixianbofangnidongdea.org.cn</code>
- <code>shipinmianfeizaixianguankanb.org.cn</code>
- <code>rihanzaixianmianfeishipinb.org.cn</code>
- <code>mianfeizhuijuwangzhanb.org.cn</code>
- <code>zaixianbofangzhongwenzimuc.org.cn</code>
- <code>shipinmianfeizaixianguankanf.org.cn</code>
- <code>rimanzaixianguankanf.org.cn</code>

以上链接将在本项目的「外链质量监测」模块中定期检查可用性与内容变更，并纳入社区评分体系。用户可通过提交 Issue 或 Pull Request 更新链接状态。

## 项目结构

```
navigator/
├── backend/                           # Python 后端服务（FastAPI + SQLAlchemy）
│   ├── api/                           # REST 路由定义，按领域拆分 version1/
│   ├── core/                          # 配置管理、安全上下文、数据库引擎
│   ├── models/                        # SQLAlchemy ORM 实体，含资源、标签、用户、快照
│   ├── services/                      # 业务逻辑层：索引服务、快照服务、订阅服务
│   ├── tasks/                         # Celery 异步任务：爬取更新、发送邮件、清理过期快照
│   └── utils/                         # 通用工具函数：URL 规范化、版本解析、哈希计算
├── frontend/                          # 前端单页应用（React + TypeScript + Vite）
│   ├── src/                           # 源码目录：组件、页面、状态管理（Zustand）、API 客户端
│   ├── public/                        # 静态资源：favicon、robots.txt、站点验证文件
│   └── index.html                     # 应用入口模板
├── infrastructure/                    # 基础设施即代码与编排配置
│   ├── docker/                        # Dockerfile 与镜像构建上下文（后端、前端、nginx）
│   ├── kubernetes/                    # K8s 部署清单：deployment、service、ingress、configmap
│   └── terraform/                     # AWS/GCP 资源定义：RDS、ElastiCache、S3 Bucket
├── docs/                              # 全部文档，按 user/admin/dev/architecture 分目录
├── scripts/                           # 运维与开发辅助脚本：数据迁移、索引重建、测试数据生成
├── tests/                             # 单元测试与集成测试，按 backend 与 frontend 分离
├── .github/                           # GitHub 工作流定义：CI 测试、代码扫描、自动构建镜像
├── Makefile                           # 常用任务快捷命令（install、lint、test、run）
└── README.md                          # 本文件
```

## 贡献指南

欢迎社区参与资源补充、代码改进与文档完善。请遵循以下步骤：

1. **查找未分配任务**：在 Issues 中筛选 `help wanted` 或 `good first issue` 标签，或自行提议新增分类与资源。重大变更建议先通过 Issue 讨论，避免无效 PR。

2. **本地环境准备**：按照快速开始章节搭建开发环境，并运行 `make pre-commit` 安装 git pre-commit 钩子（含 black、isort、eslint 自动修复）。

3. **提交资源更新**：若新增外链，请在 `backend/models/fixtures/` 下的对应分类 JSON 文件中追加条目，包含 `url`、`title`、`description`、`tags` 字段，并运行 `make validate-links` 进行可用性预检。

4. **编写测试与文档**：任何新功能或修复必须附带至少一个单元测试（`tests/` 目录），并更新对应文档章节。文档变更需同时提供中文版本，英文版本可后续由维护者补充。

5. **发起 Pull Request**：PR 标题遵循 `<type>(<scope>): <subject>` 格式（如 `feat(backend): add link validation endpoint`），描述中关联相关 Issue 编号。CI 通过且获得至少一名 maintainer 的 approve 后合并。

## 常见问题

**Q: 项目中的外链如果失效或内容被篡改，如何快速发现？**

A: 系统后台运行每日定时任务，对所有已收录的外链执行 HEAD 请求与内容哈希抽样比对。当状态码非 2xx/3xx 或内容变化超过阈值时，自动标记为「待审核」并通知管理员。用户也可在资源详情页点击「报告问题」手动标记。

**Q: 能否离线使用本导航系统？**

A: 平台核心功能（浏览、检索、查看快照）支持完全离线运行，前提是已经完成初始数据同步与索引构建。用户收藏和评分等写入操作在离线模式下会存入本地 IndexedDB，网络恢复后自动与后端合并。如需完整离线包，可使用 `manage.py export_offline_bundle` 生成静态站点。

**Q: 如何将私有部署的数据与上游社区仓库同步？**

A: 私有部署实例默认不会自动拉取上游数据。若需同步，可在管理后台配置上游仓库地址（默认为本项目官方仓库），并启用「定期合并」开关。合并策略为增量追加，不会覆盖本地已修改的自定义条目。建议在测试环境中先行试合并，确认无误后再应用于生产实例。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-25 22:00:17
