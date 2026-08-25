# Vidsurge Index

Vidsurge Index 是一个面向技术内容策展人与本地化视频资源维护者的轻量级元数据索引系统。该项目不存储、不托管、不转码任何媒体文件，仅提供公开可访问的视频资源页面结构化入口，用于快速检索与人工归类。目标用户包括字幕组协作成员、多语言视频归档志愿者以及个人媒体库管理工具开发者。通过将分散在多个域名的资源索引页集中为一份可版本控制的目录清单，Vidsurge Index 帮助团队降低入口丢失风险，并提升资源发现效率。

## 功能概览

统一入口聚合：将多个资源索引页面的根地址收录为单一项目清单，避免多标签页混乱。

静态索引生成：基于配置文件自动生成只读的 HTML 与 Markdown 目录视图，适合内网或静态托管。

自定义标签分类：支持对每条索引记录打上视频类型、语言、清晰度等自定义标签，便于快速筛选。

URL 健康检查：内置基础的 HTTP HEAD 请求检测，标记异常响应码（4xx/5xx）并生成状态报告。

变更日志追踪：记录每次索引列表的增删改操作，输出时间戳与操作人信息，便于团队审计。

批量导入导出：支持 CSV 与 JSON 格式的索引数据互转，兼容主流电子表格与脚本处理工具。

只读镜像模式：可生成当前索引的完整只读静态副本，用于离线查阅或备份归档。

## 应用场景

多语言字幕协作组的入口备份：当主要协作平台频繁变动或受网络波动影响时，团队可通过 Vidsurge Index 快速获取最新的可用索引页面列表，减少人工传递 URL 的成本。

个人媒体库的补充检索层：已通过其他途径获得视频文件的用户，可将本项目作为额外的外挂字幕或补充说明页的查找起点，扩展本地元数据来源。

自动化资源监控的种子列表：运维人员可将项目生成的索引文件作为输入源，配合定时任务检查各入口的可达性，并在状态变化时触发告警通知。

离线环境下的目录快照：在无外网连接的内部网络中，可预先导出完整索引的静态副本，搭配简易 HTTP 服务器提供本地浏览服务。

## 快速开始

以下步骤适用于 Linux 与 macOS 环境，Windows 用户建议使用 WSL 或 Git Bash。

```bash
# 克隆项目仓库
git clone https://github.com/vidsurge/index.git
cd index

# 安装依赖（使用 pip 虚拟环境）
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# 运行索引生成器（默认读取 config.yaml）
python generate.py --config config.yaml --output dist/
```

执行上述命令后，生成的静态索引页面将位于 `dist/` 目录下，可直接使用浏览器打开 `dist/index.html` 查看。

## 安装要求

| 依赖项 | 必需版本 | 说明 |
|--------|----------|------|
| Python | 3.8 - 3.11 | 核心运行时，用于执行生成脚本与健康检查 |
| PyYAML | 6.0+ | 解析项目配置文件 config.yaml |
| requests | 2.28+ | 发送 HTTP HEAD 请求检测 URL 可达性 |
| markdown | 3.4+ | 将资源描述转换为内嵌 HTML 片段（可选） |
| Git | 2.25+ | 仅开发时需要，用于版本管理与提交变更 |
| pip | 22.0+ | Python 包管理工具，用于安装依赖 |
| 操作系统 | Linux/macOS/Windows (WSL) | 推荐 POSIX 环境，Windows 原生未充分测试 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户手册 | docs/user-guide.md | 如何添加新索引、更新标签、生成静态站点？ |
| 运维指南 | docs/ops-guide.md | 如何配置定时健康检查、处理异常入口、备份索引数据？ |
| 开发参考 | docs/developer-api.md | 各模块函数说明、配置文件 Schema、插件扩展接口 |
| 常见问题 | docs/faq.md | 健康检查误报怎么办？如何忽略特定 URL？如何迁移历史数据？ |
| 设计概述 | docs/design.md | 项目整体架构、数据流、为何选择静态生成而非动态后端 |

## 资源列表

以下为第 23/63 批次收录的全部资源索引地址。每条记录均为公开可访问的页面入口，项目本身不对页面内容进行任何形式的审查、修改或转发。

