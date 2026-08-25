# Terminus Navigator

Terminus Navigator 是一个面向开发人员、技术研究人员与内容聚合者的高密度外链资源导航系统。该项目并非一个传统的软件库或框架，而是一个精心编排的互联网技术资源索引中枢，旨在解决信息分散、优质内容入口隐蔽以及中文字幕与多媒体资源检索效率低下的问题。

项目目标用户包括自动化运维工程师、多媒体内容处理管线的架构师、数据采集与标注团队，以及需要高频访问特定垂直领域视频辅助资源的技术人员。通过结构化的资源分类与状态监控机制，Terminus Navigator 将原本孤立、易失效的第三方链接转化为可维护、可审计、可快速路由的内部知识库入口，显著降低团队在信息检索环节的隐性时间成本。

## 功能概览

- **集中式外链路由表**：提供统一的域名与资源入口管理视图，将分散于各处的视频辅助、字幕数据源整合为单一可访问层，便于团队内部共享与传播。

- **资源可用性健康检查**：内置链接可达性探测逻辑，支持对收录的第三方域名进行周期性连通性测试，并自动标记异常状态，辅助管理员及时剔除或替换失效资源。

- **分类标签与全文检索**：为每个资源条目附加分类标签（如“字幕库”“在线播放”“综合检索”），并支持基于关键词的快速过滤，帮助用户在大量外链中精准定位目标服务。

- **原始数据透传与审计追踪**：保留用户提供的全部原始 URL 列表，并记录每次增删改操作的时间戳与操作者，满足企业内部的变更管理合规要求。

- **轻量级部署与离线镜像**：项目本身无外部数据库依赖，采用静态配置与本地缓存机制，可在内网环境完整运行，并支持将资源列表导出为结构化数据文件用于离线分析。

- **扩展式插件槽位**：预留标准化的资源处理器接口，允许高级用户通过编写简易脚本扩展自定义资源类型的解析、转换与展示逻辑。

- **响应式管理面板**：提供基于 Web 的仪表盘，以表格和状态徽章形式清晰呈现所有资源的当前健康状况、响应延迟和最后验证时间。

## 应用场景

- **多媒体处理管线的辅助数据源治理**：当团队需要批量获取视频文件的配套中文字幕或在线播放测试地址时，可通过 Terminus Navigator 快速检索到已验证的可用域名，避免因硬编码失效链接导致自动化任务中断。

- **技术文档与教程的外部引用规范化**：技术博客、内部培训手册或 API 文档中若需引用第三方视频演示资源，可使用本项目的索引表作为参考锚点，确保所有对外链接均经过初步可用性筛查。

- **数据采集任务的目标种子维护**：对于需要进行大规模网页样本采集或内容分析的研究项目，可将 Terminus Navigator 中收录的域名作为初始种子集，结合自定义爬虫进行纵深数据获取。

- **新员工环境配置与资源熟悉**：新入职的开发或测试人员可通过访问本项目的资源列表，快速了解团队常用的在线工具、测试视频站点及字幕获取渠道，缩短环境熟悉周期。

- **离线环境下的资源映射重建**：在完全隔离的内部网络中，运维人员可依据本项目的导出配置，在代理层或 DNS 层面重建外部资源的路由映射，确保内部应用对外部依赖的兼容性。

## 快速开始

以下步骤适用于 Linux 与 macOS 环境，Windows 用户可借助 WSL 或 Git Bash 执行。

```bash
# 1. 克隆项目仓库至本地
git clone https://github.com/terminus-navigator/navigator.git
cd terminus-navigator

# 2. 安装基础依赖（Python 3.9+ 与 pip）
pip install -r requirements.txt

# 3. 启动本地资源导航服务（默认监听 8080 端口）
python app.py --port 8080
```

启动成功后，使用浏览器访问 `http://localhost:8080` 即可查看资源导航面板。

## 安装要求

| 依赖项 | 必需版本 | 说明 |
|--------|----------|------|
| Python | 3.9 或更高 | 核心运行环境，用于提供 Web 服务与健康检查脚本 |
| pip | 22.0 或更高 | Python 包管理工具，用于安装项目依赖 |
| Flask | 2.2.5 | 轻量级 Web 框架，用于提供仪表盘界面 |
| requests | 2.31.0 | 用于发送 HTTP 探测请求以检查资源可用性 |
| markdown | 3.4.0 | 用于在仪表盘中渲染 Markdown 格式的资源描述 |
| gunicorn | 21.2.0 | 生产环境推荐的 WSGI 服务器（可选） |
| Git | 2.30.0 或更高 | 用于版本克隆与更新（仅开发/部署时必需） |

## 文档导航

| 层面 | 目录位置 | 回答的问题 |
|------|----------|------------|
| 用户手册 | /docs/user-guide.md | 如何查看资源列表、理解状态标识、使用搜索过滤功能 |
| 管理员操作 | /docs/admin-guide.md | 如何新增、禁用或删除资源条目，如何执行批量健康检查 |
| 配置参考 | /docs/config-reference.md | 环境变量、探测超时参数、缓存策略等配置项详解 |
| 扩展开发 | /docs/extension-guide.md | 如何编写自定义资源处理器，如何挂载新的数据源插件 |
| 常见问题 | /docs/faq.md | 涵盖部署异常、资源不可达、性能调优等高频问题 |
| 变更日志 | /CHANGELOG.md | 记录每个版本的特性新增、修复与不兼容变更 |

