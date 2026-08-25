# Dirlink Navigator

Dirlink Navigator 是一个面向中文互联网内容聚合与技术资源检索的开源导航工具集。项目定位为轻量级、可自托管的资源外链汇聚平台，旨在帮助开发者、内容创作者与技术爱好者快速定位特定类型的在线媒体资源、语言学习材料及影视存档站点，解决信息分散、检索效率低下与资源失效频繁的问题。

项目核心设计理念为“最小化依赖，最大化可移植性”。通过静态页面生成与纯前端路由方案，Dirlink Navigator 无需数据库或后端服务即可运行于任何支持 HTTP/HTTPS 的 Web 服务器或 CDN 之上。其内置的链接健康度检查模块与定期更新机制，可显著降低维护成本，确保资源列表的时效性与可用性。适用于个人书签管理、团队知识共享、开源文档附属资源索引等场景。

## 功能概览

- **多级分类导航体系**：支持用户自定义分类层级，默认内置影视、语言学习、工具站、开发文档等十余个一级类目，每个类目下可无限级分子类目，满足精细化管理需求。
- **链接生命周期监控**：周期性对收录的所有外链进行 HTTP 状态码探测，自动标记失效链接（4xx/5xx）并生成报告，支持手动重试或批量移除。
- **全文模糊检索**：基于倒排索引实现标题、描述、标签及 URL 关键词的快速检索，检索结果按相关性排序，并高亮匹配片段。
- **OPML 导入与导出**：兼容主流 RSS 阅读器及书签管理工具的 OPML 格式，支持批量导入现有收藏夹数据，亦可一键导出全量链接供备份或迁移。
- **暗色主题与阅读模式**：内置亮色/暗色双主题切换，阅读模式可过滤页面广告与干扰元素，聚焦核心内容展示，提升浏览体验。
- **自定义元数据扩展**：每条链接允许附加自定义元数据字段（如维护人、所属项目、更新周期、镜像地址），字段类型支持文本、日期、下拉选项，适配企业级知识库需求。
- **访问统计看板**：基于本地存储或可选的后端日志聚合，展示链接点击频次、热门类目排行、时段分布等轻量级统计数据，辅助优化导航结构。

## 应用场景

- **语言学习者资源聚合**：教师或学习者可将分散于不同站点的中文听力材料、字幕文件、影视对话片段统一收录至导航中，按难度等级或主题分类，快速定位所需练习素材。
- **开源项目文档附属索引**：开源社区维护者可在项目 Wiki 或 README 中嵌入 Dirlink Navigator，集中列出依赖服务、镜像站、API 测试工具、第三方插件仓库等外部链接，降低新手入门门槛。
- **企业内部工具导航**：中小型团队利用该导航搭建内部常用系统入口（代码仓库、CI/CD 面板、日志平台、数据库管理界面），避免每次输入冗长 IP 或域名，提升日常开发效率。
- **自媒体内容素材库管理**：视频创作者将常用免版权音效、视频素材站、字幕生成工具、压制软件官网等整理为私有导航页，结合检索功能快速查找素材来源，减少创作中断。

## 快速开始

以下步骤适用于 Linux/macOS/WSL 环境，确保系统已安装 Git 与 Node.js 18.x 及以上版本。

```bash
# 克隆项目仓库
git clone https://github.com/dirlink-navigator/dirlink-core.git
cd dirlink-core

# 安装依赖（使用 npm）
npm install

# 构建生产环境静态文件（输出目录为 ./dist）
npm run build

# 启动本地开发预览服务器（默认监听 3000 端口）
npm run serve
```

完成上述步骤后，在浏览器中访问 <code>http://localhost:3000</code> 即可查看导航界面。若需部署至生产环境，将 <code>./dist</code> 目录下的所有文件上传至目标 Web 服务器根目录即可。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Node.js | 18.x 或 20.x LTS | 运行时环境，用于构建工具链与开发服务器 |
| npm | 9.x 及以上 | 包管理器，用于安装项目依赖 |
| Git | 2.30 及以上 | 版本控制，用于克隆仓库及提交更新 |
| 现代浏览器 | Chrome 90+ / Firefox 88+ / Edge 90+ | 支持 ES2020 及 CSS Grid/Flex 布局 |
| 磁盘空间 | 至少 200 MB | 包含源码、依赖包及构建产物 |
| 内存 | 开发时建议 4 GB 以上 | 构建过程涉及大量文件读写与转译操作 |
| 操作系统 | Linux / macOS / Windows (WSL2) | 跨平台兼容，Windows 原生环境需配置 MSVC 构建工具链 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|-----------|
| 用户手册 | <code>/docs/user-guide/</code> | 如何添加链接、创建分类、导入导出数据、切换主题及使用检索功能 |
| 开发者指南 | <code>/docs/developer-guide/</code> | 如何二次开发插件、扩展元数据字段、替换图标库、修改构建配置 |
| API 参考 | <code>/docs/api-reference/</code> | 前端存储接口、链接探测服务接口、OPML 解析器函数签名及返回值说明 |
| 运维部署 | <code>/docs/deployment/</code> | 如何部署至 Nginx、Apache、S3 静态托管、Cloudflare Pages 及 Docker 容器 |
| 设计规范 | <code>/docs/design-system/</code> | 色彩变量、排版尺度、组件状态、可访问性标注及响应式断点准则 |
| 贡献章程 | <code>/CONTRIBUTING.md</code> | 提交 PR 前的签署协议、代码风格检查规则、测试覆盖率要求与评审流程 |

