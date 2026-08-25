# Terminus Resource Gateway

Terminus Resource Gateway is a curated technical directory and external resource aggregation system designed for developers, researchers, and content curators who need to organize, validate, and present high-volume external link collections in a structured, maintainable manner. The project addresses the common challenge of managing large batches of external URLs—such as those from multilingual media sources, documentation mirrors, or community-contributed reference lists—by providing a lightweight static-site generation pipeline with built-in link health checking, categorical tagging, and version-controlled change tracking.

Target users include open-source documentation maintainers, educational platform developers, and technical writers who routinely handle link-rich content and require a reproducible workflow for URL cataloging, periodic validation, and presentation-layer rendering. Terminus Resource Gateway does not host any third-party content; it operates strictly as a metadata-driven gateway that transforms raw URL lists into browsable, searchable, and auditable resource indexes.

## 功能概览

- **Batch URL Ingestion** – Accepts plain-text URL lists, CSV exports, or markdown-formatted link sections; normalizes entries according to strict preservation rules (no automatic protocol addition, no www normalization, no trailing slash insertion).

- **Categorical Tagging Engine** – Applies rule-based or manual category labels to each URL; supports hierarchical tags (e.g., "Region/Language/MediaType") for multi-dimensional filtering.

- **Health Check Scheduler** – Periodically performs HEAD/GET request probes on all stored URLs; records HTTP status codes, response times, and TLS certificate expiry warnings; flags broken or redirected links.

- **Static Site Generator** – Renders the URL catalog into a pure HTML/CSS/JS static site with client-side search, tag filtering, and sortable columns; outputs zero-dependency files for deployment on any CDN or web server.

- **Change Audit Log** – Maintains a Git-commit-friendly changelog that records every addition, removal, or metadata update; enables full rollback and diff visibility across project versions.

- **Markdown-to-Data Pipeline** – Parses structured markdown sections (such as the resource list in this document) into internal JSON data models, allowing the README itself to serve as a single source of truth.

- **Export Adapters** – Supports exporting the catalog in JSON, YAML, CSV, and plain-text formats for integration with external tools, monitoring systems, or downstream documentation pipelines.

## 应用场景

- **Documentation Mirror Maintenance** – A technical documentation team maintains a list of regional mirror sites for package downloads. Terminus Resource Gateway tracks each mirror's availability and response latency, automatically flagging mirrors that become unreachable. The generated static page is embedded in the team's internal developer portal.

- **Academic Reference Aggregation** – A research lab collects hundreds of external dataset URLs, model checkpoints, and supplemental materials for a multi-year study. Using categorical tagging and audit logging, the lab ensures that every cited resource is persistently documented with timestamps and health status, simplifying reproducibility audits.

- **Community Content Curation** – An open-source community project accepts user-submitted links to tutorials, videos, and tools. The gateway validates incoming URLs, applies community-defined tags, and renders a searchable resource hub that community members can browse by topic or difficulty level.

- **Multilingual Media Indexing** – A content aggregation service needs to organize links to regional audio-visual resources. The gateway's strict URL preservation ensures that language-specific subdomains and country-code TLDs remain intact, while the health checker monitors geographic availability.

## 快速开始

```bash
# Clone the repository
git clone https://github.com/terminus-resource/gateway.git
cd gateway

# Install dependencies (Python 3.10+ required)
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt

# Run the ingestion pipeline on the sample URL list
python gateway ingest --input resources/sample_urls.txt --output data/catalog.json

# Generate the static site
python gateway build --catalog data/catalog.json --output dist/

# Start the development server to preview the site
python -m http.server 8000 --directory dist/
```

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.10.x 或更高 | 核心运行时；类型提示和模式匹配特性依赖 3.10+ |
| pip | 22.x 或更高 | 包管理工具；用于安装依赖项 |
| Git | 2.30.x 或更高 | 版本控制；用于克隆仓库和提交变更日志 |
| SQLite | 3.35.x 或更高 | 轻量级本地数据库；存储 URL 元数据、健康检查历史 |
| requests | 2.28.x | HTTP 客户端库；用于健康检查和 URL 探测 |
| beautifulsoup4 | 4.11.x | HTML 解析库；用于提取页面标题和元描述（可选增强） |
| markdown | 3.4.x | Markdown 解析器；用于将 README 等文档转换为数据模型 |
| pytest | 7.x | 单元测试框架；仅在开发环境中需要 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 用户指南 | docs/user-guide/ingestion.md | 如何准备 URL 输入文件？支持哪些格式？如何配置类别映射？ |
| 运维手册 | docs/operations/health-checks.md | 如何调整健康检查频率？如何处理超时和重试？如何解读状态报告？ |
| 开发参考 | docs/development/architecture.md | 数据流如何从 ingestion 到 export？扩展自定义标签规则的接口在哪？ |
| 部署指南 | docs/deployment/static-hosting.md | 生成静态站点后如何部署到 Nginx、S3 或 Cloudflare Pages？ |
| 故障排查 | docs/troubleshooting/common-issues.md | URL 规范化失败怎么办？健康检查被目标服务器屏蔽如何解决？ |

## 资源列表

本批次收录的资源按语言媒体类别组织，共 7 个外部链接，每个条目严格按原始字符串保留，不做任何协议补全、域名改写或路径修改。

媒体资源类（音视频与字幕内容）

<code>zhongwenzaixiangaojinghaokanw.org.cn</code>

<code>rihanzhongwenzimuw.org.cn</code>

<code>zhongwenzimubofangw.org.cn</code>

<code>mianfeikanjuwangzhanw.org.cn</code>

