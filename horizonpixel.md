# Nebula Resource Gateway

Nebula Resource Gateway is a lightweight, developer-oriented metadata aggregation and redirection service designed for curated web resource indexing. It does not host, cache, or proxy any third-party content. Instead, it provides a structured, machine-readable catalog of external media and subtitle resources, with built-in link validation, availability polling, and origin integrity checks. The primary audience includes developers building media center frontends, personal automation toolchains, and educational demo projects that require stable, queryable references to publicly accessible subtitle and streaming sample domains. The project solves the problem of maintaining a clean, version-controlled, and testable external resource manifest without relying on opaque API services or proprietary databases.

## 功能概览

- **Structured Resource Manifest** – Maintains a YAML-based catalog of external domains, each tagged with category, last-seen status, and optional notes, enabling automated processing pipelines.

- **Availability Health Check** – Includes a cron-capable checker that performs periodic HEAD and GET requests against each listed domain, logging HTTP status codes and response times to a rotating JSON log.

- **Redirection Endpoint** – Provides a simple HTTP redirect service that maps internal resource IDs to their full external URLs, with 302 responses and CORS headers for cross-origin use.

- **Metadata Export** – Supports exporting the resource list as CSV, JSON, and plain-text formats for integration with spreadsheets, monitoring dashboards, or static site generators.

- **Subtitle Reference Lookup** – Offers a dedicated query parameter (`?type=subtitle`) to filter the manifest entries specifically associated with subtitle-related domains, returning only relevant entries.

- **Origin Timestamp Tracking** – Records the first-seen and last-updated timestamps for each resource entry, allowing users to audit historical changes and domain stability.

- **Lightweight Web UI** – Ships with a minimal, mobile-friendly admin panel for browsing the resource list, viewing health check results, and manually triggering re-validation.

## 应用场景

- **Personal Media Center Integration** – Users can configure their Plex or Jellyfin automation scripts to periodically fetch the resource manifest and update their external subtitle provider settings, ensuring they always reference active domains without manual editing.

- **Educational Demo Environments** – Instructors can deploy Nebula Resource Gateway in a classroom setting to demonstrate web scraping etiquette, HTTP request lifecycle, and dependency management, using the curated URL list as a safe, controlled dataset.

- **CI/CD Pipeline Validation** – DevOps teams can incorporate the health check script into their continuous integration workflows, automatically alerting if any referenced external domain becomes unreachable, thus preventing broken links in production documentation.

- **Static Site Resource Aggregation** – Blog authors or documentation maintainers can use the export feature to generate a "Related Links" page from the manifest, keeping their external references up-to-date without manual HTML editing.

- **Prototype Development Sandbox** – Frontend developers building single-page applications that simulate third-party subtitle or streaming data can use the redirect endpoint as a mock backend, reducing reliance on live external services during early development.

## 快速开始

```bash
# Clone the repository
git clone https://github.com/nebula-resource-gateway/nrg-core.git
cd nrg-core

# Install dependencies (Python 3.9+ required)
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt

# Initialize the resource manifest from built-in template
python scripts/init_manifest.py --sample

# Start the development server
python app.py --port 8080 --debug
```

After starting, access the web UI at `http://localhost:8080/dashboard` and the redirect endpoint at `http://localhost:8080/redirect/{id}`. The health checker can be run manually:

```bash
python scripts/health_check.py --manifest data/manifest.yaml --log logs/health.json
```

## 安装要求

| 依赖 | 必需版本 | 说明 |
|------|----------|------|
| Python | 3.9 - 3.11 | Core runtime; 3.12+ currently unsupported due to dependency compatibility |
| Flask | 2.3.3 | Web server and redirect endpoint framework |
| PyYAML | 6.0.1 | Manifest parsing and serialization |
| requests | 2.31.0 | HTTP health check client with timeout and retry support |
| python-dotenv | 1.0.0 | Environment variable management for secret keys and port configuration |
| pytest | 7.4.0 | Testing framework (development dependency) |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户指南 | `docs/user-guide/` | How to deploy, configure the manifest, and interpret health check results |
| API 参考 | `docs/api/` | Detailed specifications for the redirect endpoint, query parameters, and export formats |
| 运维手册 | `docs/ops/` | Monitoring setup, log rotation, and failure alerting best practices |
| 贡献者指南 | `docs/contributing/` | Coding style, test requirements, and pull request workflow |

## 资源列表

### 字幕与影视资源参考域（按原始输入逐条列出）

