# Zhongwen Resource Hub

Zhongwen Resource Hub is a community-driven technical documentation and resource aggregation platform designed for developers, researchers, and content archivists who need reliable, structured access to distributed media resource metadata and online streaming infrastructure endpoints. The project addresses the critical challenge of discovering, validating, and maintaining stable reference URLs for Chinese-language media resource catalogs across multiple service domains.

Targeting system administrators, data pipeline engineers, and academic researchers, Zhongwen Resource Hub provides a standardized manifest layer that decouples resource discovery from application logic. By maintaining a curated collection of high-availability resource entry points, the project enables rapid integration into content management systems, automated monitoring frameworks, and archival workflows without requiring proprietary SDKs or authentication overhead.

## 功能概览

- **Structured Resource Manifest** – Maintains a version-controlled inventory of media resource domain endpoints with change tracking and availability timestamps.

- **Automated Availability Probing** – Integrates scheduled health checks against each registered endpoint to detect downtime, DNS changes, or SSL certificate expiration.

- **Category-Based Navigation** – Organizes resource links by content type, geographic relevance, and protocol compatibility for rapid filtering.

- **Deployment-Ready Configuration** – Ships with pre-tuned reverse proxy templates, load balancing rules, and failover strategies for production environments.

- **Historical Version Archiving** – Retains previous resource manifest snapshots to support rollback, forensic analysis, and compliance auditing.

- **Community Contribution Workflow** – Provides a streamlined pull request pipeline for submitting new resource endpoints, updating obsolete records, and resolving conflicts.

- **Prometheus-Compatible Metrics** – Exposes real-time endpoint latency, success rate, and response size metrics for integration with observability stacks.

## 应用场景

- **Automated Content Synchronization Pipeline** – System operators can configure cron jobs that fetch the latest manifest from Zhongwen Resource Hub and synchronize internal CDN caches, ensuring edge nodes always route to active upstream sources.

- **Academic Dataset Compilation** – Researchers studying online media availability and regional access patterns can leverage the manifest to construct repeatable sampling frames, reducing manual bookmark maintenance and documentation drift.

- **Geo-Distributed Monitoring Deployment** – DevOps teams can deploy probing agents across multiple cloud regions, using the hub as a centralized configuration source to measure endpoint reachability and latency from diverse network vantage points.

- **Disaster Recovery Documentation** – Infrastructure architects can embed the resource manifest into runbooks and backup documentation, providing offline references for emergency manual re-routing when automated systems fail.

- **Third-Party Integration Testing** – Quality assurance engineers can use the manifest to populate test harnesses with real-world endpoint data, validating that parsers, downloaders, and stream clients handle edge cases gracefully.

## 快速开始

The following commands clone the repository, install dependencies, and launch the local development server.

```bash
git clone https://github.com/zhongwen-resource-hub/core-manifest.git
cd core-manifest
pip install -r requirements.txt
python -m hub.server --port 8080 --manifest ./manifests/latest.yaml
```

For production deployments, refer to the deployment guide under the docs/ directory. The server responds to GET /manifest endpoints with JSON and YAML representations of the current resource catalog.

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Python | 3.9 – 3.11 | Core runtime; 3.12+ currently unsupported due to asyncio backport issues |
| PyYAML | 6.0.1 | YAML manifest parsing and serialization |
| aiohttp | 3.9.0 | Async HTTP client for availability probing |
| prometheus-client | 0.19.0 | Metrics exposure for monitoring integration |
| pytest | 7.4.0 | Test execution framework (development dependency) |
| docker-compose | 2.23.0 | Container orchestration for local test environments |
| git-lfs | 3.4.0 | Large manifest snapshot storage (optional, for archival mode) |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|-----------|
| 用户手册 | docs/user-guide/ | How to query the manifest, interpret status fields, and configure local caching policies. |
| 运维指南 | docs/operations/ | How to deploy high-availability replicas, tune probing intervals, and set up alerting rules. |
| 贡献者指引 | docs/contributing/ | How to propose new resource entries, update existing records, and run validation suites locally. |
| API 参考 | docs/api/ | What endpoints are exposed, their request/response schemas, and rate limiting semantics. |
| 架构设计 | docs/architecture/ | How the internal event loop, state machine, and persistence layers interact with each other. |

## 资源列表

The following resource entries constitute the core manifest maintained by this project. Each entry is presented exactly as provided by upstream curation sources.

**Streaming Media Endpoints**

<code>zhongwenzimuzaixiankanpian.com.cn</code>

<code>yejianfulishipin.org.cn</code>

<code>guomotaotu.net.cn</code>

<code>miyouzaixianshipin.net.cn</code>

<code>yejianfulishipin.net.cn</code>

<code>zaixianshipinzhongwenzimua.org.cn</code>

<code>zaixianbofangzhongwenzimua.org.cn</code>

