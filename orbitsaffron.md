# ChronoLink 导航聚合系统

ChronoLink 是一个面向技术内容创作者与数字档案管理者的高可靠性外链资源聚合与导航平台。系统通过自动化健康检查、智能分类引擎与用户行为分析，将零散的网络资源转化为结构化的可追溯知识库，解决个人与团队在信息收集过程中面临的链接失效、分类混乱与检索低效三大核心痛点。

本项目定位于中大型开源社区文档站、技术博客聚合站以及个人知识管理（PKM）系统的底层导航基础设施。目标用户包括 DevOps 工程师、技术文档撰写者、开源社区维护者以及需要长期维护大量外链参考资源的科研人员。ChronoLink 不提供任何形式的内容托管或代理转发服务，仅作为 URL 元数据的索引、校验与展示层，严格遵循资源站点的原始协议与域名规范。

## 功能概览

- **多协议裸域名智能识别**：系统自动解析用户输入的 URL 格式，保留原始协议头与域名层级，不对 http、https 或裸域名进行任何形式的规范化改写，确保跳转路径与用户预期完全一致。

- **资源状态实时探测**：内置异步健康检查 Worker，每隔 6 小时对全部收录链接发起 TCP 连接测试与 HTTP 状态码验证，异常结果通过邮件与 Webhook 双通道告警。

- **分类标签与全文检索**：支持为每个资源条目打上最多 5 个自定义标签，配合基于 Elasticsearch 的倒排索引，实现毫秒级的关键词与域名后缀检索（如 .cn、.org）。

- **访问统计分析看板**：记录每个外链的点击次数、最后访问时间与来源 Referer，生成趋势折线图与热力图，辅助管理员识别高价值资源与僵尸链接。

- **批量导入与导出接口**：提供 RESTful API 与 Web 上传界面，支持 CSV、JSON Lines 格式的批量链接导入，导出功能支持 Markdown 列表与结构化 JSON 两种格式。

- **外链变更历史审计**：所有新增、修改、删除操作均记录操作人、时间戳与变更前后差异，审计日志保留 180 天，满足团队内部合规追溯要求。

- **响应式前端展示模板**：内置三套不同色调的卡片式布局主题，自适应桌面、平板与移动设备，列表页支持按域名、分类、状态（正常/异常）快速过滤。

## 应用场景

**技术博客聚合站维护**：个人技术博主或小型内容团队使用 ChronoLink 统一管理所有引用来源、灵感参考站及合作互推链接。系统定时检测失效外链，避免博客文章中出现死链影响读者体验，同时通过点击统计了解读者最感兴趣的外部资源类型。

**开源项目文档站引用管理**：大型开源项目的 README 或官方文档中常包含数十个依赖库、工具站与社区论坛链接。ChronoLink 提供独立的资源导航子页面，文档维护者可一键导出最新有效链接列表，替换旧版文档中的硬编码 URL，降低维护成本。

**数字档案与学术资源索引**：研究机构或图书馆利用 ChronoLink 建立专题资源门户，将分散在多个域名下的开放获取论文、数据集与工具库按主题分类。审计功能记录资源变更历史，确保引用轨迹可追溯，满足学术规范要求。

**企业内部知识库外链治理**：企业 Wiki 或 Confluence 空间中嵌入的大量外部参考链接随时间推移逐渐失效。ChronoLink 作为中间层代理所有外链引用，统一执行健康检查并缓存可用性状态，知识库页面通过 iframe 或 API 调用动态获取最新链接状态，无需逐个手动验证。

## 快速开始

以下步骤适用于 Linux/macOS 环境，Windows 用户建议通过 WSL2 或 Git Bash 执行。

```bash
# 1. 克隆项目仓库
git clone https://github.com/chronolink/chronolink-aggregator.git
cd chronolink-aggregator

# 2. 安装依赖（使用 Python 3.10+ 与 pipenv）
pip install pipenv
pipenv install --dev

# 3. 初始化环境变量与本地数据库
cp .env.example .env
python scripts/init_db.py

# 4. 启动开发服务器（默认监听 8000 端口）
python manage.py runserver --host 0.0.0.0 --port 8000

# 生产环境建议使用 gunicorn 或 uwsgi
# gunicorn -w 4 -k uvicorn.workers.UvicornWorker chronolink.asgi:application
```

