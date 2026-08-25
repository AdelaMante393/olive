# Lightning Resource Hub

Lightning Resource Hub 是一个专注于中文多媒体资源索引与导航的开源基础设施项目。本项目不存储、不分发、不托管任何实体媒体文件，仅作为公开可访问的 URL 引用集合与结构化元数据整理工具，面向内容整理者、研究人员、信息归档爱好者以及日常需要快速定位特定类别中文在线资源的终端用户。

该项目的核心定位是解决中文网络环境中高质量、特定主题资源入口分散、命名不稳定、检索效率低下的问题。通过社区驱动的 URL 维护机制、自动化可达性检测脚本以及清晰的项目内部分层架构，Lightning Resource Hub 将零散的链接转化为可复用、可审计、可版本控制的信息资产，帮助用户在合法合规的范围内高效完成信息导航与资源发现任务。

## 功能概览

**结构化链接索引** 提供多级分类目录与标签系统，将所有收录的 URL 按主题、语种、介质类型进行逻辑分组，支持快速筛选与定位。

**自动化可达性巡检** 内置基于 Python 的轻量级检查脚本，可定时或手动触发对全部收录链接的 HTTP 状态码验证，自动标记异常条目并生成健康度报告。

**元数据增强注解** 为每个资源条目附加字段说明，包括但不限于内容语言、字幕支持情况、预期清晰度等级、更新活跃度标记，提升检索与筛选的精确度。

**版本化变更追踪** 基于 Git 原生能力记录每一条链接的增删改操作与变更原因注释，支持回溯历史版本、对比差异以及回滚误操作。

**社区贡献工作流** 提供标准化的链接提交通道与审核模板，允许外部贡献者通过 Pull Request 提交新增资源或更新现有条目，并附带必要的验证信息。

**静态站点生成支持** 项目内包含可选的构建脚本，能够将索引数据渲染为纯静态 HTML 导航页面，便于内网部署或离线浏览。

**多格式数据导出** 支持将索引数据导出为 JSON、CSV、Markdown 表格等通用格式，方便下游工具链导入或二次加工。

## 应用场景

**个人研究者的主题资源归档** 研究人员在跟踪某一特定领域（例如中文影视文化研究、网络媒体语料收集）时，可使用本项目作为外部链接的外部引用池，集中管理分散在各处的相关页面，避免浏览器书签的混乱与丢失。

**小型团队的内容导航门户构建** 小型工作室或兴趣小组可利用本项目的静态站点生成功能，快速搭建内部使用的资源导航页面，无需从零开发后台管理系统，所有数据以纯文本形式维护在仓库中，便于协作与版本控制。

**信息整理爱好者的数据清洗起点** 对于习惯定期整理网络公开链接的爱好者，本项目提供的自动化可达性检测与元数据模板能够大幅减少手动验证工作量，将精力更多地投入到分类逻辑优化与注释补充上。

**开源社区文档的引用底座** 其他开源项目在撰写文档、教程或 README 时，若需要引用大量外部中文资源链接，可将本项目作为参考索引源，通过标准导出格式批量引入链接数据，保持自身文档的简洁性与可维护性。

## 快速开始

以下步骤适用于 Linux 与 macOS 环境，Windows 用户建议使用 WSL2 或 Git Bash 执行。

```bash
# 1. 克隆仓库到本地
git clone https://github.com/lightning-resource-hub/lrh-core.git
cd lrh-core

# 2. 安装项目依赖（推荐使用 Python 虚拟环境）
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# 3. 执行本地索引构建与基础验证
python scripts/build_index.py --input data/sources.yaml --output dist/
python scripts/check_health.py --target dist/index.json --timeout 5 --report health_report.md
```

执行完毕后，`dist/` 目录下将生成可供后续使用的结构化数据文件，`health_report.md` 为当前所有收录链接的状态报告。

## 安装要求

| 依赖项 | 必需版本 | 说明 |
|--------|----------|------|
| Python | 3.8 及以上 | 核心脚本运行环境，用于索引构建、健康检查及导出工具 |
| Git | 2.20 及以上 | 版本控制，用于克隆仓库、提交变更及协作流程 |
| PyYAML | 5.4.0 及以上 | 用于解析 `sources.yaml` 配置文件中的链接分类与元数据 |
| requests | 2.25.0 及以上 | 健康检查模块依赖，用于发送 HTTP 请求验证链接可达性 |
| pytest | 7.0.0 及以上 | 单元测试框架，用于在贡献前验证本地修改是否破坏既有功能（可选） |

## 文档导航

| 层面 | 目录/文件 | 回答的问题 |
|------|-----------|------------|
| 用户入门 | `docs/quick-start.md` | 如何首次使用本项目、如何获取导出的数据文件以及如何理解目录结构 |
| 贡献者指引 | `CONTRIBUTING.md` | 提交新链接的完整流程、PR 模板填写规范以及审核标准 |
| 运维手册 | `docs/operations/health-check-guide.md` | 如何配置自动化巡检、如何解读健康报告以及如何处理失效链接 |
| 数据格式参考 | `docs/schema-reference.md` | `sources.yaml` 中每个字段的类型、取值范围及示例说明 |
| 工具链开发 | `docs/developer-notes.md` | 扩展自定义导出格式或新增检查规则所需的接口与抽象类说明 |

## 资源列表

本列表按类别整理当前项目收录的全部外部链接，所有条目均按用户提供的原始形式原样呈现。项目本身不对任何链接所指向的内容的合法性、可用性及安全性作出担保，用户应自行遵守相关法律法规及网站服务条款。