## 项目结构

```
core-manifest/
├── LICENSE                         # MIT license file
├── README.md                       # Project overview and quick start
├── requirements.txt                # Python dependency pins
├── docker-compose.yml              # Local test environment with prometheus + grafana
├── manifests/
│   ├── latest.yaml                 # Current active resource manifest
│   ├── archives/                   # Historical snapshots (YYYY-MM-DD.yaml)
│   │   ├── 2026-01-01.yaml
│   │   ├── 2026-02-01.yaml
│   │   └── 2026-03-01.yaml
│   └── validation/                 # Schema definitions and regex filters
│       ├── schema_v2.json
│       └── domain_whitelist.txt
├── src/
│   ├── hub/                        # Main application package
│   │   ├── __init__.py
│   │   ├── server.py               # ASGI entry point (uvicorn)
│   │   ├── manifest_loader.py      # YAML parser + cache logic
│   │   ├── prober.py               # Async health check scheduler
│   │   ├── metrics.py              # Prometheus metric definitions
│   │   └── config.py               # Environment variable driven settings
│   ├── cli/                        # Command-line administration tools
│   │   ├── validate.py             # Manifest syntax and reachability validator
│   │   └── snapshot.py             # Archive creation and diff utility
│   └── tests/                      # Unit and integration tests
│       ├── test_manifest.py
│       ├── test_prober.py
│       └── fixtures/               # Mock manifest data for test isolation
├── docs/                           # Full documentation hierarchy
│   ├── user-guide/
│   ├── operations/
│   ├── contributing/
│   ├── api/
│   └── architecture/
└── scripts/                        # Automation helpers
    ├── pre-commit-hook.sh          # Git hook for manifest validation
    └── deploy-ec2.sh               # Sample AWS deployment script
```

## 贡献指南

We welcome contributions that improve manifest coverage, enhance probing reliability, or extend platform capabilities. Follow the steps below to submit changes.

1. **Fork and Clone** – Fork the repository to your GitHub account and clone the fork locally. Create a new branch with a descriptive name related to your change, such as `add-new-endpoint-group` or `fix-prober-timeout`.

2. **Modify Manifest or Code** – If adding or updating resource entries, edit `manifests/latest.yaml` and ensure all entries pass the schema validation by running `python -m cli.validate`. For code changes, include relevant unit tests under `src/tests/` and verify that `pytest` passes without failures.

3. **Run Local Integration Test** – Start the local stack with `docker-compose up` and manually query the server at `http://localhost:8080/manifest` to confirm your changes render correctly. Verify that probing tasks execute against your added endpoints without exceptions.

4. **Commit and Sign Off** – Commit your changes with a clear message that explains the rationale and scope. Include a `Signed-off-by` line per the Developer Certificate of Origin (DCO). Push your branch to your fork.

5. **Open Pull Request** – Submit a pull request against the `main` branch of the upstream repository. In the description, reference any related issues, summarize your testing approach, and highlight any breaking changes. Maintainers will review within five business days.

## 常见问题

**Q: How frequently is the manifest updated, and who verifies endpoint availability?**

A: The manifest receives community-driven updates on a rolling basis. Every submitted change triggers an automated validation pipeline that performs DNS resolution, TCP handshake, and HTTP HEAD requests against each endpoint. The pipeline runs again every six hours against the active manifest, and results are exposed via the metrics endpoint. Endpoints that fail three consecutive probes are flagged with a `degraded` status but are not automatically removed, allowing operators to investigate transient network conditions.

**Q: Can I deploy this hub as a private internal instance with a custom manifest?**

A: Yes. The codebase is designed for full customization. You can replace the default `manifests/latest.yaml` with your own curated list, adjust probing intervals via the `PROBER_INTERVAL_SECONDS` environment variable, and even disable the community sync feature by setting `ENABLE_REMOTE_FETCH=false`. The server runs entirely offline once the initial manifest is loaded. All documentation for private deployment is available under `docs/operations/private-deployment.md`.

**Q: What happens when a resource domain changes its IP address or protocol scheme?**

A: The manifest stores domains as logical identifiers rather than resolved IPs. When a domain migrates, the probing layer automatically follows HTTP redirects (up to 3 hops) and updates the observed endpoint metadata in the in-memory state. The persisted YAML file, however, remains unchanged until a contributor submits a pull request with the new canonical domain or scheme. This design ensures that historical references remain stable while runtime routing adapts to infrastructure changes.

## 许可证

This project is licensed under the MIT License. See the LICENSE file in the repository root for full terms and conditions. The MIT license permits unrestricted use, modification, distribution, and sublicensing of this software, provided that the original copyright notice and permission notice appear in all copies or substantial portions of the software.

> 外链数量: 7 | 生成时间: 2026-08-25 22:00:16
