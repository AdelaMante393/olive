# TechNav Resource Aggregator

TechNav is a lightweight, developer-oriented technical resource aggregation and navigation system designed to streamline access to distributed multimedia assets, documentation mirrors, and community-driven content repositories. The project addresses the growing fragmentation of technical reference materials across multiple domains by providing a unified indexing layer with minimal runtime overhead.

The system targets technical professionals, content curators, and self-hosted deployment enthusiasts who require deterministic URL resolution, offline-capable resource manifests, and reproducible environment snapshots. Unlike general-purpose bookmark managers or search engines, TechNav treats each resource entry as a first-class artifact with checksum verification, latency probing, and category-based routing rules.

## 功能概览

- **Deterministic Resource Indexing** – Every external link is stored with its original protocol and hostname case-preserved, supporting both plain domain and fully-qualified URL forms without normalization interference.

- **Multi-Protocol Probe Engine** – Background health checks verify reachability of each registered endpoint using HTTP/HTTPS fallback logic, with configurable timeout and retry policies.

- **Category-Based Tagging System** – Resources are classified into tiers such as primary streaming sources, subtitle mirrors, archival backups, and geographic affinity groups.

- **Static Manifest Generation** – Produces JSON and YAML export formats suitable for CI/CD pipelines, container image builds, or offline documentation bundles.

- **Latency-Aware Routing Hint** – Associates each URL with observed response time percentiles, enabling client-side selection of optimal endpoints.

- **Dependency-Free Core** – The indexer and validator modules require no external libraries beyond Python standard library (3.9+), while the optional web dashboard uses only vanilla JavaScript and CSS.

- **Audit Trail Logging** – Records every resource addition, removal, and modification with timestamp and operator identity for compliance tracking.

## 应用场景

- **Offline Conference Distribution** – Event organizers pre-fetch all referenced resource manifests into local network caches, ensuring attendees can access materials without relying on external internet connectivity during workshops.

- **Geo-Distributed Team Sync** – Development teams spread across regions use the probe results to automatically redirect internal documentation requests to the nearest mirror, reducing cross-continent load times.

- **Deprecated Link Remediation** – Content maintainers run periodic manifest comparisons against upstream sources to detect broken or redirected URLs, generating alert reports before user-facing impact occurs.

- **Embedded Systems Baseline** – Firmware build environments with restricted outbound access import the resource manifest as a whitelist, allowing only pre-approved endpoints for dependency fetching.

- **Academic Reference Archiving** – Research groups preserve snapshot versions of external multimedia citations, associating each with the exact original URL string to satisfy reproducibility requirements.

## 快速开始

```bash
# Clone the repository with full history
git clone https://github.com/technav-io/technav-core.git
cd technav-core

# Install runtime dependencies (Python 3.9+ required)
pip install -r requirements.txt

# Initialize the resource manifest from bundled seed data
python -m technav.manager init --seed seed/manifest-v18.json

# Start the local web dashboard on development port 8080
python -m technav.dashboard serve --port 8080 --reload
```

The above commands will produce a running instance with the default resource set for batch 18/63. Access the dashboard at `http://localhost:8080` to view categorized link listings, run health probes manually, and export filtered manifests.

## 安装要求

| 依赖 | 必需版本 | 说明 |
|------|----------|------|
| Python | 3.9 - 3.12 | Core runtime interpreter; type hints require 3.9+ |
| pip | 21.0+ | Package installer for managing optional extras |
| git | 2.25+ | Required for clone operations and version tagging |
| curl | 7.68+ | Used by the probe engine for HTTP health checks |
| sqlite3 | 3.35+ | Embedded database for audit log and manifest cache |
| openssl | 1.1.1+ | TLS verification for HTTPS endpoints |
| gunicorn | 20.1+ | Production WSGI server (optional but recommended) |
| pytest | 7.0+ | Test framework for running the validation suite |
| nodejs | 16.x+ | Only required for frontend asset rebuilding (development) |

All version constraints are tested against the CI matrix defined in `.github/workflows/ci.yml`. Older versions may work but receive no active support.

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 入门 | `docs/getting-started/` | How to set up the indexer, run first probe, and interpret health status codes |
| 运维 | `docs/operations/` | How to configure periodic reconciliation, backup manifest snapshots, and migrate between manifest versions |
| API | `docs/api/` | How to invoke the REST endpoints for resource query, bulk insert, and tag management |
| 扩展 | `docs/extending/` | How to write custom probe handlers, add new category validators, and plug into external alerting systems |
| 内部 | `docs/internals/` | How the deterministic URL normalization bypass works and why case preservation is enforced |
| 测试 | `docs/testing/` | How to run the integration test suite against staging mirrors without affecting production state |

## 资源列表

The following URLs constitute the complete manifest for batch 18/63. Each entry is presented exactly as provided by the upstream curator, with no protocol addition, removal, or hostname modification. Protocol and domain casing are preserved per the archival specification.

**Primary Streaming Sources**