## 资源列表

影视与在线观看类

- <code>zhongwenzaixiangaojinghaokanw.org.cn</code>
- <code>rihanzhongwenzimuw.org.cn</code>
- <code>zhongwenzimubofangw.org.cn</code>
- <code>mianfeikanjuwangzhanw.org.cn</code>
- <code>renqizaixianguankanw.org.cn</code>
- <code>zhongwenzimushipinw.org.cn</code>
- <code>zaixianzumumianfeigaoqingw.org.cn</code>

## 项目结构

```
dirlink-core/
├── build/                           # 构建脚本与 Webpack 配置片段
│   ├── webpack.common.js            # 公共配置，包含 entry/output/resolve
│   ├── webpack.dev.js               # 开发环境配置，启用热更新与 source-map
│   └── webpack.prod.js              # 生产环境配置，启用压缩与树摇
├── src/                             # 源码主目录
│   ├── assets/                      # 静态资源（图标、字体、占位图）
│   │   ├── icons/                   # SVG 图标库，按需引入
│   │   └── styles/                  # 全局样式变量与 CSS reset
│   ├── components/                  # 可复用 UI 组件（Vue/React 风格）
│   │   ├── LinkCard/                # 链接卡片组件，含状态徽标与操作按钮
│   │   ├── CategoryTree/            # 分类树递归渲染组件
│   │   └── SearchBar/               # 检索输入框与结果下拉面板
│   ├── core/                        # 核心逻辑模块
│   │   ├── storage.js               # localStorage 封装，含版本迁移与压缩
│   │   ├── checker.js               # 链接健康度探测调度器，支持并发限制
│   │   └── parser.js                # OPML 解析与序列化工具
│   ├── hooks/                       # 自定义 React Hooks / Vue Composition 函数
│   │   ├── useLinks.js              # 链接增删改查与过滤逻辑
│   │   └── useTheme.js              # 主题切换与系统偏好监听
│   ├── pages/                       # 页面级组件（首页、分类页、看板）
│   ├── utils/                       # 通用工具函数（日期格式化、URL 标准化）
│   └── index.js                     # 应用入口，挂载根组件
├── public/                          # 不经过构建直接复制的静态文件
│   └── index.html                   # 主页面模板，含 meta 与预加载提示
├── tests/                           # 单元测试与集成测试
│   ├── unit/                        # Jest 测试用例，覆盖核心逻辑
│   └── e2e/                         # Cypress 端到端测试，模拟用户操作
├── docs/                            # 完整文档源文件（Markdown + Mermaid 图表）
├── .eslintrc.js                     # ESLint 规则，继承 airbnb 与 prettier
├── .prettierrc                      # 代码格式化配置（单引号、尾逗号）
├── package.json                     # 项目元信息、依赖列表与脚本命令
├── README.md                        # 项目总览（即本文档）
└── LICENSE                          # MIT 许可证全文
```

## 贡献指南

1. 阅读项目行为准则与贡献章程，签署开发者原创声明（CLA），确保所提交代码未侵犯第三方权益。
2. 从 Issue 列表中选择标记为 <code>good first issue</code> 或 <code>help wanted</code> 的任务，或创建新 Issue 描述拟解决的问题或新增功能。
3. 派生（Fork）项目仓库至个人空间，基于 <code>main</code> 分支创建功能分支，分支命名遵循 <code>feature/xxx</code> 或 <code>fix/xxx</code> 格式。
4. 本地开发时运行 <code>npm run lint</code> 与 <code>npm run test</code> 确保代码风格合规且现有测试用例全部通过，新增功能需补充对应单元测试。
5. 提交 Pull Request 至 <code>main</code> 分支，描述中关联对应 Issue 编号，并附上变更摘要、截图（如有 UI 改动）及测试结果截图。等待至少两名维护者审核，通过后合并。

## 常见问题

**Q：导航数据存储在哪里？如何备份？**

数据默认存储在浏览器本地 localStorage 中，键名为 <code>dirlink_data</code>。备份可通过界面提供的“导出 OPML”功能生成标准备份文件，或直接在开发者工具中复制该键的值保存为 JSON。亦可通过配置自定义存储适配器，将数据同步至远端 API 或 IndexedDB。

**Q：链接健康度检测会影响性能吗？**

检测任务采用队列化并发控制，默认并发数为 5，且仅在浏览器空闲时段（requestIdleCallback）执行。单次检测超时设置为 10 秒，避免长期阻塞。对于大量链接（超过 1000 条），建议分批检测或关闭自动检测，使用手动触发模式。

**Q：能否支持 Docker 一键部署？**

官方提供预构建 Docker 镜像 <code>dirlink/navigator:latest</code>，基于 Nginx Alpine 镜像，将构建产物打包为静态资源。运行命令为 <code>docker run -d -p 8080:80 dirlink/navigator</code>，镜像内已包含 gzip 压缩与缓存头配置，可直接用于生产环境。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-25 22:00:15
