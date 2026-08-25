# Zhongwen Resources Hub

Zhongwen Resources Hub is a community-curated technical index and navigation system designed for developers, researchers, and content archivists who require structured access to Chinese-language media metadata, subtitle corpora, and online streaming platform references. The project does not host, store, or redistribute any copyrighted content; instead, it provides a deterministic, machine-readable registry of public-facing Uniform Resource Locators (URLs) that are frequently referenced in computational linguistics, digital preservation studies, and cross-regional media accessibility research.

This repository targets three primary user groups: (a) developers building language-processing pipelines that need sample subtitle datasets for model training or alignment evaluation; (b) researchers investigating the availability and persistence of Chinese online video resources over time; and (c) hobbyists who maintain personal media servers and require a reliable, version-controlled reference list for external subtitle sources. By centralizing these URLs into a single, well-documented index, the project reduces manual search overhead, minimizes broken-link propagation, and encourages collaborative maintenance of the underlying resource corpus.

## 功能概览

- **Structured URL Registry** – Maintains a categorized, versioned list of external URLs, each annotated with status codes, last-verified timestamps, and content-type hints (e.g., subtitle plaintext, video manifest, or portal index).

- **Automated Health Checks** – Includes a lightweight Python-based verifier that performs HEAD and GET requests against each registered URL, reporting HTTP status changes, redirect chains, and connection timeouts in a daily-updated JSON report.

- **Subtitle Corpus Metadata Extraction** – Provides helper scripts to parse common subtitle formats (.srt, .ass, .vtt) from referenced sources when they are publicly accessible, extracting duration, speaker labels, and dialogue frequency distributions without storing the underlying content.

- **Geo-Aware Mirror Suggestions** – Integrates a community-contributed database of regional mirrors and CDN endpoints, allowing users to select the nearest available source for reduced latency in media-access workflows.

- **Tag-Based Filtering System** – Supports custom tags such as `genre:documentary`, `lang:zh-CN`, `format:stream`, and `status:active` to enable fine-grained querying of the URL registry via CLI or RESTful API endpoints.

- **Change Notification Webhook** – Sends configurable alerts (via SMTP or Discord-compatible webhooks) when a monitored URL changes its response signature, enabling downstream projects to adapt their ingestion pipelines proactively.

- **Snapshot Comparison Tool** – Compares two historical versions of the URL registry and produces a side-by-side diff report, highlighting newly added, removed, or redirected entries for audit and compliance purposes.

## 应用场景

- **Academic Research in Media Availability** – A computational social scientist uses the registry to track the long-term availability of Chinese online video portals, correlating URL lifespans with policy changes and network infrastructure upgrades. The structured metadata enables reproducible longitudinal studies without manual bookmark curation.

- **Subtitle Corpus Construction for NLP** – A natural language processing team leverages the URL list to automatically fetch publicly accessible subtitle files from multiple sources, aggregating them into a cleansed, deduplicated corpus for training domain-specific language models in code-switching and colloquial Chinese dialogue understanding.

- **Personal Media Server Synchronization** – A home media server operator configures the health-check script as a cron job, receiving notifications when a frequently used subtitle source becomes temporarily unavailable or migrates to a new domain. This allows proactive adjustments to the server's external dependency list.

- **Educational Workshop on Web Archiving** – Instructors use the repository as a teaching case for web archiving techniques, demonstrating how to systematically document and verify external resource endpoints using version control, continuous integration, and automated monitoring pipelines in a classroom setting.

## 快速开始

Clone the repository, install the minimal Python dependencies, and run the initial verification suite using the following commands:

```bash
git clone https://github.com/zhongwen-resources-hub/core-index.git
cd core-index
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python main.py --verify --output report.json
```

The `main.py` script accepts optional arguments such as `--tag` to filter specific categories, `--retries` to adjust network tolerance, and `--webhook` to enable notification delivery. For the first execution, the script will generate a `registry.json` file containing the full URL list with default status placeholders.

## 安装要求

All components are written in Python 3.9+ and rely on a minimal set of third-party libraries. The following table summarizes the mandatory and optional dependencies:

| 依赖 | 必需 | 说明 |
| :--- | :--- | :--- |
| Python 3.9+ | 是 | Core interpreter; type annotations and async features require 3.9 or later |
| requests >= 2.28.0 | 是 | Handles HTTP verification, redirect following, and custom timeout settings |
| pydantic >= 2.0.0 | 是 | Provides data validation and serialization for the registry schema |
| jsonschema >= 4.17.0 | 是 | Validates external contributor-submitted JSON patches against the registry schema |
| pytest >= 7.0.0 | 否 | Required only for running the test suite during development or CI builds |
| black >= 22.0.0 | 否 | Optional code formatter for maintaining consistent style across pull requests |
| mkdocs >= 1.4.0 | 否 | Used to generate the static documentation site from the `docs/` folder |
| mypy >= 1.0.0 | 否 | Static type checker; recommended for contributors writing new verification modules |

## 文档导航

The documentation is organized to serve both casual users and advanced integrators. The table below outlines the primary documentation layers and their intended questions:

