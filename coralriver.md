# DIT - Distributed Information Tracker

DIT (Distributed Information Tracker) 是一个面向技术内容聚合与资源导航的开源元数据索引系统，专为需要高效检索、分类管理和快速访问互联网公开视频与字幕资源的开发者和内容研究者设计。项目定位于技术资源外链汇总与结构化信息管理，解决信息分散、链接失效快、检索效率低等痛点，提供一套自托管的轻量级资源目录框架。

系统核心能力包括多维度资源标签管理、链接可用性健康检查、自定义分类视图生成以及静态站点元数据导出。DIT 不存储任何实际媒体文件，仅维护资源定位符（URL）与描述性元数据，确保项目合规且轻量化。目标用户包括开源社区文档维护者、数字图书馆建设者、媒体分析研究人员以及希望建立个人资源收藏夹的高级用户。

## 功能概览

- **元数据目录管理** 支持对资源链接添加标题、描述、标签、语言、地区、质量等级和最后验证时间等结构化字段，便于后续过滤与排序。

- **自动化健康检查** 内置周期性 HTTP HEAD/GET 探测模块，可配置超时和重试策略，自动标记失效链接并生成可用性报表，减少人工维护成本。

- **多级分类与标签系统** 提供无限层级的分类树和扁平标签体系，允许同一资源归属多个分类，支持按分类路径或标签组合进行快速筛选。

- **静态站点生成器** 内置模板引擎，可将当前索引数据渲染为静态 HTML 页面，适合发布到 GitHub Pages、Nginx 或任何 Web 服务器，实现零后端资源导航。

- **RESTful 管理接口** 提供基于 JSON 的 HTTP API，支持资源的增删改查、批量导入导出和状态查询，方便与其他自动化工具或脚本集成。

- **数据导入导出** 支持 CSV 和 JSON 格式的批量导入导出，便于迁移数据或与外部数据源同步，也支持从标准书签 HTML 文件转换。

- **用户自定义视图** 允许保存常用筛选条件组合为命名视图，一键切换不同使用场景下的资源列表显示，提升高频访问效率。

- **操作审计日志** 记录所有资源变更操作（新增、删除、修改、检查），便于追溯数据变化历史和协作成员责任追踪。

## 应用场景

- **技术文档外部引用管理** 开源项目维护者可使用 DIT 统一管理文档中引用的所有外部视频教程、演讲录像或补充材料链接，定期自动检查链接有效性，避免文档中出现死链影响读者体验。

- **媒体研究数据收集** 高校或研究机构的工作人员可建立专题资源目录，例如收集特定地区、特定语言或特定时期公开的视频资料链接，通过标签和分类系统进行整理，支持研究论文的参考资料系统化归档。

- **个人知识库资源导航** 高级用户可将日常发现的优质外文视频资源进行分类存储，通过自定义视图快速访问不同领域内容，配合静态站点生成功能构建个人门户，在多设备间同步访问。

- **社区共享资源清单** 技术社区或论坛运营者可使用 DIT 构建公开的资源推荐列表，允许社区成员通过提交数据文件的方式贡献新链接，经审核后合并入主目录，形成社区共建的知识索引。

## 快速开始

以下步骤适用于 Linux / macOS / Windows WSL 环境，确保系统已安装 Python 3.9 及以上版本和 Git。

```bash
# 克隆项目仓库
git clone https://github.com/dit-org/dit-core.git
cd dit-core

# 创建并激活虚拟环境（推荐）
python3 -m venv venv
source venv/bin/activate      # Linux/macOS
# venv\Scripts\activate       # Windows

# 安装核心依赖
pip install -r requirements.txt

# 初始化本地数据库（SQLite）
python manage.py initdb

# 导入示例资源数据
python manage.py import --source samples/resources.json

# 启动开发服务器（默认监听 127.0.0.1:8080）
python manage.py runserver --port 8080

# 访问管理界面：http://127.0.0.1:8080/admin
# 默认管理员账号：admin / dit-admin-2024
```

生产环境部署建议使用 Gunicorn + Nginx 组合，参考 `deploy/` 目录下的示例配置文件。

## 安装要求

| 依赖项 | 必需版本 | 说明 |
|--------|----------|------|
| Python | 3.9 - 3.11 | 核心运行环境，3.12 暂未完全兼容 |
| SQLite | 3.31+ | 默认嵌入式数据库，无需额外安装 |
| Git | 2.25+ | 用于克隆仓库和版本管理 |
| requests | 2.28+ | 处理 HTTP 健康检查请求 |
| jinja2 | 3.1+ | 静态站点生成模板引擎 |
| click | 8.1+ | 命令行交互框架 |
| pytest | 7.0+ | 单元测试框架（仅开发环境需要） |
| black | 22.0+ | 代码格式化工具（仅贡献代码时需要） |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户手册 | `docs/user-guide/` | 如何添加资源、创建分类、设置标签、运行健康检查、生成静态站点 |
| 运维指南 | `docs/operations/` | 如何配置生产环境、调优性能、设置定时任务、备份恢复数据 |
| 开发文档 | `docs/developer/` | 项目架构设计、API 接口规范、插件扩展机制、如何提交 Pull Request |
| 设计决策 | `docs/design/` | 为什么选择 SQLite 而非 PostgreSQL、元数据模型设计考量、健康检查策略演进 |