## 资源列表

### 综合视频辅助服务类

<code>zaixianbofangw.org.cn</code>

<code>zaixianguankanwangyeshipinw.org.cn</code>

<code>zaixianzhongwenzimuw.org.cn</code>

### 字幕资源专项类

<code>zhubofuliw.org.cn</code>

<code>zhongwenzimudianyingw.org.cn</code>

<code>zhongwenzimuwangzhanw.org.cn</code>

<code>zhongwenzimuzaixianmianfeikanw.org.cn</code>

## 项目结构

```
terminus-navigator/
│
├── app.py                         # 应用主入口，初始化 Flask 服务与路由
│
├── requirements.txt               # Python 依赖声明文件
│
├── config/
│   ├── __init__.py                # 配置模块初始化
│   ├── settings.py                # 环境变量、超时阈值、端口等配置项
│   └── resources.yaml             # 核心资源列表存储（YAML 格式，包含用户提供的全部 URL）
│
├── core/
│   ├── __init__.py                # 核心模块初始化
│   ├── checker.py                 # 资源可用性探测引擎，支持并发 HTTP 请求
│   ├── parser.py                  # 资源列表解析器，支持 YAML/JSON 格式
│   └── registry.py                # 资源注册与索引管理，维护分类标签与状态
│
├── web/
│   ├── __init__.py                # Web 模块初始化
│   ├── routes.py                  # 仪表盘路由定义（列表页、详情页、状态页）
│   ├── templates/
│   │   ├── base.html              # 基础页面模板
│   │   ├── index.html             # 资源总览页，展示表格与状态徽章
│   │   └── detail.html            # 单个资源详情页，显示历史探测记录
│   └── static/
│       ├── css/
│       │   └── style.css          # 自定义样式，用于状态颜色与布局
│       └── js/
│           └── dashboard.js       # 前端交互脚本，支持过滤与自动刷新
│
├── scripts/
│   ├── health_check.py            # 独立运行的批量健康检查脚本，可配置 cron
│   └── export_snapshot.py         # 导出当前资源快照为 JSON/CSV 文件
│
├── tests/
│   ├── test_checker.py            # 探测引擎的单元测试
│   ├── test_parser.py             # 解析器的单元测试
│   └── test_registry.py           # 注册管理模块的单元测试
│
├── docs/                          # 完整文档目录（见文档导航章节）
│
├── .gitignore                     # Git 忽略规则
├── LICENSE                        # MIT 许可证文件
└── README.md                      # 本文件
```

## 贡献指南

1.  **问题反馈与建议**：请使用 GitHub Issues 提交您遇到的运行异常、资源链接失效报告或功能改进建议。提交时请附上运行环境、错误日志及复现步骤。

2.  **资源列表更新**：若您希望新增或移除资源条目，请派生本仓库，编辑 `config/resources.yaml` 文件，确保新增条目包含完整的 URL、分类标签及简要说明，然后提交 Pull Request。

3.  **代码贡献**：对于核心逻辑（如探测引擎、解析器）的优化或缺陷修复，请确保编写相应的单元测试覆盖新逻辑，并通过全部现有测试用例。提交前请执行 `pytest tests/` 验证。

4.  **文档完善**：欢迎改进文档中的拼写错误、示例代码或补充缺失的场景说明。文档更新提交可直接发起 Pull Request，无需关联 Issue。

5.  **安全性报告**：若发现与资源列表内容相关的安全风险（如恶意跳转、内容劫持），请通过邮件或私信联系维护团队，请勿公开披露。

## 常见问题

**问：启动服务后，仪表盘显示部分资源为“不可达”状态，应该如何处理？**

答：“不可达”状态通常表示 Terminus Navigator 在配置的超时时间内未能收到目标服务器的正常 HTTP 响应。首先，请确认您的网络环境能够访问公网；其次，部分第三方站点可能针对非浏览器的请求头进行拦截，您可尝试在 `config/settings.py` 中调整 `USER_AGENT` 和 `TIMEOUT` 参数。若资源持续不可达，建议根据实际情况从 `resources.yaml` 中移除或替换该条目。

**问：项目是否支持 HTTPS 访问或反向代理部署？**

答：支持。您可以在生产环境使用 Nginx 或 Apache 作为反向代理，将 HTTPS 请求转发至本服务的 HTTP 端口（默认 8080）。同时，您也可以在 `config/settings.py` 中启用 `PREFERRED_URL_SCHEME = 'https'`，确保仪表盘中生成的资源链接使用 HTTPS 协议。

**问：如何批量导入大量新的资源链接？**

答：项目支持从外部 JSON 或 YAML 文件导入。您可以将新的资源列表按 `config/resources.yaml` 中相同的格式整理为独立文件，然后使用 `scripts/import_resources.py` 脚本执行合并操作（该脚本位于 `scripts/` 目录下，需根据实际需求启用）。注意导入前备份原始配置文件。

## 许可证

MIT License

Copyright (c) 2026 Terminus Navigator Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-08-25 21:59:38
