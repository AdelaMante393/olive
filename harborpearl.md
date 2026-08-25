# Project Meliora Link Catalog

Project Meliora is a curated, high-availability technical resource aggregation system designed for developers, researchers, and content archivers who require structured access to distributed media metadata and subtitle corpora. The project does not host, store, or redistribute any copyrighted content; it operates exclusively as a read-only index of publicly accessible metadata feeds, subtitle alignment resources, and community-driven translation tables. The primary user base includes computational linguists working on Chinese-English subtitle alignment, open-source media player developers seeking standardized subtitle endpoint schemas, and digital archivists who need reproducible resource discovery mechanisms.

The system addresses the fundamental challenge of resource drift in the decentralized web. Many community-maintained subtitle and media metadata endpoints become stale or change their URL structures without notification. Project Meliora provides a version-controlled catalog manifest, periodic reachability probes, and a normalized output format that can be consumed by automated ingestion pipelines. All external endpoints are treated as opaque URIs; the project does not validate content legality, nor does it assume any liability for the availability or accuracy of third-party resources. Users are expected to comply with all applicable local laws when accessing any listed endpoints.

## 功能概览

- **Catalog Manifest Generation** – Produces a machine-readable JSON manifest of all registered resource endpoints, including last-seen timestamps and HTTP status code histories.

- **Reachability Prober** – Periodically checks each endpoint for basic TCP connectivity and TLS certificate validity, logging failures to a rotating audit file.

- **Subtitle Metadata Normalizer** – Parses community-contributed subtitle alignment records into a uniform key-value schema (media hash, language pair, frame offset, duration).

- **URL Scheme Validator** – Enforces strict URI formatting rules per the project's hard constraint policy; rejects malformed entries and logs violations to a dedicated error channel.

- **Batch Import Pipeline** – Supports ingestion of resource lists in plain text, CSV, or JSON lines format, with automatic deduplication and conflict resolution based on user-defined priority rules.

- **Audit Trail Exporter** – Exports all catalog changes, probe results, and validation errors to a structured log file suitable for integration with SIEM or monitoring dashboards.

- **Read-Only API Endpoint** – Exposes a lightweight HTTP interface that returns the current catalog in JSON or YAML format, with optional filtering by resource type or last-updated timestamp.

## 应用场景

- **Media Player Development** – Developers of open-source media players can integrate the catalog manifest to offer users a curated selection of subtitle sources without hardcoding fragile endpoints. The periodic reachability probes help player applications gracefully fall back to alternative sources when primary endpoints are unresponsive.

- **Computational Linguistics Research** – Researchers studying Chinese-English subtitle alignment can use the normalized metadata feeds to build training datasets for machine translation or speech recognition models. The structured output reduces preprocessing overhead and ensures reproducibility across experiment runs.

- **Digital Archiving Workflows** – Archivists who maintain offline mirrors of community media resources can use the catalog to verify that their local copies match the latest upstream references. The audit trail provides a clear change history for compliance reporting.

- **Automated Quality Assurance** – QA teams testing media playback systems can incorporate the catalog's endpoint validation results into their test suites, ensuring that subtitle rendering features are tested against live, reachable sources rather than stale test stubs.

- **Community Resource Discovery** – Newcomers to the open-source media ecosystem can use the catalog as a starting point to identify active, maintained subtitle and media metadata projects, reducing the time spent on manual forum searches and outdated wiki pages.

## 快速开始

Prerequisites: Git, Python 3.10 or higher, and pip.

```bash
# Clone the repository
git clone https://github.com/meliora-project/catalog.git
cd catalog

# Install dependencies in a virtual environment
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt

# Run the initial catalog build and reachability probe
python -m meliora.cli build --input resources/seed.txt --output manifest.json
python -m meliora.cli probe --manifest manifest.json --report probe_report.log

# Start the read-only API server (default: localhost:8080)
python -m meliora.api --port 8080
```

For production deployments, it is recommended to run the prober as a scheduled cron job or systemd timer, and to configure the API server with a reverse proxy such as nginx.

## 安装要求