访问 http://localhost:8000 即可进入管理控制台，默认管理员账号 admin / admin123（首次登录强制修改密码）。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.10 或 3.11 | 核心运行环境，3.12 暂未完全兼容 |
| PostgreSQL | 14.x 或 15.x | 主数据库，存储资源元数据、审计日志与用户信息 |
| Redis | 7.0+ | 缓存会话、健康检查任务队列与限流计数器 |
| Elasticsearch | 8.5+ | 全文检索引擎，可选启用（不安装则降级为 SQL LIKE 模糊查询） |
| Node.js | 18.x LTS | 仅用于前端静态资源构建（Vue 3 + Vite） |
| Nginx | 1.22+ | 生产环境反向代理与静态文件服务，开发环境可跳过 |
| Supervisor | 4.2+ | 进程守护管理，生产环境推荐用于保持 Worker 持久运行 |
| Git | 2.30+ | 版本控制与自动部署拉取 |
| Docker Compose | 2.15+ | 可选方案，提供一键式容器编排部署 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 用户手册 | /docs/user-guide/ | 如何注册账号、添加链接、创建分类、查看统计报表？ |
| 管理员指南 | /docs/admin-guide/ | 如何配置健康检查间隔、设置告警阈值、管理用户权限？ |
| API 参考 | /docs/api-reference/ | 所有 REST 端点的请求/响应结构、鉴权方式与错误码定义？ |
| 部署运维 | /docs/deployment/ | 如何配置 Nginx 反向代理、设置 HTTPS 证书、执行数据库迁移？ |
| 开发贡献 | /docs/contributing/ | 代码风格规范、提交信息格式、测试用例编写与 PR 流程？ |
| 架构设计 | /docs/architecture/ | 系统模块划分、消息队列设计、缓存策略与水平扩展方案？ |

## 资源列表

本章节收录本系统初始化默认加载的全部外链资源。所有 URL 严格按照用户提供的原始格式呈现，未做任何协议补充、域名改写或路径规范化处理。系统核心能力之一即保证跳转目标与原始输入完全一致，请勿自行添加前缀或变更大小写。

**影视与娱乐类**

<code>zhongwenzimuzaixiankanpian.com.cn</code>

<code>yejianfulishipin.org.cn</code>

<code>guomotaotu.net.cn</code>

<code>miyouzaixianshipin.net.cn</code>

<code>yejianfulishipin.net.cn</code>

<code>zaixianshipinzhongwenzimua.org.cn</code>

<code>zaixianbofangzhongwenzimua.org.cn</code>

## 项目结构

```
chronolink-aggregator/
├── src/                                    # 核心源代码目录
│   ├── api/                                # RESTful API 路由与视图集
│   │   ├── endpoints/                      # 按资源类型划分的端点模块
│   │   ├── middleware/                     # 鉴权、限流、日志中间件
│   │   └── validators/                     # 输入参数校验器（含 URL 格式严格校验）
│   ├── core/                               # 业务逻辑层
│   │   ├── checker/                        # 健康检查引擎（TCP/HTTP/HTTPS 多协议）
│   │   ├── classifier/                     # 基于域名后缀与关键词的自动分类器
│   │   └── analytics/                      # 点击统计与趋势计算模块
│   ├── models/                             # SQLAlchemy ORM 模型定义
│   │   ├── resource.py                     # 资源主表、标签表、状态历史表
│   │   ├── user.py                         # 用户与权限模型
│   │   └── audit.py                        # 操作审计日志模型
│   ├── tasks/                              # Celery 异步任务定义
│   │   ├── health_check.py                 # 周期性的全量链接探测任务
│   │   └── report_generation.py            # 周报/月报统计任务
│   └── utils/                              # 通用工具函数
│       ├── url_parser.py                   # 裸域名保留、协议嗅探、规范化拒绝器
│       └── validators.py                   # 邮箱、IP、域名格式校验
├── frontend/                               # Vue 3 前端工程
│   ├── src/
│   │   ├── components/                     # 可复用 UI 组件（卡片、表格、筛选栏）
│   │   ├── views/                          # 页面级组件（仪表盘、资源列表、审计日志）
│   │   └── stores/                         # Pinia 状态管理（用户会话、过滤条件）
│   └── dist/                               # 构建输出目录（由 Vite 生成）
├── deployment/                             # 部署相关配置
│   ├── docker/                             # Dockerfile 与 Compose 编排文件
│   ├── nginx/                              # Nginx 站点配置模板（含 Gzip 与缓存策略）
│   └── supervisor/                         # Supervisor 进程管理配置文件
├── scripts/                                # 运维与开发辅助脚本
│   ├── init_db.py                          # 初始化数据库表结构与默认管理员账号
│   ├── seed_resources.py                   # 从 resources.json 加载初始外链列表
│   └── export_links.py                     # 导出当前全部链接为 Markdown 或 JSON
├── tests/                                  # 单元测试与集成测试
│   ├── unit/                               # 针对 URL 解析器、分类器、校验器的测试
│   └── integration/                        # API 端到端测试与数据库事务回滚测试
├── docs/                                   # 完整文档源文件（Markdown + MkDocs 配置）
├── .env.example                            # 环境变量配置模板（含数据库连接串与密钥）
├── requirements.txt                        # Python 生产依赖列表
├── requirements-dev.txt                    # 开发与测试额外依赖
├── pyproject.toml                          # 项目元数据与构建系统配置
└── README.md                               # 当前文件
```