类别：中文影视资源索引

<code>guochanzhubozaixianguankanw.org.cn</code>

<code>zhongwenshipinw.org.cn</code>

<code>zaixianbofangzhongwenzimuw1.org.cn</code>

<code>zhongwenzimugaoqingw.org.cn</code>

<code>renqimianfeishipinw.org.cn</code>

<code>zhongwenzimuzaixiankanw.org.cn</code>

<code>zuixinzhongwenzimuw.org.cn</code>

## 项目结构

```
lrh-core/
├── data/
│   ├── sources.yaml                # 主索引文件，包含所有链接分类、标签与元数据
│   ├── categories/                 # 分类定义子目录，按主题拆分 yaml 以降低合并冲突
│   │   ├── video.yaml
│   │   ├── subtitle.yaml
│   │   └── archive.yaml
│   └── overrides/                  # 本地覆盖或临时禁用条目的配置，不提交至上游
│       └── local_ignore.example.yaml
├── scripts/
│   ├── build_index.py              # 将 sources.yaml 及分类文件合并为单一 JSON 索引
│   ├── check_health.py             # 并发检查索引中所有链接的 HTTP 状态，输出报告
│   ├── export_csv.py               # 将索引数据导出为 CSV 格式
│   ├── export_static.py            # 基于 Jinja2 模板生成静态 HTML 导航页
│   └── utils/
│       ├── validators.py           # URL 格式校验、域名黑名单过滤等工具函数
│       └── logger.py               # 统一日志格式与级别控制
├── tests/
│   ├── test_build.py               # 构建流程单元测试
│   ├── test_health.py              # 健康检查模拟测试（不实际请求外网）
│   └── fixtures/                   # 测试用样例数据
│       └── sample_sources.yaml
├── docs/                           # 完整文档目录，包含用户手册与开发者指南
│   ├── quick-start.md
│   ├── schema-reference.md
│   ├── operations/
│   │   ├── health-check-guide.md
│   │   └── deployment-examples.md
│   └── developer-notes.md
├── templates/                      # 静态站点生成所用的 Jinja2 模板文件
│   ├── base.html
│   └── index_page.html
├── dist/                           # 构建输出目录（默认 gitignore，仅保留示例）
│   └── .gitkeep
├── requirements.txt                # 生产环境依赖列表
├── requirements-dev.txt            # 开发与测试额外依赖
├── Makefile                        # 常用任务快捷命令（build, check, clean）
├── CONTRIBUTING.md                 # 贡献指南全文
├── LICENSE                         # MIT 许可证文本
└── README.md                       # 本文件
```

## 贡献指南

1.  **分支准备** 从主分支 `main` 签出新的特性分支，分支命名建议采用 `feature/add-domain-category` 或 `fix/update-broken-link` 格式，确保分支历史清晰可溯。

2.  **修改索引数据** 根据 `docs/schema-reference.md` 中的字段定义，编辑 `data/sources.yaml` 或相应分类文件。新增链接必须附带必要的元数据字段（至少包括 `name`、`description`、`category` 以及 `submitter` 注释）。移除或修改现有链接时，请在注释中说明原因。

3.  **本地验证** 在提交前运行 `make check` 或手动执行 `python scripts/build_index.py` 与 `python scripts/check_health.py`，确保索引构建通过且无新增严重健康异常。若新增链接预期不可达（例如仅限内网访问），需在元数据中显式标记 `internal_only: true` 以绕过健康检查告警。

4.  **提交与推送** 提交信息应遵循约定式提交格式，例如 `feat: add new video source entries` 或 `chore: update health report for Q3`。推送至个人远端分支后，在 GitHub 上发起 Pull Request 至 `main` 分支。

5.  **审核与合并** 项目维护者将检查 PR 中的链接是否违反收录准则（例如包含违法违规内容）、元数据是否完整、格式是否正确。通过审核的 PR 将被合并，合并后自动触发 CI 重建索引并更新静态站点。

## 常见问题

**Q1：如果发现某个收录的链接已经失效，应该如何处理？**

A1：请按照贡献指南中的流程提交一个修改 PR，在对应的 YAML 条目中将 `status` 字段更新为 `inactive`，并在注释中记录检查日期和失效现象（例如返回 404 或连接超时）。如果能够找到可替代的有效链接，欢迎同时提交新链接并标注为 `replacement_for`。项目维护者会定期合并此类更新，并重新生成健康报告。

**Q2：项目是否会存储或缓存任何视频、字幕等多媒体文件？**

A2：不会。Lightning Resource Hub 严格定位为纯文本形式的 URL 引用索引。项目仓库内不包含任何音视频文件、字幕文件或二进制大对象。所有链接仅作为文本记录存在，运行时的健康检查仅发送轻量级 HEAD 或 GET 请求验证可达性，不会下载完整内容。用户使用本项目导出的链接访问外部站点时，所有数据交互均发生在用户端与目标服务器之间。

**Q3：我可以将本项目导出的索引数据用于商业项目吗？**

A3：可以。本项目采用 MIT 许可证，导出的索引数据（即链接及其元数据）本身不包含任何第三方版权内容，因此你可以自由使用、复制、修改和分发这些数据，包括用于商业目的。但请注意，本项目的许可证不覆盖被索引的外部网站的内容、商标或服务条款，你在使用链接数据时应独立评估并遵守各目标站点的使用规定。

## 许可证

MIT License. See the LICENSE file for full text.

> 外链数量: 7 | 生成时间: 2026-08-25 21:59:37
