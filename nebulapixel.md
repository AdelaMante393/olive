# Terminus Resource Gateway

Terminus Resource Gateway is a curated technical index and external resource aggregation system designed for developers, technical researchers, and content archivists who require structured access to specialized online media references and subtitle-related knowledge bases. The project does not host any copyrighted or proprietary content; it serves exclusively as a navigational layer that organizes publicly available reference materials into a coherent, queryable catalog with strict version control and validation pipelines.

Target users include developers building language-learning tools, researchers analyzing subtitle alignment patterns, and system administrators who need to maintain reproducible external resource inventories for compliance or archival purposes. The gateway addresses the fundamental problem of link rot and inconsistent naming conventions in community-maintained resource lists by providing a standardized ingestion format, automated reachability testing, and human-readable documentation that evolves with the underlying sources.

## Functionality Overview

- **Structured Resource Indexing** – Each external link is stored with metadata including last verification timestamp, content type hints, and suggested category tags. The indexing engine supports incremental updates without full re-scan.

- **Automated Health Checking** – A background worker performs daily HEAD requests and content-type assertions against every registered URL. Unreachable or mismatched endpoints are flagged in the monitoring dashboard.

- **Subtitle Reference Mapping** – The system maintains a normalized mapping between domain patterns and common subtitle naming conventions, enabling fuzzy search across distributed subtitle collections without centralized storage.

- **Batch Ingestion Pipeline** – Administrators can submit batches of URLs (as in batch 57/63) via a CLI tool or web form. The pipeline validates syntax, resolves redirects, and assigns persistent internal identifiers.

- **Audit Trail Logging** – Every modification, verification attempt, and user query is logged with ISO-8601 timestamps and client IP hashes. Logs are retained for 90 days and can be exported for compliance reviews.

- **Read-Only Public Interface** – The frontend exposes a read-only view of the indexed resources with sorting, filtering, and simple keyword search. No write operations are exposed over the public endpoint.

- **Metadata Export Module** – Resources can be exported as JSON, YAML, or plain text lists. Export includes all stored fields plus the most recent health check result.

## Use Cases

- **Language Tool Developers** – Developers building subtitle-alignment utilities for language learning platforms use the gateway to discover and monitor subtitle reference domains without manually curating bookmark files across team members.

- **Archival Compliance Officers** – Organizations required to document external data sources for regulatory audits rely on the gateway's audit trail and health check logs to prove due diligence in maintaining accessible references.

- **Academic Researchers** – Researchers studying subtitle distribution patterns or online media accessibility use the structured index to sample domain availability trends over time, leveraging the historical verification records.

- **DevOps Automation Scripts** – System administrators incorporate the gateway's export endpoints into configuration management playbooks, automatically pulling the latest verified resource lists for firewall whitelist generation or proxy configuration.

## Quick Start

```bash
# Clone the repository
git clone https://github.com/terminus-resource/gateway.git terminus-gateway

# Change into the project directory
cd terminus-gateway

# Install dependencies using pip (Python 3.9+ required)
pip install -r requirements.txt

# Copy example environment configuration
cp .env.example .env

# Initialize the local SQLite database and run migrations
python manage.py migrate

# Start the development server on port 8000
python manage.py runserver 0.0.0.0:8000
```

After starting the server, access the web interface at `http://localhost:8000`. The default admin credentials are printed to the console during first startup. Change these immediately in production deployments.

## Installation Requirements

| Dependency | Required Version | Description |
|------------|------------------|-------------|
| Python | 3.9 or higher | Core runtime. Earlier versions lack required typing features. |
| SQLite | 3.31 or higher | Embedded database for metadata storage. Production deployments may use PostgreSQL via optional driver. |
| requests | 2.28.x | HTTP client library used for health checking and URL validation. |
| click | 8.1.x | CLI framework for administrative commands and batch ingestion tools. |
| python-dotenv | 1.0.x | Environment variable loading for configuration separation. |
| pytest | 7.4.x | Test framework (development dependency, not required for runtime). |
| gunicorn | 21.2.x | WSGI server recommended for production deployments. |
| sentry-sdk | 1.40.x | Optional error reporting integration. Disabled if DSN not set. |

## Documentation Navigation

| Layer | Directory | Questions Addressed |
|-------|-----------|---------------------|
| User Manual | /docs/user/ | How do I query the index? How do I interpret health check statuses? What export formats are supported? |
| Administration Guide | /docs/admin/ | How do I configure the health checker interval? How do I manually override a resource category? How do I restore from a backup? |
| API Reference | /docs/api/ | What endpoints are available? What authentication is required for write operations? What pagination parameters are accepted? |
| Developer Guide | /docs/dev/ | How do I extend the indexing engine? How do I add a new content-type validator? How do I run the test suite? |
| Deployment Handbook | /docs/deploy/ | What are the recommended system requirements? How do I set up HTTPS termination? How do I configure logging aggregation? |
| Batch Processing | /docs/batch/ | How do I prepare a batch manifest file? What validation rules apply to URLs in a batch? How do I roll back a batch? |

## Resource List

### Subtitle Reference Domains

<code>zaixianbofang2.org.cn</code>

<code>zhubofuli.org.cn</code>

<code>zhongwenzimudianying.org.cn</code>

<code>zhongwenzimuwangzhan.org.cn</code>

<code>zhongwenzimuzaixianmianfeikan2.org.cn</code>

<code>zaixianguankanwangyeshipin2.org.cn</code>

<code>zaixianzhongwenzimu.org.cn</code>

## Project Structure

