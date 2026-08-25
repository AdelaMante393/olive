# Zimuxi Online Video Resource Aggregator

Zimuxi is a curated technical aggregation platform designed to assist developers, content researchers, and media analysts in navigating the fragmented landscape of online Chinese subtitle and video resources. The project addresses the persistent challenge of locating stable, accessible, and updated subtitle sources for Chinese-language media content by providing a structured, machine-readable index of high-availability domain resources.

Target users include open-source developers building media playback tools, academic researchers conducting content analysis, and DevOps engineers automating media metadata collection. Zimuxi does not host, stream, or distribute copyrighted content. It functions exclusively as a discovery and reference layer, publishing domain metadata, availability status, and response-time metrics for public-facing subtitle and video platforms. All resource entries are community-reported and periodically validated through automated health checks.

## 功能概览

- **Domain Availability Monitoring** – Automated daily HEAD requests against each indexed domain, recording HTTP status codes and response latency, with historical uptime trending.

- **Subtitle Language Tagging** – Each domain entry is annotated with primary subtitle languages (Simplified Chinese, Traditional Chinese, bilingual) and content categories (movies, TV series, variety shows, documentaries).

- **Geographic Access Profiling** – Resource entries include observed geographic restrictions (CDN regions, country-level blocks) derived from community-contributed access logs.

- **Metadata Extraction Pipeline** – Parses public HTML meta tags and Open Graph data from indexed domains to extract site titles, descriptions, and content update frequencies.

- **Change Detection Notifications** – Triggers alert events when indexed domains change their SSL certificates, response headers, or root path content signatures, enabling rapid adaptation to site migrations.

- **Community-Submitted Annotations** – Registered users can append free-text notes to any domain entry, documenting observed playback compatibility, subtitle sync offsets, or regional mirror links.

- **Exportable Data Formats** – Provides JSON, YAML, and CSV exports of the full resource index for integration into external monitoring dashboards or data analysis workflows.

## 应用场景

- **Media Player Development** – Developers building cross-platform video players can use Zimuxi's domain index to pre-populate subtitle source options, offering users a curated set of fallback URLs for Chinese subtitle retrieval without manual searching.

- **Content Availability Research** – Academic researchers studying the lifecycle of online media resources can leverage the historical availability metrics to model domain persistence, migration patterns, and censorship-induced churn in the Chinese subtitle ecosystem.

- **DevOps Automation Scripts** – Site reliability engineers can integrate Zimuxi's JSON export into their monitoring stacks, creating automated alerts when critical subtitle domains become unreachable, allowing rapid reconfiguration of media services.

- **Localization Tooling Integration** – Localization platforms that synchronize subtitle assets across multiple projects can use Zimuxi as a discovery seed, automatically testing each domain for subtitle format compatibility (SRT, ASS, VTT) before inclusion in their asset pipelines.

- **Educational Demonstration** – Instructors teaching web scraping and data pipeline design can use Zimuxi's domain list as a safe, real-world dataset for building distributed crawlers, implementing polite crawling policies, and parsing semi-structured HTML metadata.

## 快速开始

```bash
# 1. Clone the repository
git clone https://github.com/zimuxi-io/zimuxi-aggregator.git
cd zimuxi-aggregator

# 2. Install Python dependencies using pip and virtual environment
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt

# 3. Run the availability checker against all indexed domains
python check_availability.py --config config/production.yaml --output results/

# 4. Generate the static resource catalog (HTML and JSON)
python build_catalog.py --input results/availability.json --output docs/

# 5. Start the local development server to preview the catalog
python -m http.server 8080 --directory docs/
```

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Python | 3.9 或更高 | 核心运行环境，用于执行检查脚本和构建工具 |
| pip | 22.0 或更高 | Python 包管理器，用于安装依赖库 |
| aiohttp | 3.9.0 或更高 | 异步 HTTP 客户端，用于并发执行域名健康检查 |
| beautifulsoup4 | 4.12.0 或更高 | HTML 解析库，用于提取域名页面的元数据信息 |
| pyyaml | 6.0 或更高 | YAML 配置文件解析器，用于读取运行参数 |
| pandas | 2.0.0 或更高 | 数据分析库，用于生成统计报告和导出表格数据 |
| redis | 5.0.0 或更高 | 可选缓存后端，用于存储历史检查结果以加速趋势分析 |
| docker | 24.0.0 或更高 | 可选容器化部署，用于标准化生产环境运行 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|-----------|
| 用户手册 | docs/user-guide/ | 如何配置监控频率、如何添加自定义域名、如何解读可用性报表 |
| 开发者指南 | docs/developer-guide/ | 贡献检查插件的方式、元数据提取器接口规范、单元测试编写要求 |
| 运维手册 | docs/operations/ | 生产环境部署步骤、Redis 缓存配置、日志轮转策略和告警规则设置 |
| API 参考 | docs/api/ | 输出的 JSON 和 YAML 数据结构定义、字段说明、示例响应体 |

## 资源列表

以下为 Zimuxi 项目当前索引的全部公开域名资源。条目按内容特征和社区反馈类别分组呈现。

