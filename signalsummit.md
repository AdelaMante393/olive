# ResourceGateway

ResourceGateway 是一个面向技术社区与内容创作者的轻量级外链资源聚合与导航系统。项目定位为“结构化资源入口”，主要帮助开发者、研究员、内容运营者快速定位、分类管理与共享分散在网络各处的垂直领域资源链接。该项目不提供任何资源文件存储或分发服务，仅作为可自托管的链接目录与元信息索引工具，适用于构建个人书签库、团队共享知识库或特定主题的公开导航站点。

项目默认以静态站点模式运行，所有资源数据基于 Markdown 与 YAML Frontmatter 管理，支持通过 GitHub Actions 实现自动化构建与部署。核心设计目标为低维护成本、高可读性、便于版本控制与协作。用户可通过编辑单一数据目录下的 Markdown 文件完成所有资源增删改操作，无需操作数据库或后端面板。项目内置标签系统、分类索引与全文检索功能（基于 Pagefind），确保在数百至数千条链接规模下依然保持快速筛选体验。

ResourceGateway 特别适用于需要定期整理和发布外链汇总的团队或个人，例如开源社区月报、行业资讯周报、技术文档索引、在线工具清单等场景。项目本身不依赖外部 API 或云服务，完全可离线运行，输出为纯静态 HTML，可托管于任意 Web 服务器或对象存储服务。

## 功能概览

- **结构化链接目录管理**：基于文件夹与 Markdown 文件组织资源条目，每个条目支持标题、URL、描述、标签、优先级、过期时间等元数据字段，便于长期维护与批量检查。

- **多维度分类与标签过滤**：内置两级分类体系（大类 / 子类）和自由标签系统，支持组合过滤查询。用户可在页面端通过侧边栏快速筛选特定主题或格式的资源。

- **全静态全文检索**：集成 Pagefind 索引引擎，构建时自动生成检索数据库，支持前端无服务端搜索，查询响应时间低于 200 毫秒，有效覆盖标题、描述、标签与分类文本。

- **资源状态自动标记**：系统每日定时检查链接可达性（通过 HEAD 请求），对超时、返回 4xx/5xx 状态的链接自动添加“异常”标记，并在界面中高亮提示，帮助维护者及时清理失效资源。

- **自定义视图与导出**：支持列表视图与卡片视图切换，卡片视图自动提取 URL 域名并生成 favicon 占位。支持将当前筛选结果导出为 CSV 或 JSON 格式，便于二次处理或备份。

- **多用户协作草稿模式**：支持 GitHub 式 PR 协作流程，非维护者可通过提交 Markdown 文件变更发起资源新增或修改建议，维护者合并后自动触发站点重建。

- **访问统计与热点排序**：基于页面浏览量（需配合简单的前端计数器或 Plausible 等第三方服务）生成热门资源排行榜，辅助内容优先级评估。

## 应用场景

- **技术团队内部知识库导航**：开发团队可将常用的内部文档链接、设计规范、API 参考、日志平台、监控面板等集中收录至 ResourceGateway，部署在内网服务器上，新成员入职时可一键获取所有必要工具入口，减少沟通成本。

- **开源项目社区资源月报**：开源项目维护者可以按月收集社区内产生的优质讨论、博客文章、视频教程、周边工具等外链，通过 ResourceGateway 生成当月的资源汇总页，并嵌入项目官网或 GitHub README 中，提升社区活跃度与信息沉淀效率。

- **在线教育课程外链索引**：教育培训机构或独立讲师可使用本系统为每门课程建立独立的资源目录，收录参考文档、论文链接、在线练习平台、代码仓库等，学生可通过分类和标签快速定位所需补充材料，同时讲师可标记已失效链接并及时更新。

- **行业资讯与政策监控看板**：媒体编辑或政策研究员可构建特定领域（如 AI 监管、数据安全、开源许可证）的每日链接汇总，结合自动状态检查功能，快速发现被下架或迁移的政策原文页面，确保引用资料的准确性与时效性。