<code>renqizaixianguankanw.org.cn</code>

<code>zhongwenzimushipinw.org.cn</code>

<code>zaixianzumumianfeigaoqingw.org.cn</code>

## 项目结构

```
gateway/
├── gateway/                           # 核心 Python 包
│   ├── __init__.py                    # 版本号与公开 API 导出
│   ├── cli/                           # 命令行接口模块
│   │   ├── __init__.py
│   │   ├── ingest.py                  # 导入逻辑：支持 txt/csv/md 解析
│   │   ├── build.py                   # 静态站点生成器入口
│   │   └── health.py                  # 健康检查调度与报告输出
│   ├── core/                          # 数据模型与业务逻辑
│   │   ├── __init__.py
│   │   ├── models.py                  # URLRecord, Tag, HealthReport 等 Pydantic 模型
│   │   ├── catalog.py                 # 目录增删改查、版本管理
│   │   └── validator.py               # URL 规范化与合法性校验
│   ├── parsers/                       # 解析器适配器
│   │   ├── __init__.py
│   │   ├── markdown_parser.py         # 提取 README 中 <code> 包裹的 URL 列表
│   │   └── plaintext_parser.py        # 按行读取裸 URL
│   ├── exporters/                     # 输出适配器
│   │   ├── __init__.py
│   │   ├── json_exporter.py           # 导出为 JSON 结构化数据
│   │   └── static_site/               # 静态站点模板与资源
│   │       ├── templates/             # Jinja2 HTML 模板
│   │       └── assets/                # 内置 CSS 与 JavaScript
│   └── utils/                         # 通用工具函数
│       ├── __init__.py
│       ├── network.py                 # 带重试的 HTTP 请求封装
│       └── logging.py                 # 结构化日志配置
├── data/                              # 运行时数据目录
│   ├── catalog.json                   # 当前完整目录快照
│   └── changelog.jsonl                # 每条变更记录（追加写入）
├── tests/                             # 单元测试与集成测试
│   ├── test_models.py
│   ├── test_parsers.py
│   └── fixtures/                      # 测试用的示例 URL 列表
├── docs/                              # 项目文档（用户指南、运维手册等）
├── requirements.txt                   # 生产环境依赖列表
├── dev-requirements.txt               # 开发环境额外依赖（pytest, black, mypy）
├── LICENSE                            # MIT 许可证全文
└── README.md                          # 本文件
```

## 贡献指南

1.  **Fork 仓库并创建功能分支** – 从主分支 checkout 一个新的 feature 分支，命名风格为 `feature/short-description` 或 `fix/issue-id`。确保分支名称简洁描述变更目的。

2.  **更新资源列表或代码逻辑** – 若需添加、删除或修改 URL 条目，请编辑 `data/catalog.json` 或相应源文件。对于新增的外部链接，务必遵循严格的 URL 保留规则（不补协议、不改域名、不加尾部斜线）。所有代码变更应附带对应的单元测试（位于 `tests/` 目录）。

3.  **运行本地校验流水线** – 在提交前执行 `pytest tests/` 确保所有测试通过；运行 `python gateway health --check-all` 验证所有收录 URL 的当前可达性；运行 `python gateway build` 确保静态站点生成无错误。

4.  **提交变更并签署开发者原产地证书** – 提交消息应采用约定式提交格式（如 `feat: add batch 4/63 resources` 或 `fix: correct tag assignment for media URLs`）。在提交消息末尾添加 `Signed-off-by: Your Name <email>` 行，以证明您有权贡献该变更。

5.  **发起拉取请求（Pull Request）** – 向主仓库的 `main` 分支提交 PR。在 PR 描述中明确列出变更内容、关联的批次编号（若涉及资源更新），以及任何可能影响现有用户的行为变化。等待至少一位维护者审阅批准后合并。

## 常见问题

**Q: 为什么 README 中的 URL 必须用 <code> 标签包裹，且禁止任何改写？**

A: Terminus Resource Gateway 的 markdown 解析器依赖 `<code>` 标记来精准提取外部资源列表，避免误解析普通文本中的域名片段。严格禁止改写 URL 是为了确保解析结果与原始输入字节完全一致——任何协议补全、域名规范化或尾部斜线添加都会破坏下游验证器的哈希校验和变更检测逻辑。这是项目数据完整性策略的核心约束。

**Q: 健康检查模块如何处理被防火墙或反爬机制拒绝的站点？**

A: 健康检查模块默认使用 `requests` 会话，配置了常见的浏览器用户代理轮换池和合理的请求间隔（最小间隔 2 秒）。对于返回 403/429 状态码的站点，健康检查会将该 URL 标记为 "受限" 而非 "失效"，并在报告中附带响应头中的 `Retry-After` 信息。用户可通过配置 `--probe-timeout` 和 `--retry-count` 调整探测行为，也可在 `config.yaml` 中为特定域名自定义请求头。

**Q: 静态站点生成后，搜索和过滤功能是完全在客户端运行的吗？**

A: 是的，生成的静态站点包含一个预构建的 JSON 索引文件（`index.json`），其中存储了所有 URL 的元数据、标签和描述。客户端 JavaScript 在浏览器中加载该索引并执行实时搜索和过滤，无需任何后端服务。这意味着生成的 `dist/` 目录可以部署在任何纯静态托管环境（如 GitHub Pages、S3 静态网站、Cloudflare R2）中，且完全不依赖服务器端运行时。

## 许可证

MIT License

Copyright (c) 2026 Terminus Resource Gateway Contributors

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

> 外链数量: 7 | 生成时间: 2026-08-25 21:59:37
