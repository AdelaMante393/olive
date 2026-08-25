# NovaIndex

NovaIndex 是一个面向技术内容创作者与开发者的外链资源聚合与导航系统。该项目不提供具体软件或库，而是以结构化、可维护的方式组织分散在网络各处的优质技术资源、工具站与知识库入口，帮助技术团队与独立开发者快速定位所需的外部依赖、文档源与在线工具。NovaIndex 聚焦于解决资源链接失效、入口分散、检索成本高的问题，通过统一索引与分类体系，将碎片化的网络资源转化为可复用、可共享的知识资产。

NovaIndex 的核心受众是运维工程师、全栈开发者、技术决策者以及开源项目的维护者。项目本身不依赖特定编程语言运行环境，所有索引数据以纯文本与 Markdown 格式存储，兼容任何版本控制系统，并支持通过自动化脚本进行链接健康检查与格式校验。项目定位为轻量级、低侵入、高可读性的资源索引层，可嵌入现有文档站点或作为独立导航页部署。

## 功能概览

- **多级分类索引**：按技术领域、使用场景、资源类型建立三级分类体系，每个资源条目均归属至少一个分类标签，便于按主题浏览。

- **链接状态检测**：内置基于 HTTP 状态码的链接可用性检查脚本，可定期输出失效链接报告，辅助维护者及时更新或移除不可用资源。

- **全文检索支持**：资源标题、描述、关键词字段被提取为独立元数据文件，可对接简单的静态站点搜索工具（如 Pagefind 或 Lunr）实现客户端全文检索。

- **资源变更追踪**：每次增删改操作均通过 Pull Request 或提交记录留存变更历史，支持回滚与审计，便于团队协作。

- **外链关系图谱**：自动解析资源页面中的对外链接数量与目标域名分布，生成简单的依赖关系数据，辅助评估资源的外部依赖性。

- **多格式导出**：索引数据可导出为 JSON、YAML 或 CSV 格式，便于导入其他系统或进行二次分析。

- **自定义元数据扩展**：每条资源可附加自定义键值对元数据（如维护人、更新周期、备选域名），满足企业级内部索引需求。

## 应用场景

- **技术团队内部知识库导航**：研发团队可将 NovaIndex 作为内部文档站的首屏导航，集中存放常用 CI/CD 工具链地址、镜像仓库入口、内部 API 文档链接，减少成员在多个标签页间切换的时间损耗。

- **开源项目 README 外链整理**：开源项目维护者使用 NovaIndex 管理项目依赖的外部资源（如协议参考、规范文档、社区论坛），避免在 README 中堆砌大量裸链接，提升文档可维护性。

- **个人开发者的书签替代方案**：独立开发者将 NovaIndex 部署为个人起始页，按项目维度组织日常使用的在线 JSON 格式化工具、正则表达式测试平台、图标库与字体资源站，所有链接以纯文本形式受 Git 版本管理，告别浏览器书签同步失败问题。

- **技术培训与教学资源索引**：培训机构或高校实验室可采用 NovaIndex 整理课程所需的在线实验环境地址、视频资源入口与习题参考链接，学员通过统一页面获取所有外部资源，降低学习初始门槛。

- **社区共建资源导航站**：技术社区可基于 NovaIndex 搭建公开的资源导航，由社区成员通过 Merge Request 贡献新链接，维护者审核后合并，形成持续演进的外部资源精选集。

## 快速开始

以下步骤适用于在本地环境中启动 NovaIndex 索引服务（即静态页面预览与链接检查工具链）。

```bash
# 克隆项目仓库
git clone https://github.com/novaindex/novaindex.git

# 进入项目目录
cd novaindex

# 安装依赖（Python 虚拟环境推荐）
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# 运行链接健康检查脚本（示例）
python scripts/check_links.py --source data/links.json --report reports/

# 构建静态导航页面（输出到 dist 目录）
python scripts/build_static.py --config config.yaml --output dist/

# 启动本地预览服务器
python -m http.server 8080 --directory dist/
```

## 安装要求

| 依赖项目 | 必需版本 | 说明 |
|---------|---------|------|
| Python | 3.8 及以上 | 运行链接检查与构建脚本的核心解释器 |
| pip | 20.0 及以上 | 管理 Python 依赖包 |
| Git | 2.20 及以上 | 克隆仓库与提交变更 |
| requests 库 | 2.25.0 及以上 | 用于发送 HTTP 请求检测链接状态 |
| PyYAML 库 | 5.4.0 及以上 | 解析配置文件与元数据 |
| markdown 库 | 3.3.0 及以上 | 将资源描述渲染为 HTML（可选，用于静态生成） |
| 任意现代浏览器 | 最新稳定版 | 预览生成的静态导航页面 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|-----------|
| 用户手册 | docs/user-guide/ | 如何添加新资源、如何修改分类、如何导出索引数据 |
| 维护者指南 | docs/maintainer/ | 链接检查流程、失效链接处理规范、版本发布策略 |
| 配置参考 | docs/config/ | config.yaml 中每个字段的含义、自定义元数据模板写法 |
| 开发说明 | docs/developer/ | 脚本架构说明、新增导出格式的方法、测试用例编写要求 |
| 设计文档 | docs/design/ | 分类体系设计原则、关系图谱数据结构、静态生成渲染逻辑 |