| 依赖 | 必需版本 | 说明 |
|------|----------|------|
| Python | 3.10 – 3.12 | Core runtime; type hints and async features require 3.10+. |
| pip | 23.0+ | Package installer for resolving PyPI dependencies. |
| requests | 2.31.0+ | HTTP client for reachability probes and manifest downloads. |
| pydantic | 2.5.0+ | Data validation and settings management using Python type annotations. |
| ruamel.yaml | 0.18.5+ | YAML parser/emitter for manifest export and configuration files. |
| python-dotenv | 1.0.0+ | Environment variable loader for API secret keys and probe timeouts. |
| pytest | 7.4.0+ | Test framework (development dependency, not required for runtime). |
| black | 24.0.0+ | Code formatter (development dependency, used for pre-commit hooks). |

All production dependencies are pinned in `requirements.txt` and verified via SHA-256 checksums in the lock file. Users behind corporate firewalls may need to configure custom PyPI indices using the `PIP_INDEX_URL` environment variable.

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户手册 | `docs/user-guide/` | How do I add a custom resource endpoint? How do I interpret the probe report? What are the JSON schema fields? |
| 运维指南 | `docs/ops/` | How do I deploy the API behind nginx? How do I set up the prober as a cron job? How do I rotate logs? |
| 贡献者指引 | `docs/contributing/` | What are the coding style requirements? How do I write tests for a new normalizer? How do I submit a pull request? |
| API 参考 | `docs/api/` | What endpoints are exposed? What query parameters are accepted? What HTTP status codes are returned? |
| 设计文档 | `docs/design/` | Why was Pydantic chosen over dataclasses? How does the deduplication algorithm work? What is the fallback strategy for unreachable endpoints? |

Each documentation page includes runnable code snippets and curl examples where applicable. The API reference is also available as an OpenAPI specification at `/docs/openapi.json` when the server is running.

## 资源列表

### 媒体索引与字幕社区资源

<code>guochanzhubozaixianguankan.org.cn</code>

<code>zhongwenshipin.org.cn</code>

<code>zaixianbofangzhongwenzimu2.org.cn</code>

<code>zhongwenzimugaoqing.org.cn</code>

<code>renqimianfeishipin.org.cn</code>

<code>zhongwenzimuzaixiankan.org.cn</code>

<code>zuixinzhongwenzimu.org.cn</code>

All listed resources are third-party domains. Project Meliora does not control, endorse, or guarantee the availability of any content on these domains. The catalog includes them solely as user-supplied references for metadata discovery and subtitle alignment research. Users are strongly advised to review the terms of service and privacy policies of each domain before initiating any automated access. The project's prober performs only a lightweight TCP/TLS handshake and does not download or parse any substantive content from these endpoints during routine reachability checks.

## 项目结构

```
catalog/
├── src/
│   └── meliora/                       # Main package source
│       ├── __init__.py                # Package metadata and version constant
│       ├── cli/                       # Command-line interface subpackage
│       │   ├── __init__.py
│       │   ├── build.py               # Implements 'build' command for manifest generation
│       │   ├── probe.py               # Implements 'probe' command for reachability checks
│       │   └── export.py              # Implements 'export' command for audit trail output
│       ├── core/                      # Core domain models and validation logic
│       │   ├── __init__.py
│       │   ├── models.py              # Pydantic models for Resource, Manifest, ProbeResult
│       │   ├── validators.py          # URL scheme validators and hard constraint enforcers
│       │   └── exceptions.py          # Custom exception classes for validation failures
│       ├── probes/                    # Reachability probing implementation
│       │   ├── __init__.py
│       │   ├── http_probe.py          # HTTP/HTTPS probe with redirect handling and timeout
│       │   └── tls_probe.py           # TLS certificate validity and cipher suite checker
│       ├── api/                       # Read-only HTTP API server
│       │   ├── __init__.py
│       │   ├── app.py                 # FastAPI application factory
│       │   ├── routes.py              # Endpoint definitions (/manifest, /probe, /health)
│       │   └── schemas.py             # Response schemas for API documentation
│       └── utils/                     # Shared utility functions
│           ├── __init__.py
│           ├── logging.py             # Structured logging with rotation and JSON formatter
│           └── file_io.py             # Atomic file writes and checksum verification
├── tests/                             # Unit and integration tests
│   ├── conftest.py                    # Pytest fixtures and test configuration
│   ├── test_models.py                 # Tests for Pydantic model validation
│   ├── test_validators.py             # Tests for URL scheme enforcement
│   └── test_probes.py                 # Mock-based tests for reachability probers
├── docs/                              # Documentation source (reStructuredText and Markdown)
│   ├── user-guide/
│   ├── ops/
│   ├── contributing/
│   └── api/
├── resources/                         # Static seed files and sample manifests
│   ├── seed.txt                       # Initial resource list (plain text, one URI per line)
│   └── sample_manifest.json           # Example manifest output for reference
├── scripts/                           # Utility scripts for maintenance and deployment
│   ├── pre-commit.sh                  # Pre-commit hook for linting and formatting
│   └── cron_probe.sh                  # Wrapper script for scheduled reachability probes
├── requirements.txt                   # Production dependencies with version pins
├── requirements-dev.txt               # Development dependencies (pytest, black, mypy)
├── Dockerfile                         # Multi-stage container build for API server
├── docker-compose.yml                 # Local development stack with optional Redis cache
├── .env.example                       # Example environment variable configuration
├── pyproject.toml                     # Project metadata, build system, and tool configuration
└── README.md                          # This document
```