## 快速开始

以下步骤适用于在本地开发环境或服务器上快速启动 ResourceGateway 实例。

```bash
# 1. 克隆项目仓库
git clone https://github.com/resourcegateway/resourcegateway.git
cd resourcegateway

# 2. 安装依赖（使用 npm）
npm install

# 3. 构建静态站点并启动本地预览服务
npm run build
npm run serve
```

执行完成后，访问 `http://localhost:8080` 即可看到初始示例资源页面。若需修改资源内容，请编辑 `data/` 目录下的 Markdown 文件，保存后重新运行 `npm run build` 即可刷新站点。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Node.js | >= 18.12 LTS | 运行时环境，用于执行构建脚本与本地服务 |
| npm | >= 8.19 | 包管理器，用于安装项目依赖 |
| Git | >= 2.25 | 版本控制工具，用于克隆仓库和提交变更 |
| Python（可选） | >= 3.9 | 仅当启用链接可达性检查脚本时需要，用于发送 HEAD 请求 |
| 静态 Web 服务器（可选） | 任意 | 生产环境部署时需要，如 Nginx、Caddy、Apache 或对象存储静态托管 |
| 内存 | >= 512 MB | 构建时最大内存占用，建议 1GB 以上以获得较快构建速度 |
| 磁盘空间 | >= 200 MB | 包含依赖、源码与构建产物，建议预留 500MB |

## 文档导航

| 层面 | 目录 / 文档 | 回答的问题 |
|---|---|---|
| 用户使用指南 | `docs/user-guide.md` | 如何添加、编辑、删除资源链接？如何利用分类和标签快速筛选？如何导出筛选结果？ |
| 维护者手册 | `docs/maintainer-guide.md` | 如何配置自动链接检查？如何自定义站点标题、LOGO 和颜色主题？如何迁移现有数据？ |
| 协作流程说明 | `docs/collaboration.md` | 如何通过 GitHub PR 提交资源建议？合并后站点如何自动更新？冲突如何解决？ |
| 部署参考 | `docs/deployment.md` | 支持哪些部署方式（Vercel、Netlify、自建 Nginx、OSS）？环境变量如何配置？如何开启 HTTPS？ |
| API 与扩展 | `docs/api-extension.md` | 是否提供 JSON 数据接口？如何编写自定义过滤器插件？如何集成第三方检索服务？ |

## 资源列表

以下为当前版本预置的外部资源参考条目，按类别分组展示。所有 URL 均以原始格式原样列出。

**视频资源类**

- <code>zhongwenzimuzaixiankanpian.com.cn</code>
- <code>yejianfulishipin.org.cn</code>
- <code>miyouzaixianshipin.net.cn</code>
- <code>yejianfulishipin.net.cn</code>
- <code>zaixianshipinzhongwenzimua.org.cn</code>
- <code>zaixianbofangzhongwenzimua.org.cn</code>

**图片资源类**

- <code>guomotaotu.net.cn</code>

## 项目结构