<code>zaixianbofangnidongdea.org.cn</code>

<code>shipinmianfeizaixianguankanb.org.cn</code>

<code>rihanzaixianmianfeishipinb.org.cn</code>

**Mirror and Backup Nodes**

<code>mianfeizhuijuwangzhanb.org.cn</code>

<code>zaixianbofangzhongwenzimuc.org.cn</code>

<code>shipinmianfeizaixianguankanf.org.cn</code>

<code>rimanzaixianguankanf.org.cn</code>

These endpoints are periodically validated by the probe engine. Operators may override the default validation interval (3600 seconds) via the `--probe-interval` flag in the manager module.

## 项目结构

```
technav-core/
├── src/
│   └── technav/                      # Main package root
│       ├── __init__.py               # Version and exports
│       ├── manager.py                # CLI entry point for manifest operations
│       ├── probe/                    # Health checking subsystem
│       │   ├── engine.py             # Multi-protocol probe orchestrator
│       │   ├── http.py               # Curl-based HTTP/HTTPS checker
│       │   └── scheduler.py          # Background task coordinator
│       ├── index/                    # Resource indexing and storage
│       │   ├── manifest.py           # JSON/YAML manifest parser and writer
│       │   ├── cache.py              # SQLite-backed metadata store
│       │   └── validator.py          # URL format and protocol validator
│       ├── dashboard/                # Web interface components
│       │   ├── serve.py              # Development server with live reload
│       │   ├── static/               # CSS, JS, and image assets
│       │   └── templates/            # Jinja2 HTML pages
│       └── audit/                    # Logging and compliance
│           ├── logger.py             # Structured log formatter
│           └── rotation.py           # Log file rotation policy
├── tests/                            # Unit and integration tests
│   ├── test_probe.py                 # Probe engine test cases
│   ├── test_manifest.py              # Manifest serialization tests
│   └── fixtures/                     # Sample manifests for testing
├── docs/                             # Full documentation set
│   ├── getting-started/              # Installation and first run
│   ├── operations/                   # Maintenance and upgrade guides
│   └── api/                          # REST API reference
├── scripts/                          # Utility scripts for deployment
│   ├── seed-db.py                    # Initial database population
│   └── export-stats.py               # Export probe statistics to CSV
├── requirements.txt                  # Core dependencies
├── requirements-dev.txt              # Development and test dependencies
├── setup.py                          # Package installation metadata
├── LICENSE                           # MIT license text
└── README.md                         # This document
```

Each subdirectory under `src/technav/` contains at least one module with comprehensive docstrings. The `scripts/` directory is not part of the installed package but is provided for operational convenience.

## 贡献指南

1. **Fork and Clone** – Create a personal fork of the repository and clone it locally. Use a dedicated branch for your changes, named according to the pattern `feature/<description>` or `fix/<issue-number>`.

2. **Run the Validation Suite** – Execute `pytest tests/` to ensure all existing tests pass. Add new tests for any added functionality or bug fixes. The CI pipeline will enforce a minimum coverage threshold of 85%.

3. **Update the Manifest Seed** – If your contribution involves adding, removing, or modifying resource URLs, edit the `seed/manifest-v18.json` file accordingly. Preserve the exact string representation of each URL per the archival rules documented in `docs/internals/normalization.md`.

4. **Document Changes** – Update the relevant documentation files under `docs/` to reflect your changes. For user-facing features, include a short example in the appropriate guide. For internal changes, update the inline comments and module docstrings.

5. **Submit a Pull Request** – Push your branch to your fork and open a pull request against the `main` branch of the upstream repository. Include a clear description of the changes, the motivation, and any manual testing performed. The maintainers will review within 5 business days.

## 常见问题

**Q: Why are URLs not normalized to a consistent format, e.g., all with https:// and without www?**

A: The project explicitly preserves the exact user-provided string to maintain compatibility with legacy systems that rely on case-sensitive or protocol-specific resolution. Normalization would break deterministic validation for endpoints that distinguish between http and https, or that treat www and non-www as different virtual hosts. The validator checks only syntactic well-formedness, not semantic equivalence.

**Q: How do I force a re-probe of all resources without restarting the dashboard?**

A: Send a POST request to the internal admin endpoint `/api/probe/force` with an empty JSON body, or use the CLI command `python -m technav.manager probe --force --all`. The dashboard also provides a "Refresh All" button in the status panel, which triggers an asynchronous background scan. Results are updated incrementally as each probe completes.

**Q: Can I use TechNav behind a corporate proxy that requires authentication?**

A: Yes. The probe engine respects the `HTTP_PROXY`, `HTTPS_PROXY`, and `NO_PROXY` environment variables. For authenticated proxies, set the `PROXY_USER` and `PROXY_PASS` environment variables or use the `--proxy-auth` flag with the manager command. Note that plaintext credentials in environment variables are discouraged for production; use a secrets manager or the encrypted credential store described in `docs/operations/proxy-support.md`.

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-25 21:59:37
