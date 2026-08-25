# HyperLink Resource Aggregator

HyperLink Resource Aggregator is a high-performance, open-source web navigation and resource aggregation platform designed for developers, researchers, and content curators who need to manage, organize, and distribute large volumes of external resource links. The system provides a lightweight, dependency-minimal indexing engine that transforms raw URL collections into structured, searchable, and categorised resource repositories.

Target users include open-source documentation maintainers, academic research groups, media archive specialists, and DevOps engineers who require reproducible and auditable link registries. The project solves the fundamental problem of link rot, inconsistent URL formatting, and manual curation overhead by providing a declarative YAML-based catalogue definition, automated link health checks, and static site generation capabilities.

## 功能概览

**Declarative Catalogue Definition** – Define entire resource collections using YAML or JSON schemas with support for custom tags, categories, and metadata fields.

**Automated Link Validity Probing** – Built-in asynchronous HTTP/HTTPS health checker that validates reachability, detects redirect chains, and logs response status codes.

**Static Site Generation Engine** – Transform catalogue definitions into fully self-contained HTML documentation sites with search, filtering, and pagination.

**Multi-Format Export Pipeline** – Export catalogues as Markdown tables, CSV spreadsheets, JSON APIs, or plain-text lists for integration with external toolchains.

**Tag-Based Browsing and Filtering** – Assign multiple tags per resource entry and generate dynamic category views without rebuilding the entire catalogue.

**Redirect and Canonical URL Normalisation** – Automatically detect and suggest canonical URL forms while preserving user-provided original strings for audit purposes.

**Cache-Aware Refresh Strategy** – Conditional HTTP requests reduce bandwidth usage and improve revalidation speed for large catalogues exceeding 10,000 entries.

## 应用场景

**Open-Source Documentation Repositories** – Project maintainers can embed a machine-readable resource section that automatically updates external reference links across multiple language versions, ensuring all cited tools, papers, and dependencies remain accessible.

**Academic Research Data Management** – Research groups compiling bibliographies, dataset repositories, or supplementary material indexes can use the system to maintain versioned link catalogues with expiry warnings and alternative location suggestions.

**Media Archive and Content Distribution** – Organisations managing large collections of media references, streaming source indices, or distributed content mirrors can structure their link portfolios with regional tags, language filters, and availability timestamps.

**Internal DevOps Knowledge Bases** – Infrastructure teams can document internal service endpoints, dashboard URLs, and monitoring dashboards with automated reachability checks that trigger alerts when critical internal resources become unreachable.

## 快速开始

```bash
# Clone the repository
git clone https://github.com/hyperlink-resource-aggregator/hra-core.git
cd hra-core

# Install dependencies
pip install -r requirements.txt

# Create a minimal catalogue file
echo "catalogue:
  - url: '<code>zaixianbofangnidongdea.org.cn</code>'
    category: streaming
  - url: '<code>shipinmianfeizaixianguankanb.org.cn</code>'
    category: streaming" > examples/minimal.yml

# Run the validation and generate static site
python -m hra.cli validate -c examples/minimal.yml
python -m hra.cli generate -c examples/minimal.yml -o ./dist

# Start the development server to preview
python -m http.server --directory ./dist 8000
```

## 安装要求

| Dependency | Version Requirement | Purpose / Notes |
|------------|---------------------|-----------------|
| Python | 3.9 or higher | Core runtime language; type hints require 3.9+ for generics support |
| aiohttp | 3.8.0 or higher | Asynchronous HTTP client library for concurrent link probing |
| pyyaml | 6.0 or higher | YAML parser and emitter for catalogue definition files |
| jinja2 | 3.1.0 or higher | Templating engine for static site generation with autoescaping |
| markdown | 3.4.0 or higher | Markdown-to-HTML converter used in export pipelines |
| click | 8.1.0 or higher | Command-line interface framework for subcommand routing |
| pytest | 7.0.0 or higher | Test framework (development dependency, not required for runtime) |
| ruff | 0.1.0 or higher | Linter and code formatter (development dependency) |

## 文档导航

| Documentation Layer | Directory / Entry Point | Questions Answered |
|---------------------|-------------------------|-------------------|
| User Manual | `docs/user/` | How do I define a catalogue? What are the schema fields? How do I run health checks? |
| API Reference | `docs/api/` | Which Python classes and functions are exposed for programmatic usage? |
| CLI Guide | `docs/cli/` | What subcommands exist? What arguments and options are available? |
| Deployment Guide | `docs/deployment/` | How do I deploy the generated static site to CDN, GitHub Pages, or a self-hosted server? |
| Schema Specification | `docs/schema/` | What is the exact YAML/JSON structure? Which fields are required or optional? |
| Contribution Workflow | `CONTRIBUTING.md` | How do I set up the development environment, run tests, and submit pull requests? |

## 资源列表

The following resource entries have been provided as part of the catalogue indexing batch 9/63. Each entry is preserved exactly as supplied, with no modifications to protocol prefixes, subdomain structure, or trailing characters.

### Streaming Media Index Category