## 资源列表

以下为项目元数据索引库中预置的参考资源链接，均来源于公开互联网。DIT 仅维护这些链接的结构化描述信息，不托管或代理任何实际内容。

中文视频综合类

<code>guochanzhubozaixianguankanw.org.cn</code>

<code>zhongwenshipinw.org.cn</code>

中文视频字幕类

<code>zaixianbofangzhongwenzimuw1.org.cn</code>

<code>zhongwenzimugaoqingw.org.cn</code>

中文热门视频类

<code>renqimianfeishipinw.org.cn</code>

<code>zhongwenzimuzaixiankanw.org.cn</code>

<code>zuixinzhongwenzimuw.org.cn</code>

## 项目结构

```
dit-core/
├── dit/                                # 核心 Python 包
│   ├── __init__.py                     # 包版本声明
│   ├── app.py                          # 主应用工厂与路由注册
│   ├── config.py                       # 配置管理（环境变量 + 默认值）
│   ├── models/                         # 数据模型层
│   │   ├── __init__.py
│   │   ├── resource.py                 # 资源实体（title, url, tags, status）
│   │   ├── category.py                 # 分类树节点
│   │   └── audit_log.py                # 操作审计记录
│   ├── services/                       # 业务逻辑层
│   │   ├── checker.py                  # 健康检查服务（并发探测）
│   │   ├── exporter.py                 # JSON/CSV 导出与静态站点生成
│   │   └── importer.py                 # 批量导入与书签转换
│   ├── api/                            # RESTful 接口层
│   │   ├── v1/
│   │   │   ├── resources.py            # 资源 CRUD 端点
│   │   │   └── health.py               # 健康检查触发与状态查询
│   ├── cli/                            # 命令行工具
│   │   ├── manage.py                   # 主入口（initdb, runserver, import）
│   │   └── commands/                   # 子命令拆分
│   └── templates/                      # 静态站点 HTML 模板
│       ├── index.html.j2               # 首页资源列表
│       └── detail.html.j2              # 单个资源详情页
├── tests/                              # 单元测试与集成测试
│   ├── test_models/
│   ├── test_services/
│   └── test_api/
├── samples/                            # 示例数据
│   ├── resources.json                  # 初始资源索引（含上述 URL）
│   └── categories.json                 # 初始分类结构
├── deploy/                             # 部署相关配置
│   ├── nginx.conf.example
│   ├── gunicorn.conf.py
│   └── systemd/dit.service
├── docs/                               # 文档源码（Markdown）
├── requirements.txt                    # 生产依赖列表
├── requirements-dev.txt                # 开发额外依赖
├── setup.py                            # 打包与安装脚本
├── README.md                           # 项目总览（本文件）
└── LICENSE                             # MIT 许可证文本
```

## 贡献指南

1. 阅读设计文档与代码风格规范（位于 `docs/developer/` 目录），确认对项目架构和编码约定有基本了解。所有新增代码必须通过 black 格式化检查。

2. 从 issues 列表中选择未被认领的标记为 `help-wanted` 或 `good-first-issue` 的问题，在 issue 下留言说明将开始处理，避免重复工作。建议先 fork 主仓库到个人账号。

3. 创建功能分支并提交代码，分支命名采用 `feature/简短描述` 或 `fix/问题编号` 格式。提交信息应遵守 Conventional Commits 规范（如 `feat: 添加批量标签更新接口`）。

4. 编写或更新对应的单元测试，确保测试覆盖率不低于原有水平。运行 `pytest tests/` 验证所有用例通过，并在本地手动测试相关功能。

5. 向主仓库的 `develop` 分支发起 Pull Request，在 PR 描述中关联对应的 issue 编号，并简要说明实现方案与测试结果。等待至少一名维护者审核后合并。

## 常见问题

**问：DIT 是否存储或缓存视频文件本身？**

答：DIT 完全不存储、代理或缓存任何视频、音频或字幕文件内容。项目仅维护资源定位符（URL）及其文本元数据（标题、描述、标签、状态）。所有链接的访问和内容获取均需用户自行通过标准网络工具完成，DIT 仅提供链接管理框架。任何资源链接的有效性仅取决于外部源服务器，DIT 的健康检查结果只反映探测时刻的连通性，不构成对资源可用性的长期承诺。

**问：健康检查模块如何避免对目标服务器造成压力？**

答：健康检查模块默认采用单线程顺序探测，每两个请求之间强制间隔至少 500 毫秒，并发数可由用户配置且默认不超过 2。超时时间默认设为 10 秒，仅发送 HEAD 请求以最小化带宽消耗。对于返回 403 或 429 状态码的服务器，系统会自动将该资源标记为谨慎状态并降低探测频率（从每日一次降低至每周一次），避免触发目标服务器的反爬机制。

**问：如何从旧版本迁移数据或与其他系统同步？**

答：DIT 提供 JSON 格式的完整导入导出功能，兼容绝大部分数据库系统导出的标准格式。若需从书签 HTML 文件迁移，可使用 `import --from-bookmarks` 命令进行转换。对于持续同步需求，建议使用 RESTful API 的批量更新端点，或定时执行导出脚本并配合外部同步工具（如 rsync、rclone）进行文件级同步。

## 许可证

MIT License

Copyright (c) 2026 DIT Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-08-25 22:00:20
