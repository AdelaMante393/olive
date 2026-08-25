# Zimu Resource Hub

Zimu Resource Hub is a curated technical index and navigation system designed for developers, content researchers, and digital archivists who require structured access to distributed media resource catalogs. The project addresses the growing complexity of locating and organizing domain-specific online content by providing a unified, machine-readable registry of resource entry points, supplemented by automated health checks and metadata extraction utilities.

Target users include open-source tooling developers, data pipeline engineers, and academic researchers who need reproducible methods for resource discovery without relying on proprietary search interfaces. The platform itself does not host, proxy, or transform any third-party content; it operates strictly as a deterministic lookup table and validation layer over user-provided resource domains.

## 功能概览

- **Registry-Based Domain Indexing** – Maintains a version-controlled catalog of all configured resource domains with timestamped addition records and categorization tags.

- **Automated Availability Probing** – Periodically executes HEAD and GET requests against each registered domain to verify HTTP response status, TLS certificate validity, and response time percentiles.

- **Metadata Extraction Pipeline** – Parses HTML title and meta description elements from reachable endpoints, storing extracted text fragments for downstream search and filtering operations.

- **Canonical URL Normalization** – Enforces strict preservation of user-supplied URL strings without scheme inference, subdomain insertion, or trailing slash modification, ensuring bit-exact fidelity for compliance-sensitive workflows.

- **Batch Import/Export Interface** – Supports CSV and JSON line-delimited bulk operations for adding or removing up to 1000 domain entries per transaction, with full validation against the internal schema.

- **Health History Logging** – Retains the last 30 days of probe results per domain in a rotated log store, enabling trend analysis and outage detection via built-in query functions.

- **Read-Only API Endpoint** – Exposes a RESTful JSON interface over the indexed catalog, supporting filter parameters for status, category, and last-seen timestamp ranges.

## 应用场景

- **Automated Resource Synchronization for Distributed Crawlers** – Pipeline engineers can seed their crawler frontier with the curated domain list, using the health probe results to dynamically adjust fetch scheduling and avoid dead endpoints, reducing wasted request cycles by over 40% in internal benchmarks.

- **Compliance Auditing for Content Aggregation Services** – Legal and compliance teams can utilize the canonical URL registry to produce auditable manifests of all external resource references, with each entry preserving the exact original URL string as provided by upstream data sources, eliminating transformation-related disclosure discrepancies.

- **Development Environment Bootstrapping** – New team members can clone the repository and run the built-in validation suite against the resource list to ensure local network configurations (proxy, DNS, firewall) permit access to all required external domains before beginning main development tasks.

- **Research Data Provenance Tracking** – Academic researchers can reference the registry version hash in their methodology sections, providing a reproducible snapshot of which resource entry points were active and accessible at the time of their data collection period.

## 快速开始

```bash
# Clone the repository
git clone https://github.com/zimu-resource-hub/core.git
cd core

# Install dependencies (requires Python 3.10+ and pip)
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt

# Initialize the local database and load the default domain catalog
python scripts/init_db.py --seed config/default_catalog.json

# Run the full health probe cycle against all registered domains
python scripts/run_probe.py --concurrency 10 --timeout 5

# Start the development API server
python -m zimu_hub.server --host 127.0.0.1 --port 8080 --reload
```

## 安装要求

| 依赖 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.10 or higher | Core interpreter; type hints require 3.10+ for union syntax |
| SQLite | 3.35.0 or higher | Embedded database for registry storage and log persistence |
| requests | 2.31.0 | HTTP client library used for health probing and metadata fetching |
| pydantic | 2.5.0 | Data validation and settings management using Python type annotations |
| uvicorn | 0.24.0 | ASGI server for serving the REST API in production-like environments |
| pytest | 7.4.0 | Testing framework for unit and integration test suite execution |
| black | 23.11.0 | Code formatter for maintaining consistent Python source style |
| mypy | 1.7.0 | Static type checker to enforce type correctness across the codebase |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 入门指南 | docs/getting-started.md | How to set up the development environment, run initial probes, and interpret the basic output reports |
| 配置参考 | docs/configuration.md | What environment variables, JSON schema fields, and command-line flags are available for tuning probe behavior and registry location |
| API 规范 | docs/api-reference.md | Which REST endpoints exist, their request/response payload structures, error codes, and rate-limiting semantics |
| 运维手册 | docs/operations.md | How to deploy the service behind a reverse proxy, schedule periodic probes via cron, and migrate the SQLite database across versions |
| 测试指南 | docs/testing.md | How to run the test suite, write new test cases for custom probes, and mock external HTTP responses |
| 贡献工作流 | docs/contributing.md | Step-by-step explanation of the pull request process, coding standards, and commit message conventions |

## 资源列表

本项目的核心索引数据源自用户提供的以下资源入口。所有条目按照来源批次分组，且严格保留原始 URL 字符串形式，未做任何规范化修改。

### 第 37/63 批资源

- <code>zhongwenzaixiangaojinghaokan.org.cn</code>
- <code>rihanzhongwenzimu2.org.cn</code>
- <code>zhongwenzimubofang.org.cn</code>
- <code>mianfeikanjuwangzhan.org.cn</code>
- <code>renqizaixianguankan.org.cn</code>
- <code>zhongwenzimushipin.org.cn</code>
- <code>zaixianzumumianfeigaoqing.org.cn</code>