```
terminus-gateway/
├── .env.example                    # Sample environment variables with all configurable options
├── .gitignore                      # Standard ignore rules for Python, SQLite, and IDE artifacts
├── README.md                       # This document – entry point for all users
├── requirements.txt                # Production runtime dependencies pinned to known-good versions
├── setup.py                        # Setuptools configuration for package installation
├── manage.py                       # Django-style management script for CLI operations
├── gateway/
│   ├── __init__.py                 # Package initializer
│   ├── settings.py                 # Main configuration module (loads .env, conditional overrides)
│   ├── urls.py                     # URL routing table for all public and admin endpoints
│   ├── wsgi.py                     # WSGI application entry for production servers
│   ├── asgi.py                     # ASGI application entry for async workers (experimental)
│   ├── models/
│   │   ├── __init__.py             # Model registry
│   │   ├── resource.py             # Resource model: URL, category, added_by, timestamp
│   │   ├── health.py               # HealthCheck model: status, response_time, last_checked
│   │   └── batch.py                # Batch model: manifest_hash, imported_count, status
│   ├── views/
│   │   ├── __init__.py             # View registry
│   │   ├── public.py               # Public read-only endpoints (listing, search, export)
│   │   ├── admin.py                # Admin endpoints (batch ingest, manual update, delete)
│   │   └── health.py               # Health check trigger and status reporting endpoints
│   ├── services/
│   │   ├── __init__.py             # Service registry
│   │   ├── checker.py              # Background health checker – uses threading.Timer
│   │   ├── parser.py               # URL parser and normalizer – handles IDN and punycode
│   │   ├── exporter.py             # Export formatter – JSON, YAML, plain text
│   │   └── validator.py            # Input validator – enforces allowed schemes and domain patterns
│   ├── cli/
│   │   ├── __init__.py             # CLI registry
│   │   ├── ingest.py               # Batch ingestion command – reads CSV/JSON manifests
│   │   ├── verify.py               # Manual verification command – checks a single URL
│   │   └── export.py               # Export command – writes resources to stdout or file
│   ├── templates/
│   │   ├── base.html               # Base HTML template with common header and footer
│   │   ├── index.html              # Homepage with search bar and quick stats
│   │   ├── list.html               # Resource listing with pagination and filter controls
│   │   └── detail.html             # Single resource detail page with health history
│   └── static/
│       ├── css/
│       │   └── style.css           # Minimal CSS – dark theme preferred, responsive grid
│       └── js/
│           └── dashboard.js        # Client-side filtering and auto-refresh for health status
├── tests/
│   ├── __init__.py                 # Test package initializer
│   ├── test_models.py              # Unit tests for Resource, Health, Batch models
│   ├── test_services.py            # Unit tests for checker, parser, exporter, validator
│   ├── test_cli.py                 # Integration tests for CLI commands using Click runner
│   └── fixtures/
│       └── sample_batch.csv        # Sample manifest file for batch ingestion testing
├── docs/
│   ├── user/
│   │   └── index.md                # User manual – search syntax, export usage, dashboard tour
│   ├── admin/
│   │   └── index.md                # Admin guide – configuration, backup, monitoring setup
│   ├── api/
│   │   └── index.md                # API reference – endpoint signatures, examples, error codes
│   ├── dev/
│   │   └── index.md                # Developer guide – coding standards, test strategy, extension points
│   └── deploy/
│       └── index.md                # Deployment handbook – systemd service, nginx config, SSL
└── scripts/
    ├── backup.sh                   # Daily backup script – dumps SQLite to compressed archive
    ├── restore.sh                  # Restore script – loads backup into clean database
    └── health_worker.py            # Standalone health worker daemon – runs as separate process
```

## Contribution Guidelines

1. **Fork and Branch** – Fork the main repository and create a feature branch with a descriptive name matching the pattern `feature/description` or `fix/issue-number`. All changes must be rebased against the latest `main` branch before submission.

2. **Run Test Suite** – Execute `pytest tests/` from the project root. Ensure all tests pass with zero failures. New features must include corresponding unit or integration tests with at least 80% coverage for the changed code paths.

3. **Update Documentation** – For any user-facing change (new CLI flag, changed endpoint behavior, new configuration variable), update the relevant documentation file under `/docs/`. Include a brief example showing the new usage.

4. **Submit Pull Request** – Open a pull request against the `main` branch with a clear title and a detailed description referencing any related issues. The PR template must be filled out completely, including the testing checklist and documentation impact statement.

5. **Code Review Compliance** – Address all review comments within five business days. Maintainers reserve the right to close pull requests that remain unaddressed for longer periods. All commits must be signed off with `git commit -s` to indicate acceptance of the Developer Certificate of Origin.

## Frequently Asked Questions

**Q: How often are the resources automatically verified?**

The background health checker runs every 24 hours starting at 02:00 UTC. Each verification attempts a HEAD request with a 10-second timeout. If HEAD is not supported, it falls back to GET with range headers. Results are stored in the health history table. Manual verification can be triggered at any time via the admin interface or CLI `verify` command.

**Q: What should I do if a resource is flagged as unreachable?**

First, manually verify the URL using the CLI tool with the `--follow-redirects` flag to check if the endpoint has moved. If the resource is permanently gone, use the admin interface to mark it as "deprecated" rather than deleting it – this preserves the historical record. For temporary outages, the system will automatically retry during the next scheduled cycle. No action is required unless the resource remains unreachable for seven consecutive checks.

**Q: Can I host this gateway behind a corporate proxy or firewall?**

Yes. Set the `HTTP_PROXY` and `HTTPS_PROXY` environment variables in your `.env` file. The requests library and the health checker respect these variables. Additionally, you can configure the `GATEWAY_ALLOWED_NETWORKS` setting to restrict which source IP ranges are permitted to access the public interface, though this is optional and depends on your deployment architecture.

## License

MIT License

Copyright (c) 2026 Terminus Resource Gateway Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-08-25 22:00:25