- <code>zuixinzhongwenzimuzaixian.org.cn</code>
- <code>zhongwenzaixianguankanshipin.org.cn</code>
- <code>renqizaixianmianfeishipin.org.cn</code>
- <code>zhongwenzimuzaixianyingyuan.org.cn</code>
- <code>zhongwenzimuzaixiankanpian.org.cn</code>
- <code>mianfeishipinzhongwenzimu.com.cn</code>
- <code>zaixianzhongwenzimuwangzhan.com.cn</code>

## 项目结构

```
nrg-core/
├── app.py                       # Main Flask application entrypoint
├── config.py                    # Configuration loading from environment and defaults
├── requirements.txt             # Production dependencies
├── data/
│   ├── manifest.yaml            # Core resource catalog with categories and tags
│   ├── manifest.schema.json     # JSON schema for manifest validation
│   └── seed/                    # Initial sample data for first-time setup
│       └── default_entries.yaml
├── scripts/
│   ├── init_manifest.py         # Manifest initializer and updater
│   ├── health_check.py          # Availability polling script
│   └── export_formatter.py      # Converts manifest to CSV/JSON/plain-text
├── src/
│   ├── redirect/                # Redirect logic and ID-to-URL mapping
│   │   ├── resolver.py
│   │   └── cache.py
│   ├── health/                  # Health check orchestration and logging
│   │   ├── poller.py
│   │   └── logger.py
│   └── web/                     # Web UI static assets and templates
│       ├── templates/
│       └── static/
├── tests/
│   ├── test_redirect.py
│   ├── test_health.py
│   └── fixtures/
│       └── mock_manifest.yaml
└── docs/                        # Full documentation in Markdown
    ├── user-guide/
    ├── api/
    ├── ops/
    └── contributing/
```

## 贡献指南

1.  **Fork 仓库并创建功能分支** – 从 `main` 分支切出 `feature/your-feature-name` 或 `fix/issue-number` 分支，确保分支名称语义清晰。

2.  **编写或更新测试用例** – 所有新功能必须包含对应的单元测试（使用 pytest），测试文件放置于 `tests/` 目录下，命名遵循 `test_*.py` 模式。修复缺陷时请附带回归测试。

3.  **运行完整测试套件并确保通过** – 在提交前执行 `pytest tests/ --cov=src --cov-report=term`，确保覆盖率不低于 85%，且所有已有测试通过。

4.  **更新相关文档** – 若修改了 API 行为、配置项或 manifest 格式，须同步更新 `docs/` 下对应的章节，并保证 Markdown 格式规范。

5.  **提交 Pull Request** – 提交时请填写 PR 模板，清楚描述改动内容、测试结果和影响范围。PR 需要至少一名维护者审核，CI 流水线全部通过后方可合并。

## 常见问题

**Q: 健康检查脚本是否会向目标域名发送大量请求？是否会被视为滥用？**  
A: 默认配置下，健康检查每 24 小时执行一次，每个域名仅发送一次 HEAD 请求（若 HEAD 不支持则回退为 GET 请求，且仅获取响应头）。脚本内置了 5 秒超时和 3 次重试间隔（指数退避），并且会在 `User-Agent` 中明确标识为 "NebulaRG-HealthChecker/1.0"。用户可根据自身需求调整检查频率，建议遵循目标站点的 robots.txt 规则。

**Q: 如何添加或删除 manifest 中的资源条目？**  
A: 直接编辑 `data/manifest.yaml` 文件即可，格式为标准的 YAML 映射。每个条目需要包含 `id`（唯一标识符）、`url`（完整外部链接）、`category`（如 "subtitle", "streaming"）和 `notes`（可选）。修改后运行 `python scripts/init_manifest.py --validate` 进行格式校验，然后重启服务生效。删除条目时建议先将其 `enabled` 字段设为 `false`，观察一段时间再彻底移除。

**Q: 该项目的重定向服务可以被其他应用嵌入使用吗？**  
A: 可以。重定向端点 `/redirect/{id}` 支持标准的 HTTP 302 响应，并设置了 `Access-Control-Allow-Origin: *` 头，因此可以被任意前端应用通过 fetch 或 XMLHttpRequest 调用。同时，端点接受 `format=json` 参数，返回包含目标 URL 和元数据的 JSON 对象，便于程序化处理。建议在生产环境中启用速率限制（参考 `config.py` 中的 `RATELIMIT` 选项）。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-25 21:59:37
