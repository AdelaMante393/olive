# NovaLink 技术资源导航站

NovaLink 是一个面向开发人员、技术研究人员及内容创作者的轻量级外链资源聚合与导航系统。该项目定位于解决个人或团队在浏览、整理和分享高频使用的技术文档、在线工具、视频资源及社区平台时，链接分散、检索效率低、管理成本高的问题。NovaLink 以纯静态站点形式交付，提供分类清晰、响应迅速、可私有化部署的导航页面，同时内置简易的站点可用性监测与访问统计钩子，便于运维人员快速掌握资源健康状态。

目标用户包括：需要维护团队技术文档门户的架构师、经常切换多平台在线工具的开发工程师、从事开源教育的内容创作者，以及希望建立个人知识检索入口的高级技术爱好者。NovaLink 不依赖外部数据库，所有资源条目以结构化 Markdown 或 YAML 文件存储，既保证版本控制友好性，又降低部署与迁移复杂度。项目本身采用 Vue 3 + Vite 构建，前端渲染高效，同时提供完整的 Docker 化交付方案，可实现一键启动。

## 功能概览

- **多级分类导航体系** 支持无限层级的资源分类，用户可自定义类别图标与颜色标识，便于快速定位技术领域（如前端框架、运维监控、学术检索等）。

- **全局模糊检索与标签过滤** 基于资源标题、描述、标签及域名关键词的轻量级全文检索，支持多标签组合筛选，响应时间低于 200 毫秒。

- **链接可用性被动监测** 后端异步任务每 6 小时对已收录链接发起 HEAD 请求，标记超时或非 200 状态码的资源，并在管理界面高亮提示。

- **访问点击统计看板** 记录每个外链的点击次数与最后访问时间，提供基于 LocalStorage 的去重策略，避免单用户重复计数干扰数据真实性。

- **一键导入与导出资源包** 支持通过 JSON 或 CSV 格式批量导入/导出链接数据，便于团队间共享导航配置，或与浏览器书签系统互操作。

- **响应式布局与明暗主题** 适配桌面、平板及移动设备浏览，内置跟随系统主题的明暗模式切换，同时允许用户手动锁定偏好。

- **自定义品牌标识与页脚信息** 允许管理员通过环境变量或配置文件修改站点标题、Logo 链接及页脚版权声明，满足企业内部门户定制需求。

## 应用场景

**技术团队内部文档门户辅助导航** 开发团队通常维护多个内部系统（如 Jenkins、SonarQube、Nexus、GitLab）及外部文档站（如 Kubernetes 官方文档、Spring Boot 参考指南）。NovaLink 可将这些入口统一收纳，并按项目或部门分组，新成员入职时只需访问一个导航页即可获取全部必要工具地址，大幅减少沟通成本。

**开源项目 README 中的资源索引页** 开源项目维护者往往需要在 README 中列出大量依赖库、学习视频、社区论坛和镜像源地址。直接堆砌链接会使 README 冗长且难以维护。NovaLink 可作为独立索引站点部署，项目 README 仅保留主站链接，所有外部资源在 NovaLink 中分类呈现，并支持版本化更新日志，方便用户追溯链接变更历史。

**在线教育或技术培训的课程资料汇总** 讲师在讲授编程或运维课程时，需要向学员提供多组练习环境地址、参考视频链接和习题答案仓库。NovaLink 可为每期班级独立部署实例，允许讲师按课时动态增删链接，学员通过统一入口获取所有材料，且可通过检索功能快速找到特定知识点对应的资源。

**个人技术研究者的知识收藏夹替代方案** 技术研究者日常积累大量博客、论文预印本、在线编译器、数据可视化示例等碎片化链接。传统浏览器书签缺乏分类视图和搜索能力，且跨设备同步受限。NovaLink 可部署在个人云服务器上，配合 Git 仓库实现配置备份，形成长期可维护的个人知识路由表。

## 快速开始

以下步骤适用于开发环境快速启动，生产环境部署请参考文档导航中的部署指南章节。

```bash
# 克隆项目仓库
git clone https://github.com/novalink-dev/novalink-core.git
cd novalink-core

# 安装依赖（使用 npm 或 yarn）
npm install

# 复制环境变量模板并配置必要参数
cp .env.example .env.local

# 启动开发服务器（默认监听 5173 端口）
npm run dev
```