影视综合类

<code>zuixinzhongwenzimuzaixian.org.cn</code>

<code>zhongwenzaixianguankanshipin.org.cn</code>

<code>renqizaixianmianfeishipin.org.cn</code>

播放源与字幕库类

<code>zhongwenzimuzaixianyingyuan.org.cn</code>

<code>zhongwenzimuzaixiankanpian.org.cn</code>

免费资源聚合类

<code>mianfeishipinzhongwenzimu.com.cn</code>

<code>zaixianzhongwenzimuwangzhan.com.cn</code>

## 项目结构

```
zimuxi-aggregator/
├── config/                              # 配置文件目录
│   ├── production.yaml                  # 生产环境配置（超时、重试、并发数）
│   ├── staging.yaml                     # 预发布环境配置
│   └── domains.yaml                     # 核心域名列表（YAML 格式）
├── src/                                 # 源代码主目录
│   ├── checker/                         # 可用性检查模块
│   │   ├── http_client.py               # 异步 HTTP 请求封装
│   │   ├── status_reporter.py           # 状态报告生成器
│   │   └── ssl_validator.py             # SSL 证书有效期验证
│   ├── parser/                          # 元数据解析模块
│   │   ├── html_meta_extractor.py       # 从 HTML 提取 meta 标签
│   │   └── open_graph_parser.py         # Open Graph 协议解析
│   ├── storage/                         # 数据持久化模块
│   │   ├── redis_cache.py               # Redis 缓存操作接口
│   │   └── json_exporter.py             # JSON 和 CSV 格式导出
│   └── notify/                          # 通知模块
│       ├── webhook_sender.py            # 发送 Webhook 告警
│       └── log_aggregator.py            # 日志聚合与格式化
├── tests/                               # 单元测试和集成测试
│   ├── test_checker.py                  # 检查器模块测试
│   ├── test_parser.py                   # 解析器模块测试
│   └── fixtures/                        # 测试用例固定样本
├── scripts/                             # 运维和辅助脚本
│   ├── init_db.py                       # 初始化缓存数据库
│   ├── migrate_domains.py               # 域名列表迁移工具
│   └── health_check_cron.sh             # 定时任务调度脚本（cron 入口）
├── docs/                                # 生成的静态文档输出目录
│   ├── index.html                       # 项目主页
│   ├── catalog.json                     # 域名目录 JSON 输出
│   └── catalog.yaml                     # 域名目录 YAML 输出
├── requirements.txt                     # Python 生产依赖列表
├── requirements-dev.txt                 # 开发附加依赖（pytest, black, mypy）
├── Dockerfile                           # 容器化部署定义
├── Makefile                             # 常用命令快捷方式（check, build, test）
└── README.md                            # 本文件
```

## 贡献指南

1.  **查阅现有议题和文档** – 访问 GitHub Issues 页面确认待办事项和已知问题，阅读开发者指南了解模块设计和编码规范，避免重复工作。

2.  **派生仓库并创建功能分支** – 从主仓库派生代码库到个人账户，使用 `git checkout -b feature/your-feature-name` 创建新分支，分支命名遵循 `feature/`、`fix/` 或 `docs/` 前缀约定。

3.  **编写代码并添加测试** – 所有新增检查器或解析器必须附带单元测试，覆盖率不低于 80%。使用 `make test` 运行完整测试套件，确保所有既有测试通过。

4.  **更新文档和域名列表** – 若新增或移除域名，需同步更新 `config/domains.yaml` 以及资源列表章节。对任何用户可见的变更，需补充对应文档说明。

5.  **发起拉取请求并等待审核** – 推送分支至个人远程仓库，通过 GitHub 发起 Pull Request 到主仓库的 `main` 分支。PR 描述需写明变更目的、测试结果和影响范围。至少一名项目维护者审核通过后方可合并。

## 常见问题

**问：Zimuxi 是否提供字幕文件下载或视频流播放服务？**

答：不提供。Zimuxi 严格定位为域名资源索引和可用性监控工具。项目不存储、缓存、分发或代理任何字幕文件、视频内容或流媒体数据。所有列出的域名均为公开可访问的第三方网站，用户访问这些域名时需遵守各自网站的使用条款。

**问：域名可用性检查的频率是多少？检查结果如何获取？**

答：默认检查频率为每 6 小时一次，可通过修改 `config/production.yaml` 中的 `check_interval_minutes` 参数进行调整。检查结果以 JSON 和 YAML 格式保存在 `docs/catalog.json` 和 `docs/catalog.yaml` 文件中，也可通过 Redis 缓存实时查询最新状态。历史趋势数据保留 90 天。

**问：我发现某个域名已经失效或迁移，如何向项目报告更新？**

答：可以通过三种方式报告：在 GitHub 仓库提交 Issue 并附上新的域名和验证信息；发送邮件至维护者邮箱；或者直接按照贡献指南提交包含更新域名列表的 Pull Request。项目维护者会验证变更并在合并前进行额外测试。

## 许可证

MIT License

Copyright (c) 2026 Zimuxi Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-08-25 22:00:41