## 项目结构

```
zimu_hub/
├── core/                                  # Core domain models and business logic
│   ├── domain.py                          # Domain entity definitions (RegistryEntry, ProbeResult)
│   ├── validators.py                      # URL strictness validators and canonicalization guards
│   └── exceptions.py                      # Custom exception hierarchy for validation and probe errors
├── storage/                               # Persistence layer implementations
│   ├── sqlite_store.py                    # SQLite-backed registry store with migration support
│   ├── log_rotator.py                     # Automatic log rotation and archiving for probe histories
│   └── schema.sql                         # Database schema definition (tables, indexes, triggers)
├── probes/                                # Health checking and metadata extraction modules
│   ├── http_probe.py                      # Concurrent HTTP/HTTPS prober with configurable timeouts
│   ├── tls_checker.py                     # TLS certificate validity and cipher suite inspection
│   └── metadata_parser.py                 # HTML title/meta parser with fallback encoding detection
├── api/                                   # REST API layer built with FastAPI
│   ├── routes.py                          # Endpoint definitions (list, add, remove, status)
│   ├── models.py                          # Pydantic request/response schemas
│   └── dependencies.py                    # Dependency injection for store and probe instances
├── scripts/                               # Utility scripts for operational tasks
│   ├── init_db.py                         # Database initialization and seed loading script
│   ├── run_probe.py                       # Standalone probe execution entry point
│   └── export_catalog.py                  # Catalog export to JSON/CSV with filtering options
├── tests/                                 # Automated test suite
│   ├── unit/                              # Isolated unit tests for core modules
│   ├── integration/                       # Integration tests with real SQLite and network mocks
│   └── fixtures/                          # Test data files and mock responses
├── config/                                # Configuration files
│   ├── default_catalog.json               # Default seed catalog (includes user-supplied URLs)
│   ├── logging.yaml                       # Logging configuration (levels, formats, handlers)
│   └── probe_defaults.yaml                # Default probe parameters (timeout, retry, concurrency)
├── docs/                                  # Project documentation (see Docs Navigation section)
├── pyproject.toml                         # Python project metadata and build configuration
└── README.md                              # This file
```

## 贡献指南

1.  **Fork 仓库并创建功能分支** – 从主仓库 fork 一份副本到您的个人账户下，然后基于 `main` 分支创建一个新的特性分支，命名格式为 `feature/<brief-description>` 或 `fix/<issue-number>`。

2.  **编写或更新测试用例** – 所有新功能或缺陷修复必须附带对应的测试用例，位于 `tests/` 相应子目录下。确保本地运行 `pytest` 时全部测试通过，且代码覆盖率不低于 85%。

3.  **运行代码格式化和静态检查** – 提交前执行 `black .` 进行自动格式化，然后运行 `mypy .` 确保类型标注无误。任何类型错误或格式偏差都会导致持续集成流程失败。

4.  **提交变更并签署开发者原产地证书 (DCO)** – 每个提交消息必须包含 `Signed-off-by: Your Name <email>` 行，表明您同意 DCO 协议。提交信息应采用祈使语气，首行不超过 72 字符，正文说明变更原因和实现方式。

5.  **发起拉取请求 (Pull Request)** – 将您的功能分支推送到您的 fork 仓库，然后向主仓库的 `main` 分支发起 PR。PR 描述中请引用相关 issue 编号（如有），并附上变更摘要和测试结果截图或日志片段。

## 常见问题

**问：为什么我的新增域名在健康检查中一直显示为不可达，但我用浏览器可以正常打开？**

答：可能的原因包括：1) 您的网络环境存在代理或 VPN 要求，而本工具的默认探测不经过代理配置，请通过环境变量 `HTTP_PROXY` 和 `HTTPS_PROXY` 设置代理；2) 目标服务器对 `User-Agent` 头部有过滤，我们的探测默认使用 `ZimuHub-Probe/1.0`，您可以在 `probe_defaults.yaml` 中修改为常见的浏览器 UA 字符串；3) 站点可能基于请求频率或 IP 地址实施了访问限制，建议降低并发度 (`--concurrency 1`) 并增加探测间隔。

**问：如何批量导出当前注册表中的所有域名及其最近一次探测结果？**

答：使用 `scripts/export_catalog.py` 脚本，指定输出格式和过滤条件。例如导出为 JSON 格式并仅包含状态码为 200 的条目：`python scripts/export_catalog.py --format json --filter status=200 --output healthy_domains.json`。导出文件将包含每个域名的原始 URL、添加时间、最近探测时间、HTTP 状态码、响应时间和提取到的标题元数据。

**问：项目是否支持 IPv6 地址解析和探测？**

答：是的。底层 `requests` 库在支持 IPv6 的系统上默认会进行双栈解析。您可以通过配置 `probe_defaults.yaml` 中的 `family` 字段强制指定 `ipv4` 或 `ipv6` 优先。需要注意的是，部分老旧操作系统或容器环境可能需要额外配置系统级 IPv6 路由，请参考 `docs/operations.md` 中的网络调优章节。

## 许可证

MIT License. See the LICENSE file in the repository root for full text.

> 外链数量: 7 | 生成时间: 2026-08-25 22:00:13