```
resourcegateway/
├── data/                           # 数据目录，所有资源条目以 Markdown 存放
│   ├── _meta.yaml                  # 全局配置（站点标题、分类定义、标签别名）
│   ├── video/                      # 视频大类子目录
│   │   ├── index.md                # 该分类说明页
│   │   ├── animation/              # 动画子类
│   │   │   ├── entry1.md           # 单个资源条目，含 frontmatter 元数据
│   │   │   └── entry2.md
│   │   └── documentary/            # 纪录片子类
│   │       └── entry3.md
│   ├── image/                      # 图片大类子目录
│   │   ├── index.md
│   │   ├── photography/            # 摄影子类
│   │   │   └── entry4.md
│   │   └── illustration/           # 插画子类
│   │       └── entry5.md
│   └── audio/                      # 音频大类子目录（示例）
│       ├── index.md
│       └── podcast/
│           └── entry6.md
├── src/                            # 源码目录
│   ├── build.js                    # 核心构建脚本，读取 data/ 生成 HTML
│   ├── filters.js                  # 标签与分类过滤逻辑
│   ├── checker.js                  # 链接可达性检查脚本（定时任务调用）
│   └── templates/                  # 页面模板（EJS）
│       ├── index.ejs               # 首页列表模板
│       ├── detail.ejs              # 资源详情页模板
│       └── partials/               # 可复用组件（头部、侧边栏、卡片）
├── public/                         # 静态资源目录（CSS、JS、字体）
│   ├── css/
│   │   └── style.css
│   ├── js/
│   │   ├── search.js               # Pagefind 前端绑定
│   │   └── filter.js               # 客户端过滤交互
│   └── images/
│       └── favicon.ico
├── dist/                           # 构建输出目录（自动生成，不纳入版本控制）
│   ├── index.html
│   ├── detail/
│   └── search/
├── scripts/                        # 辅助运维脚本
│   ├── deploy.sh                   # 一键部署脚本（打包 + 上传至 OSS）
│   └── check-all.sh                # 手动触发全量链接检查
├── docs/                           # 文档目录（参见文档导航）
├── .github/                        # GitHub 工作流配置
│   └── workflows/
│       └── build.yml               # 定时构建与部署流水线
├── package.json                    # npm 项目配置与依赖声明
├── README.md                       # 本文件
└── LICENSE                         # MIT 许可证文件
```

## 贡献指南

1. **Fork 仓库并创建功能分支**：从主仓库 Fork 后，在本地基于 `main` 分支创建新分支，分支命名建议采用 `feature/xxx` 或 `fix/xxx` 格式，确保分支用途清晰。

2. **在 data/ 目录下编辑或新增资源条目**：所有资源条目为 Markdown 文件，头部需包含 YAML Frontmatter，至少填写 `title`、`url`、`category` 三个字段。建议使用 `tags` 字段增加检索维度。修改前请参考 `data/_meta.yaml` 中已有的分类和标签定义，避免冗余。

3. **本地构建验证**：提交变更前，务必在本地运行 `npm run build` 确保无构建错误。建议同时运行 `npm run test:links`（若有）检查新增链接的可达性，避免引入失效资源。

4. **提交变更并发起 Pull Request**：提交信息请遵循约定式提交规范（如 `feat: add new video resource entry`）。PR 描述中请简要说明新增或修改的资源用途及来源，便于维护者审核。

5. **等待维护者审核与合并**：PR 合并后，GitHub Actions 将自动触发生产环境构建。若合并前需要讨论，可在 PR 评论区交流。合并后的资源变更将在 5-10 分钟内反映在正式站点上。

## 常见问题

**Q：ResourceGateway 是否存储或缓存任何外部资源文件（图片、视频、文档等）？**

A：不存储。ResourceGateway 仅记录链接地址及其元数据，所有访问请求最终重定向至原始外部 URL。系统不会下载、转存或代理任何外部资源内容，链接可达性检查仅发送轻量级 HEAD 请求，不获取响应体。用户需遵守外部站点各自的使用条款。

**Q：如何批量导入现有书签或浏览器收藏夹数据？**

A：项目未提供图形化导入界面，但支持通过脚本转换。您可以将浏览器导出的 HTML 书签文件或 CSV 列表放置于 `scripts/import/` 目录下，运行 `node scripts/import/from-bookmark.js --file=bookmarks.html` 即可自动解析并生成对应的 Markdown 条目文件。目前支持 Chrome、Firefox 和 Edge 的书签导出格式。导入后建议人工检查分类映射是否正确。

**Q：链接自动检查发现异常后，系统是否会主动通知维护者？**

A：默认行为仅为标记异常并在前端界面展示。若需邮件或 Webhook 通知，可在 `config/checker.config.js` 中配置 `notification` 字段，支持 SMTP 邮件和 Slack / Discord 通用 Webhook。配置后，每次检查完成（每日 UTC 2:00）会汇总异常链接列表并发送通知。通知内容包含链接 URL、状态码、检查时间，但不包含响应内容。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-25 22:00:13