## 资源列表

### 在线视频与字幕资源

<code>zaixianbofang2.org.cn</code>

<code>zhubofuli.org.cn</code>

<code>zhongwenzimudianying.org.cn</code>

<code>zhongwenzimuwangzhan.org.cn</code>

<code>zhongwenzimuzaixianmianfeikan2.org.cn</code>

<code>zaixianguankanwangyeshipin2.org.cn</code>

<code>zaixianzhongwenzimu.org.cn</code>

## 项目结构

```
novaindex/
│
├── data/                        # 核心索引数据目录
│   ├── links.json               # 主资源链接库（含标题、URL、分类、描述）
│   ├── categories.yaml          # 分类层级定义（一级/二级/三级）
│   └── metadata/                # 每条资源的独立元数据文件（按ID分片）
│       ├── 001.yaml
│       ├── 002.yaml
│       └── ...
│
├── scripts/                     # 工具脚本目录
│   ├── check_links.py           # 并发链接状态检测，输出 CSV 报告
│   ├── build_static.py          # 从 data/ 生成静态 HTML 导航页面
│   ├── export_json.py           # 导出为标准化 JSON 格式
│   └── validate_schema.py       # 校验 links.json 字段完整性
│
├── config/                      # 配置文件目录
│   ├── config.yaml              # 主配置（站点名称、导出路径、检查超时）
│   └── ignore_patterns.txt      # 链接检查时忽略的 URL 正则列表
│
├── docs/                        # 项目文档（面向人类阅读）
│   ├── user-guide/              # 用户操作手册
│   ├── maintainer/              # 维护者操作流程
│   └── developer/               # 二次开发说明
│
├── reports/                     # 链接检查报告输出目录（自动生成，不入库）
│   └── link_report_YYYYMMDD.csv
│
├── dist/                        # 静态站点构建输出目录（自动生成，不入库）
│   ├── index.html
│   └── assets/
│
├── tests/                       # 单元测试与集成测试
│   ├── test_check_links.py
│   └── test_build_static.py
│
├── requirements.txt             # Python 依赖清单
├── Makefile                     # 常用命令快捷方式（make check / make build）
└── README.md                    # 项目概述（即本文档）
```

## 贡献指南

1.  **派生与克隆**：将本仓库派生至个人账户，然后克隆到本地开发环境。请确保使用 SSH 协议克隆以便于后续推送变更。

2.  **创建特性分支**：从 `main` 分支切出新分支，分支命名规范为 `feature/资源分类-简述` 或 `fix/链接失效-资源ID`。禁止直接在 main 分支上修改。

3.  **修改索引数据**：根据 `docs/user-guide/add-resource.md` 中的步骤，在 `data/links.json` 中新增或修改资源条目，并确保所有必填字段（title、url、category、description）完整且符合 `data/schema.json` 定义的格式规范。

4.  **本地验证**：运行 `make check` 执行链接可用性检测与数据格式校验，确保所有新增链接返回 200 状态码或符合忽略规则，且无 JSON 语法错误。运行 `make build` 验证静态页面能否正常生成。

5.  **提交并推送**：编写清晰的提交信息，格式为 `[类型] 简短描述`（例如 `[add] 新增容器镜像加速器资源`）。推送至个人远程分支后，通过平台界面发起 Pull Request 至主仓库的 `main` 分支，等待维护者审核。

## 常见问题

**Q：索引中的链接失效了怎么办？**

A：NovaIndex 的链接检查脚本会每日自动运行（若配置了 CI 流水线）或由维护者手动触发。失效链接会被记录在 `reports/` 目录下的 CSV 报告中。社区贡献者可认领失效链接，通过 Pull Request 更新 URL 或删除该条目。若暂时无法找到替代链接，可在资源描述中添加 `[deprecated]` 标记并保留 30 天，之后将被移除。

**Q：我能否添加非技术类或商业推广性质的链接？**

A：NovaIndex 定位为技术资源导航，原则上不接受纯商业广告、无实质内容或与编程开发无关的链接。但若某商业工具提供免费社区版或开源版本，且其官方文档或下载入口对开发者有实际帮助，可在分类中标记为"商业产品"并添加备注。所有新增链接需经维护者审核，审核标准详见 `docs/maintainer/review-criteria.md`。

**Q：如何批量导入现有书签文件？**

A：项目未内置直接导入浏览器书签的功能，但您可使用 `scripts/import_bookmarks.py` 脚本（需自行适配）将 Netscape 格式的 HTML 书签导出文件转换为符合 `links.json` 结构的 JSON 数据。转换后务必运行 `validate_schema.py` 校验字段完整性，并手动补充分类与描述信息，因为书签文件通常不包含这些字段。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-25 21:59:37