| 层面 | 目录 | 回答的问题 |
| :--- | :--- | :--- |
| User Guide | `docs/user/` | How do I install the tool? How do I run a basic verification? What do the output fields mean? |
| API Reference | `docs/api/` | Which functions are exposed for programmatic access? How do I extend the verifier with custom checks? |
| Registry Schema | `docs/schema/` | What is the exact JSON structure of the registry? Which fields are mandatory and which are optional? |
| Contributor Handbook | `docs/contributor/` | How do I propose a new URL entry? What are the review criteria? How do I update an existing entry? |
| Operations Guide | `docs/ops/` | How do I set up the webhook notifier? How do I schedule daily verification using cron or systemd timers? |
| FAQ & Troubleshooting | `docs/faq/` | Why does a URL show as timeout? How do I handle rate-limiting from source portals? |

## 资源列表

The following URLs constitute the core registry index. They are presented exactly as provided, without any normalization, protocol addition, or domain modification. Each entry is enclosed in a code tag to preserve its literal form. Categories are assigned based on nominal content-type inference from the domain naming conventions.

### Subtitle Source Portals

- <code>zuixinzhongwenzimuzaixian.org.cn</code>

- <code>zhongwenzaixianguankanshipin.org.cn</code>

- <code>renqizaixianmianfeishipin.org.cn</code>

- <code>zhongwenzimuzaixianyingyuan.org.cn</code>

- <code>zhongwenzimuzaixiankanpian.org.cn</code>

### Streaming and Video Access References

- <code>mianfeishipinzhongwenzimu.com.cn</code>

- <code>zaixianzhongwenzimuwangzhan.com.cn</code>

## 项目结构

The repository follows a modular layout to separate core logic, configuration, test assets, and user-facing documentation. Each directory includes an `__init__.py` file where appropriate to enable package imports.

```
core-index/
├── main.py                      # Entry point for CLI verification and report generation
├── registry.json                # Master URL registry with metadata and status history
├── config/
│   ├── default.yaml             # Default timeouts, retry policies, and webhook endpoints
│   └── schema_v2.json           # JSON Schema for validating registry entries
├── src/
│   ├── verifier/                # HTTP verification engine with async support
│   │   ├── client.py            # Wrapper around requests with retry and backoff logic
│   │   ├── parser.py            # Parses HTML titles and meta tags for content-type hints
│   │   └── reporter.py          # Generates JSON and Markdown summary reports
│   ├── webhook/                 # Notification delivery modules
│   │   ├── smtp_sender.py       # Sends email alerts via SMTP
│   │   └── discord_webhook.py   # Posts status updates to Discord-compatible channels
│   └── utils/
│       ├── logging.py           # Structured logging with rotation and verbosity levels
│       └── diff.py              # Computes structural diffs between registry versions
├── tests/
│   ├── unit/                    # Unit tests for individual functions and classes
│   ├── integration/             # End-to-end tests that perform actual HTTP calls
│   └── fixtures/                # Mock registry snapshots for deterministic testing
├── docs/                        # MkDocs source; see Documentation Navigation table above
│   ├── user/
│   ├── api/
│   ├── schema/
│   ├── contributor/
│   ├── ops/
│   └── faq/
├── scripts/
│   ├── daily_verify.sh          # Wrapper script for cron-based scheduled execution
│   └── import_legacy.py         # One-time script to import URLs from older CSV formats
├── requirements.txt             # Production dependencies pinned to known-good versions
├── requirements-dev.txt         # Development dependencies including linters and formatters
└── LICENSE                      # MIT License (see full text in the LICENSE file)
```

## 贡献指南

We welcome contributions that improve the reliability, coverage, or usability of the URL registry and its associated verification tooling. All contributions must adhere to the following steps:

1. Fork the repository and create a new branch with a descriptive name, such as `feature/add-verification-retry` or `fix/update-timeout-handling`. Ensure your branch is based on the latest `main` branch commit.

2. Update the registry entries or source code as needed. For registry changes, modify `registry.json` directly and run the schema validation script (`python scripts/validate_schema.py`) to confirm structural compliance. For code changes, include relevant unit tests under `tests/unit/` and ensure all existing tests pass with `pytest`.

3. Commit your changes with a clear, imperative-style commit message summarizing the intent and scope. Reference any related issue numbers if applicable. Push your branch to your fork and open a pull request against the upstream `main` branch.

4. In the pull request description, explicitly state whether the change introduces new dependencies, alters the output format, or modifies the verification behavior. Provide sample outputs or screenshots for user-facing changes.

5. Respond to reviewer feedback promptly. The maintainers will run additional integration tests against a staging environment before merging. Once merged, your changes will be included in the next versioned release.

## 常见问题

**Q: Why do some URLs in the registry return HTTP 403 or 429 errors during verification?**

A: Many content portals implement rate-limiting, geo-blocking, or bot-detection mechanisms to restrict automated access. Our verifier uses a conservative default retry policy (3 attempts with exponential backoff) and respects `Retry-After` headers when present. If a URL consistently returns 403, we recommend using the `--skip-verify` flag for that entry and periodically re-testing manually. The registry maintains a `last_ok` timestamp to track when a URL was last confirmed reachable.

**Q: How frequently is the registry updated, and can I contribute a URL that is temporarily offline?**

A: The master registry is updated on a best-effort basis through community contributions and automated daily health checks. We accept entries for URLs that are temporarily offline, provided they have a documented historical significance or are expected to return online within a reasonable timeframe. In such cases, include a `status_notes` field in your pull request describing the known downtime. The verification script will mark these entries as `soft_unreachable` rather than `dead`, distinguishing them from permanently removed resources.

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-25 22:00:16