<code>zaixianbofangnidongdea.org.cn</code>

<code>shipinmianfeizaixianguankanb.org.cn</code>

<code>rihanzaixianmianfeishipinb.org.cn</code>

<code>mianfeizhuijuwangzhanb.org.cn</code>

<code>zaixianbofangzhongwenzimuc.org.cn</code>

<code>shipinmianfeizaixianguankanf.org.cn</code>

<code>rimanzaixianguankanf.org.cn</code>

## 项目结构

```
hra-core/
├── src/
│   └── hra/
│       ├── __init__.py          # Package initialisation, version constant
│       ├── cli.py               # Click command group, subcommand routing
│       ├── catalogue.py         # Catalogue data model, validation logic
│       ├── checker.py           # Async HTTP prober, redirect resolver
│       ├── export.py            # Markdown, CSV, JSON, HTML exporters
│       └── templates/           # Jinja2 base templates for static generation
│           ├── base.html        # Layout skeleton with navigation
│           ├── index.html       # Landing page with category grid
│           └── detail.html      # Single resource detail view
├── tests/
│   ├── unit/                    # Unit tests for each module (90%+ coverage)
│   ├── integration/             # End-to-end tests with sample catalogues
│   └── fixtures/                # YAML/JSON sample files for test scenarios
├── docs/
│   ├── user/                    # User manual chapters (Markdown)
│   ├── api/                     # Autogenerated API docstrings via Sphinx
│   ├── cli/                     # CLI command reference with examples
│   ├── deployment/              # Deployment recipes for various platforms
│   └── schema/                  # Full schema definition with annotations
├── examples/
│   ├── minimal.yml              # Smallest valid catalogue definition
│   ├── full-featured.yml        # All supported fields and tags demonstrated
│   └── batch-09.yml             # Catalogue containing the 63rd batch entries
├── scripts/
│   ├── pre-commit.sh            # Git pre-commit hook for linting
│   └── validate-examples.sh     # CI script that validates all example files
├── requirements.txt             # Production dependencies list
├── requirements-dev.txt         # Development and testing dependencies
├── pyproject.toml               # PEP 621 project metadata, ruff configuration
├── pytest.ini                   # Pytest discovery and plugin settings
├── .gitignore                   # Ignore patterns for version control
├── LICENSE                      # MIT License full text
└── README.md                    # This document
```

## 贡献指南

1.  **Fork and Clone** – Fork the official repository to your GitHub account and clone the fork locally. Set up the upstream remote to track the main repository for synchronising changes.

2.  **Create a Feature Branch** – Branch from the `main` branch using a descriptive name prefixed with the issue number (e.g., `feat/123-add-retry-logic`). Keep changes focused and atomic to simplify code review.

3.  **Run Development Checks** – Install development dependencies with `pip install -r requirements-dev.txt`. Run `ruff check .` for linting, `pytest` for the full test suite, and `python -m hra.cli validate -c examples/*.yml` to verify all examples remain valid.

4.  **Write or Update Documentation** – For any user-facing feature, update the relevant manual section in `docs/user/` and add a usage example. For API changes, regenerate the API reference using the provided Sphinx script.

5.  **Submit a Pull Request** – Push your branch and open a pull request against the `main` branch. Fill out the PR template completely, including the testing checklist and documentation impact statement. Pull requests must pass all CI checks before maintainers will review.

## 常见问题

**Q: The link checker reports a timeout for many entries. How can I adjust the timeout and retry settings?**

A: You can configure timeout and retry parameters via the command line using `--timeout` (default 10 seconds) and `--retries` (default 3 attempts). For persistent timeouts, consider running the checker with `--concurrency` set to a lower value (e.g., 5) to reduce network contention, or use the `--exclude` flag to temporarily skip problematic domains while troubleshooting.

**Q: How do I preserve the exact original URL strings in the generated output without normalisation or encoding changes?**

A: The system stores user-provided URLs in a dedicated `raw_url` field and performs all validation against a normalised copy. The export pipeline always prints the `raw_url` field when generating Markdown lists or HTML displays. Ensure your catalogue definition uses the `url` key exactly as you intend to display it – the system will not add or remove protocols, subdomains, or trailing slashes unless you explicitly enable the `canonicalise` option (disabled by default).

**Q: Can I use this system for private/internal URLs that are not publicly resolvable?**

A: Yes. The link checker accepts a `--skip-validation` flag that bypasses all HTTP probing, treating every entry as valid for generation purposes. This is suitable for air-gapped environments, internal network references, or draft catalogues that are not yet deployed. Additionally, you can provide a custom `--probe-whitelist` file containing IP ranges or domain patterns that the checker should treat as reachable without performing actual network requests.

## 许可证

This project is distributed under the MIT License. See the `LICENSE` file in the repository root for the full license text. You are free to use, modify, distribute, and sublicense this software for commercial and non-commercial purposes, provided that the original copyright notice and permission notice are retained in all copies or substantial portions of the software.

> 外链数量: 7 | 生成时间: 2026-08-25 21:59:37