执行完成后，浏览器访问 `http://localhost:5173` 即可预览导航页面。如需构建生产版本并预览，执行 `npm run build` 后使用 `npm run preview` 启动本地静态服务器。若使用 Docker 部署，可执行 `docker-compose up -d`，容器将自动暴露 8080 端口。

## 安装要求

| 依赖项 | 必需版本 | 说明 |
|--------|----------|------|
| Node.js | >= 18.12.0 (LTS) | 构建与开发服务器运行环境，推荐使用 nvm 管理版本 |
| npm | >= 8.19.0 | 依赖管理工具，支持 package-lock.json 一致性安装 |
| Vite | >= 4.0.0 | 构建工具，内部已锁定版本，无需额外安装 |
| Docker (可选) | >= 20.10.0 | 若选择容器化部署，需配合 docker-compose >= 2.0.0 |
| Git | >= 2.30.0 | 克隆仓库及版本控制，用于配置备份和更新合并 |
| 现代浏览器 | 最新两个版本（Chrome/Edge/Firefox/Safari） | 支持 ES Module 和 CSS Grid 布局，IE 11 及以下不支持 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户手册 | `/docs/user-guide/` | 如何添加、编辑、删除链接？如何创建分类和标签？检索语法支持哪些通配符？ |
| 管理员指南 | `/docs/admin-guide/` | 如何配置站点标题与 Logo？如何调整监测间隔与超时阈值？如何手动触发全量链接健康检查？ |
| 部署运维 | `/docs/deployment/` | 支持哪些部署方式（Nginx 静态托管、Docker、Vercel、云对象存储）？如何配置反向代理与 SSL 证书？ |
| 开发贡献 | `/docs/contributing/` | 项目目录结构说明、代码规范、提交信息格式、如何新增内置主题或扩展检索引擎？ |

## 资源列表

以下为 NovaLink 默认收录的部分外部资源，供用户参考或直接使用。所有链接均按原始格式原样列出，未做任何协议或域名修改。

**影视及多语言字幕资源（示例分类）**

- <code>zhongwenzaixiangaojinghaokan.org.cn</code>
- <code>rihanzhongwenzimu2.org.cn</code>
- <code>zhongwenzimubofang.org.cn</code>
- <code>mianfeikanjuwangzhan.org.cn</code>
- <code>renqizaixianguankan.org.cn</code>
- <code>zhongwenzimushipin.org.cn</code>
- <code>zaixianzumumianfeigaoqing.org.cn</code>

## 项目结构

项目采用模块化分层设计，核心代码位于 `src/` 目录，配置与资源文件位于根目录。以下为关键目录与文件说明：

```
novalink-core/
├── public/                          # 静态资源目录，构建时直接复制
│   └── favicon.ico                  # 站点图标
├── src/
│   ├── api/                         # 后端模拟接口及数据请求层（含监测逻辑）
│   │   ├── health.js                # 链接可用性监测模块
│   │   └── stats.js                 # 点击统计写入与读取
│   ├── assets/                      # 样式、图片、字体等静态资源
│   │   ├── themes/                  # 明暗主题变量定义
│   │   └── logo.svg                 # 默认品牌 Logo
│   ├── components/                  # Vue 可复用组件
│   │   ├── NavBar.vue               # 顶部导航与主题切换
│   │   ├── SearchBox.vue            # 检索输入与标签过滤面板
│   │   ├── LinkCard.vue             # 单个链接卡片渲染（含状态徽标）
│   │   └── CategoryTree.vue         # 分类树递归渲染组件
│   ├── config/                      # 站点配置加载与合并
│   │   ├── site.config.js           # 站点标题、描述、默认分类
│   │   └── links.data.js            # 初始外链数据（可被用户替换）
│   ├── composables/                 # Vue Composition API 钩子
│   │   ├── useSearch.js             # 检索与过滤逻辑封装
│   │   └── useTheme.js              # 主题状态管理与持久化
│   ├── layouts/                     # 页面布局容器
│   │   ├── DefaultLayout.vue        # 常规页面布局（含侧边栏）
│   │   └── FullWidthLayout.vue      # 无侧边栏全宽布局（用于登录页）
│   ├── pages/                       # 路由对应页面组件
│   │   ├── HomePage.vue             # 首页导航视图（核心）
│   │   ├── AdminPage.vue            # 链接管理后台（需权限）
│   │   └── AboutPage.vue            # 项目说明与版本信息
│   ├── router/                      # Vue Router 路由配置
│   │   └── index.js
│   ├── store/                       # Pinia 状态管理（分类、链接、用户偏好）
│   │   ├── linkStore.js             # 链接数据 CRUD 及缓存
│   │   └── uiStore.js               # 侧边栏折叠、主题、通知状态
│   ├── utils/                       # 通用工具函数
│   │   ├── domainParser.js          # 从 URL 提取主域名用于展示
│   │   └── debounce.js              # 检索防抖函数
│   ├── App.vue                      # 根组件
│   └── main.js                      # 应用入口（创建 Vue 实例）
├── .env.example                     # 环境变量模板（端口、监测周期等）
├── .eslintrc.cjs                    # ESLint 代码检查规则
├── docker-compose.yml               # 容器编排定义（含 Nginx + 应用）
├── Dockerfile                       # 多阶段构建镜像文件
├── index.html                       # Vite 入口 HTML
├── package.json                     # 依赖清单与脚本定义
├── vite.config.js                   # Vite 构建配置（含别名与代理）
└── README.md                        # 项目主说明文档（即本文档）
```