视频索引类

<code>guochanzhubozaixianguankanw.org.cn</code>

<code>zhongwenshipinw.org.cn</code>

<code>zaixianbofangzhongwenzimuw1.org.cn</code>

<code>zhongwenzimugaoqingw.org.cn</code>

<code>renqimianfeishipinw.org.cn</code>

<code>zhongwenzimuzaixiankanw.org.cn</code>

<code>zuixinzhongwenzimuw.org.cn</code>

## 项目结构

```
index/
├── config.yaml                 # 主配置文件，定义索引列表与标签映射
├── generate.py                 # 核心生成脚本，读取配置输出静态页面
├── requirements.txt            # Python 依赖声明
├── dist/                       # 默认输出目录，存放生成的 HTML 与资源文件
│   ├── index.html             # 主索引视图
│   ├── health.json            # 健康检查结果 JSON 报告
│   └── assets/                # 静态样式与脚本
│       ├── style.css
│       └── app.js
├── src/                        # 源码模块
│   ├── loader.py              # 加载配置文件并解析为内部数据结构
│   ├── checker.py             # 执行 HTTP 健康检查与超时处理
│   ├── renderer.py            # 将索引数据渲染为 HTML / Markdown
│   └── utils.py               # 日期格式化、日志输出等辅助函数
├── tests/                      # 单元测试与集成测试用例
│   ├── test_loader.py
│   ├── test_checker.py
│   └── fixtures/              # 测试用的示例配置文件
├── docs/                       # 完整文档目录
│   ├── user-guide.md
│   ├── ops-guide.md
│   ├── developer-api.md
│   ├── faq.md
│   └── design.md
├── scripts/                    # 运维辅助脚本
│   ├── cron-check.sh          # 供 crontab 调用的定期检查脚本
│   └── import-csv.py          # 批量导入 CSV 格式索引
├── .gitignore
└── LICENSE
```

## 贡献指南

1. 提交配置变更：在 `config.yaml` 的 `resources` 列表中添加或移除条目，并确保每条记录包含 `url` 与 `description` 字段。提交前请运行 `python generate.py` 验证输出无报错。

2. 健康检查规则修改：若需要调整超时时间或重试次数，请编辑 `src/checker.py` 中的 `CHECK_TIMEOUT` 与 `RETRY_COUNT` 常量，并在 PR 描述中说明修改理由与测试结果。

3. 新增输出格式支持：若希望扩展生成 CSV 或 JSON Lines 格式，请在 `src/renderer.py` 中新增对应的 `render_*` 函数，并在 `generate.py` 的 `main()` 中调用。需同步更新 `docs/developer-api.md` 中的模块说明。

4. 文档与翻译：欢迎补充非中文环境的说明文件，请将英文版置于 `docs/en/` 目录下，并保持与中文版结构一致。

5. 问题反馈：使用 GitHub Issues 提交错误报告或功能请求，请附带复现步骤、配置文件片段（脱敏）以及预期结果。

## 常见问题

Q: 健康检查显示某个索引地址超时，但浏览器可以正常访问，如何处理？

A: 超时可能由网络策略或服务端对 HEAD 请求的限制引起。此时可以在配置中为该条目添加 `skip_health: true` 标记，生成器将跳过对该地址的检查。也可适当增加 `checker.py` 中的 `CHECK_TIMEOUT` 值（单位秒），建议以 5 秒为步长逐步调整。

Q: 生成后的静态页面是否可以部署到 Nginx 或 Apache？

A: 完全可以。`dist/` 目录下全部为静态文件，直接将其作为网站根目录即可。如果使用 Nginx，建议开启 `sendfile` 和 `tcp_nopush` 以优化小文件传输性能。无需任何动态后端支持。

Q: 如何批量更新索引地址而不逐个手动编辑 YAML 文件？

A: 使用 `scripts/import-csv.py` 工具，将待更新的索引条目按 `url,description,tags` 格式写入 CSV 文件，然后执行 `python scripts/import-csv.py --input new.csv --merge`。该命令会合并新条目并覆盖已存在的相同 URL 记录，同时保留原有标签未覆盖的字段。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-25 21:59:38