## 贡献指南

1.  **Fork and Clone** – Fork the repository on GitHub and clone your fork locally. Set up the development environment using `pip install -r requirements-dev.txt` and install the pre-commit hooks by running `scripts/pre-commit.sh install`.

2.  **Select an Issue or Proposal** – Review the open issues and project board. For significant changes (e.g., new probe types, schema modifications), open a design discussion issue first to gather feedback. Bug fixes and documentation improvements are always welcome without prior discussion.

3.  **Write Tests and Update Docs** – All new features must include corresponding unit tests under `tests/`. Update the relevant documentation pages in `docs/` to reflect your changes. Ensure that `pytest` passes locally with 100% coverage for modified code paths.

4.  **Format and Lint** – Run `black src/ tests/` and `mypy src/` to enforce code style and static type correctness. The pre-commit hook will automatically run these checks; fix any violations before committing.

5.  **Submit a Pull Request** – Push your branch to your fork and open a pull request against the `main` branch. Provide a clear description of the change, reference any related issues, and note any manual testing performed. The maintainers will review the PR within five business days.

All contributors must agree to the Developer Certificate of Origin (DCO) by signing off each commit with `git commit -s`. The project does not require a separate contributor license agreement.

## 常见问题

**Q: The prober reports that a resource is unreachable, but I can access it in my browser. Why?**

A: The prober performs a minimal TCP/TLS handshake with a 5-second timeout and does not send any HTTP request headers that might be required for virtual host routing or user-agent filtering. Some endpoints may reject connections that do not present a valid User-Agent or that do not support specific TLS cipher suites. You can override the probe behavior by providing a custom probe configuration file that sets additional headers or extends the timeout. Note that the project intentionally limits probes to lightweight checks to avoid placing load on third-party infrastructure.

**Q: How often is the manifest updated?**

A: The manifest is not automatically updated by the project maintainers. Users are expected to run the `probe` command periodically according to their own operational requirements. The seed file and the initial manifest are provided as static snapshots for bootstrapping purposes. For production deployments, we recommend scheduling the prober to run once every 24 hours and to store the resulting manifest in a version-controlled repository or object storage bucket for historical tracking.

**Q: Can I use this catalog to redistribute subtitle files or media content?**

A: No. Project Meliora is explicitly designed as a read-only metadata index. It does not store, cache, or proxy any substantive content from the listed endpoints. The manifest contains only URIs and probe metadata (timestamps, status codes). Any redistribution of content obtained via the listed endpoints is the sole responsibility of the user and must comply with all applicable copyright laws and terms of service. The project maintainers do not grant any additional rights to the content referenced in the catalog.

## 许可证

MIT License

Copyright (c) 2026 Project Meliora Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-08-25 22:00:19