## 贡献指南

我们欢迎各类贡献，包括但不限于新增分类模板、优化检索算法、修复监测模块的边界条件缺陷以及完善国际化翻译。请遵循以下步骤提交贡献：

1.  **Fork 仓库并创建功能分支** 从 `main` 分支切出以 `feat/`、`fix/` 或 `docs/` 为前缀的分支，例如 `feat/add-nginx-deployment`。避免直接在 `main` 上修改。

2.  **本地开发与自测** 执行 `npm run dev` 启动开发服务器，确保新增功能或修复不影响现有导航渲染和检索逻辑。新增链接数据需包含 `title`、`url`、`category` 和至少一个 `tag` 字段。运行 `npm run test:unit`（若已配置单元测试）验证核心工具函数。

3.  **提交代码并签署开发者证书** 提交信息需符合 Conventional Commits 规范，即 `<type>(<scope>): <subject>` 格式，且提交消息中必须包含 `Signed-off-by: Real Name <email>` 行，以表明您接受 DCO 协议。

4.  **发起 Pull Request 至 `main` 分支** 在 PR 描述中清晰说明改动动机、影响范围以及是否涉及破坏性变更。若新增外部依赖，需在 PR 中阐述理由并更新 `package.json` 及 `pnpm-lock.yaml`（如有）。

5.  **等待代码审查与 CI 通过** 维护者将在一周内进行审查，可能要求补充单元测试或调整 API 设计。CI 流程包含 ESLint 检查、构建测试及容器镜像构建验证，全部通过后方可合并。

## 常见问题

**问：如何迁移现有浏览器书签或收藏夹到 NovaLink？**

NovaLink 未内置直接导入浏览器书签 HTML 文件的功能，但提供两步替代方案：首先使用浏览器书签导出功能生成 HTML 文件，然后借助社区工具（如 `bookmarks-to-json`）将其转换为 JSON 格式，最后通过 NovaLink 管理后台的「导入 JSON」按钮完成批量添加。建议导入后手动补充分类与标签字段以获得更好检索体验。

**问：链接可用性监测对性能的影响如何？是否会导致站点加载变慢？**

监测任务默认部署为独立的异步 Worker 进程，与主应用服务分离。在单机部署模式下，监测周期为 6 小时，每次并发检测上限为 20 个链接，超时设置为 5 秒，对主站 CPU 和内存影响可忽略。用户端页面加载时仅读取监测结果缓存，不触发实时检测，因此前端响应速度不受影响。若需调整监测频率或超时阈值，可修改环境变量 `HEALTH_CHECK_INTERVAL` 和 `HEALTH_CHECK_TIMEOUT`。

**问：NovaLink 支持多用户协作编辑吗？**

当前版本定位为个人或小型团队工具，未内置用户体系与权限管理。多用户场景建议采用「Git + 文件变更」工作流：将链接数据文件（`src/config/links.data.js`）纳入 Git 仓库，不同成员通过 Pull Request 提交变更，由管理员合并后重新构建部署。后续大版本规划中会加入基于 JWT 的轻量级认证和基于角色的编辑权限控制。

## 许可证

MIT License

Copyright (c) 2026 NovaLink Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-08-25 21:59:37
