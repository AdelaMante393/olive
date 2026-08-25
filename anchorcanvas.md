# LinkHub

LinkHub 是一个轻量级、高可扩展的技术资源外链聚合平台，面向开发者、技术内容创作者以及开源社区维护者，用于集中管理、分类展示和快速检索项目相关的优质外部资源。项目定位为“技术资源的统一入口”，解决多源链接分散、检索效率低、展示不直观等问题，适用于个人知识库、团队文档站或开源项目的资源导航页。

## 功能概览

- **分类目录管理**：支持按技术领域、资源类型、使用频率等多维度对链接进行归类，便于快速定位。
- **全文检索与过滤**：内置关键词搜索功能，支持对链接标题、描述、标签进行实时过滤。
- **链接状态检测**：周期性检查已收录链接的可访问性，自动标记失效或重定向资源。
- **访问统计看板**：记录每个外链的点击次数、最近访问时间，辅助评估资源热度。
- **导入导出机制**：支持批量导入链接列表（CSV/JSON 格式），也支持将当前目录导出为静态站点或 Markdown 文档。
- **用户自定义分类**：允许注册用户创建私有分类，实现个人化资源整理，不干扰全局目录。
- **RSS 订阅更新**：为每个分类生成 RSS Feed，方便关注特定领域资源的用户及时获取新增链接。

## 应用场景

- **开源项目文档站**：作为项目 README 的补充，在项目官网或 Wiki 中嵌入 LinkHub 页面，集中存放依赖库、学习教程、社区论坛等外部参考链接，降低新手入门时的信息搜集成本。
- **技术团队内部知识库**：研发团队可使用 LinkHub 整理常用的中间件文档、运维工具手册、设计规范站点，统一团队技术视野，减少重复沟通。
- **个人技术博客导航**：独立开发者或技术博主可搭建 LinkHub 实例，将自己长期积累的优质资源（如视频教程、在线编译器、API 参考）对外开放，提升博客的专业度和复用价值。
- **技术活动或课程资料集**：适用于技术培训、线上公开课或黑客松活动，讲师将所需的软件下载地址、环境配置指南、示例代码仓库统一收录，参与者可一键获取全部资料。
- **多环境资源镜像站**：针对国内访问国际资源较慢的场景，可配置 LinkHub 收集国内镜像源、加速地址或替代方案，提升开发效率。

## 快速开始

以下操作基于 Linux / macOS 环境，Windows 用户可使用 WSL 或 Git Bash 执行。

```bash
# 1. 克隆项目仓库
git clone https://github.com/your-org/linkhub.git
cd linkhub

# 2. 安装依赖（使用 Python 3.9+ 及 pip）
pip install -r requirements.txt

# 3. 初始化数据库并导入示例数据
python manage.py initdb
python manage.py load-fixtures --sample

# 4. 启动开发服务器
python manage.py runserver --host 0.0.0.0 --port 8080
```

启动后，访问 `http://localhost:8080` 即可查看本地运行的 LinkHub 实例。管理员后台位于 `/admin`，默认账号密码为 `admin / linkhub2024`，请首次登录后立即修改。

## 安装要求

| 依赖 | 必需版本 | 说明 |
|------|----------|------|
| Python | 3.9 - 3.11 | 核心运行环境，3.12 暂未完全兼容 |
| SQLite | 3.28+ | 默认内置数据库，适用于小型部署；生产环境建议 PostgreSQL |
| Redis | 6.0+ | 用于缓存热点链接列表与访问统计计数器 |
| Node.js | 16.x 或 18.x | 仅前端构建工具需要，运行时可忽略 |
| Nginx | 1.18+ | 推荐用于反向代理与静态资源缓存（生产环境） |
| Git | 2.25+ | 版本控制及自动更新脚本依赖 |
| Memcached | 1.5+ | 可选，用于替代 Redis 作为会话缓存 |
| Elasticsearch | 7.17+ | 可选，用于更高级的全文搜索功能 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户指南 | `/docs/user-guide/` | 如何注册、创建分类、添加链接、使用检索和 RSS 功能 |
| 管理员手册 | `/docs/admin-manual/` | 如何管理用户权限、批量导入导出、配置失效检测策略 |
| 部署运维 | `/docs/deployment/` | 如何部署到生产环境（Docker / K8s / 传统虚拟机）、性能调优参数 |
| 开发者文档 | `/docs/developer/` | 如何二次开发、扩展自定义分类器、增加新的统计维度 |
| API 参考 | `/docs/api/` | 所有 RESTful API 的端点定义、请求示例与错误码说明 |
| 变更日志 | `/docs/changelog/` | 每个版本的特性新增、缺陷修复与不兼容变更说明 |