## 贡献指南

1. **提交 Issue 讨论**：在 GitHub Issues 中搜索是否存在类似需求或缺陷。若无，则新建 Issue 并选择对应模板（功能增强 / 缺陷报告 / 文档改进），详细描述改动动机与预期效果。核心团队会在 48 小时内回复并标注优先级标签。

2. **派生仓库并创建特性分支**：将主仓库 Fork 至个人账号，然后克隆本地。创建分支时遵循命名规范 `feature/简短描述` 或 `fix/问题编号`，例如 `feature/url-validator-enhance`。禁止直接在主分支或 develop 分支上进行修改。

3. **编写测试与通过 CI 检查**：新增功能必须补充对应的单元测试或集成测试，确保覆盖率不低于 80%。提交前在本地运行 `pytest tests/` 与 `flake8 src/` 检查代码风格。CI 流水线会自动执行测试、安全扫描与构建，所有检查通过后方可进入审查阶段。

4. **发起 Pull Request 并参与审查**：从特性分支向主仓库的 `develop` 分支发起 PR，标题简短总结变更内容，正文中链接关联的 Issue 编号并列出测试结果截图。至少需要一位核心成员 Approve 且无未解决的对话后方可合并。合并后 CI 会自动构建并部署至预发布环境进行 24 小时验证。

5. **更新文档与示例**：任何影响用户使用方式或 API 行为的变化，必须同步更新 `/docs` 下的对应文档以及 `README.md` 中的快速开始或功能说明部分。配置变更需同步修改 `.env.example` 并添加注释。

## 常见问题

**Q: 系统是否会自动将裸域名补全为 www 子域名或添加 https 前缀？**
A: 不会。ChronoLink 设计原则之一是“用户输入即最终跳转目标”。系统内部存储的 URL 字段与用户提交的字符串完全一致，渲染时仅在 `href` 属性中原样输出。若浏览器因缺少协议头无法识别，用户需自行在地址栏或配置中补全。该行为有明确审计日志记录，方便追溯原始输入。

**Q: 健康检查任务对所有链接是否会产生额外的流量或请求开销？**
A: 系统默认对每个链接执行轻量级 TCP 握手检测（仅验证端口可达性），并不请求完整页面内容。对于启用了 HTTPS 的站点，执行 TLS 握手和 HTTP HEAD 请求，收到状态码 2xx 或 3xx 即视为存活。单个链接检测超时设置为 3 秒，并发 Worker 数量为 10，全量扫描 1000 个链接约耗时 5 分钟，平均每天产生不足 1MB 的流量开销。

**Q: 如何迁移现有的书签或收藏夹数据到 ChronoLink？**
A: 支持三种导入方式：一是通过管理后台的“批量导入”页面上传 CSV 文件（列标题为 `url, title, tags`）；二是通过 REST API 的 `/api/import/` 端点发送 JSON Lines 数据流；三是从浏览器导出书签为 HTML 文件后，使用社区提供的转换脚本 `scripts/convert_bookmarks.py` 将 Netscape 格式转为系统兼容的 CSV。导入后系统自动触发一次健康检查以更新初始状态。

## 许可证

MIT License

Copyright (c) 2026 ChronoLink Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-08-25 22:00:21