## 资源列表

本部分收录本项目的所有外部关联资源，按用途分类展示。所有 URL 严格遵循用户原始数据输出。

### 中文影视资源类（示例分类，用于展示聚合能力）

- <code>guochanzhubozaixianguankan.org.cn</code>
- <code>zhongwenshipin.org.cn</code>
- <code>zaixianbofangzhongwenzimu2.org.cn</code>
- <code>zhongwenzimugaoqing.org.cn</code>
- <code>renqimianfeishipin.org.cn</code>
- <code>zhongwenzimuzaixiankan.org.cn</code>
- <code>zuixinzhongwenzimu.org.cn</code>

## 项目结构

```text
linkhub/
├── app/                            # 核心应用目录
│   ├── controllers/                # 控制器层，处理请求路由与视图逻辑
│   ├── models/                     # 数据模型定义 (User, Category, Link, ClickLog)
│   ├── services/                   # 业务服务层，包含检索、检测、统计等核心功能
│   ├── templates/                  # Jinja2 模板文件，前端页面渲染
│   └── static/                     # 静态资源 (CSS, JS, 图片)
│       ├── css/                    # 基于 Tailwind 构建的自定义样式
│       └── js/                     # 原生 JavaScript 模块，负责前端交互
├── config/                         # 配置文件目录 (development, production, testing)
├── migrations/                     # 数据库迁移脚本 (Alembic 管理)
├── scripts/                        # 运维工具脚本，包含数据导入导出、备份、检测任务
├── tests/                          # 单元测试与集成测试 (pytest)
│   ├── unit/                       # 模型与服务的单元测试
│   └── integration/                # API 与数据库交互的集成测试
├── logs/                           # 日志存储目录 (按日切割)
├── requirements.txt                # Python 生产依赖列表
├── requirements-dev.txt            # 开发环境额外依赖 (测试、文档生成)
├── Dockerfile                      # 容器化构建文件
├── docker-compose.yml              # 本地容器编排 (含 Redis + PostgreSQL)
├── Makefile                        # 常用命令快捷方式 (如 make init, make test)
├── README.md                       # 项目总览与快速入门 (即本文档)
└── LICENSE                         # MIT 许可证文件
```

## 贡献指南

我们欢迎并感谢任何形式的贡献，包括但不限于问题反馈、功能建议、代码提交和文档改进。请遵循以下步骤：

1. **提交 Issue**：在 GitHub Issues 中描述您发现的问题或期望的新功能，请使用提供的模板，并注明复现步骤或使用场景。
2. **Fork 仓库**：将主仓库 Fork 至您的个人账号，并 clone 到本地开发环境。
3. **创建功能分支**：从 `main` 分支切出新分支，命名遵循 `feature/描述` 或 `fix/描述` 格式，例如 `feature/add-elasticsearch-support`。
4. **编写代码与测试**：确保新增代码覆盖相应的单元测试，并运行 `make test` 确保全部测试通过。保持代码风格与现有 PEP8 规范一致。
5. **提交 Pull Request**：推送到您的 Fork 仓库后，向主仓库的 `main` 分支发起 PR。请在 PR 描述中关联对应的 Issue 编号，并简述您的修改内容与影响范围。

## 常见问题

**Q：LinkHub 是否支持自定义域名或二级目录部署？**  
A：支持。您可以在 `config/production.py` 中设置 `BASE_URL` 和 `STATIC_URL` 前缀。若使用 Nginx 代理，请注意配置 `proxy_set_header` 保证转发路径正确。二级目录部署时需额外调整 `static` 与 `media` 的路径映射。

**Q：如何迁移现有的书签或收藏夹到 LinkHub？**  
A：目前支持从 Chrome 导出的 HTML 书签文件（`bookmarks.html`）以及通用 CSV 格式（列：标题，URL，分类，标签）。在管理后台的“导入”界面选择对应文件类型，系统会自动解析并映射到当前用户下。对于大规模迁移（超过 5000 条），建议使用命令行脚本 `scripts/batch_import.py`。

**Q：链接状态检测会影响被检测网站的正常访问吗？**  
A：不会。检测采用 HEAD 请求方式，仅获取响应头信息，不下载完整页面内容。检测频率默认每小时一次，且可配置为仅对近期点击量较高的链接进行高频检测，以降低对第三方站点的请求压力。您也可以在配置中完全关闭自动检测功能，改为手动触发。

## 许可证

本项目采用 MIT 许可证进行分发。您可以在遵守许可证条款的前提下自由使用、修改、复制和再发布本软件。详细信息请查阅项目根目录下的 `LICENSE` 文件。

> 外链数量: 7 | 生成时间: 2026-08-25 21:59:37
