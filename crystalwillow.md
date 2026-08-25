# ResourceBridge

ResourceBridge 是一个面向技术内容创作者、本地化团队与数字档案管理者的高质量外链资源聚合与导航系统。项目定位为“技术资源的索引之索引”，不直接托管任何视频、文档或二进制文件，而是通过结构化元数据与可校验的外链，帮助用户在复杂网络环境中快速定位稳定、可访问且内容合规的公共资源入口。ResourceBridge 解决的核心问题是：当目标资源分散于多个域名、存在访问波动或命名模糊时，提供一套可验证、可审计、可自动更新的外链状态追踪方案。

项目采用纯静态架构，所有资源记录以 YAML 前端数据块形式存储于 `/data` 目录，构建时通过 Go 模板引擎生成只读 HTML 与 JSON API。这一设计使其天然适配 CDN 部署、GitOps 工作流与轻量级内网镜像站场景。ResourceBridge 不依赖数据库，不记录用户行为，所有外链状态检测由 GitHub Actions 定时执行，并将结果以 badges 形式嵌入界面，确保透明性与可维护性。

## 功能概览

- **结构化资源目录**：按语种、内容类型、访问协议三个维度对每一枚外链进行标签化分类，支持多级筛选与全文检索。

- **自动可用性探测**：每日 UTC 00:00 与 12:00 两次发起 HEAD 请求，记录响应码、响应时间与 TLS 证书剩余天数，状态变化时触发邮件告警。

- **元数据版本追踪**：每次资源变更（新增、下架、URL 迁移）均生成 Git commit 记录，支持回滚至任意历史版本，满足审计合规需求。

- **批量导入与校验**：支持通过 CSV 或 Markdown 列表批量导入外链，导入时自动去重、规范化协议头（保留原始输入格式）并检查域名黑名单。

- **自定义分类视图**：允许用户按项目批次（如第 3/63 批）、域名后缀（.org.cn / .com）或内容主题（影视字幕 / 在线播放）创建个人仪表板，视图配置保存在浏览器本地存储中。

- **RESTful 元数据接口**：提供 `/api/v1/links` 端点，返回全量资源 JSON 数据，包含 `original_url`、`last_checked`、`status` 字段，便于第三方工具集成。

- **暗色主题与高对比度模式**：前端样式严格遵循 WCAG 2.1 AA 标准，适配低视力与光敏用户群体。

## 应用场景

**技术文档本地化团队**：团队在翻译海外开源项目文档时，常需要引用原始视频教程或字幕辅助文件。ResourceBridge 的外链聚合表可帮助团队集中管理这些引用源，并自动检测源站可用性，避免在正式发布文档时出现死链。

**数字档案长期保存项目**：档案管理员使用 ResourceBridge 定期抓取并记录公共资源外链的状态快照，结合元数据版本追踪功能，可生成访问稳定性报告，为资源迁移决策提供数据支持。

**个人知识库进阶用户**：深度使用 Obsidian / Logseq 的知识工作者将 ResourceBridge 作为“外链保险库”，所有外部参考链接均先录入 ResourceBridge，再由本地笔记通过 API 调用获取最新状态，从而减少笔记库中的失效链接数量。

**教育机构资源导航站**：学校图书馆或计算中心可利用 ResourceBridge 快速搭建面向师生的中文多媒体资源导航页，将分散的合法播放与字幕站点统一归类，并提供实时可用性标识，提升资源检索效率。

## 快速开始

以下步骤适用于 Linux / macOS / Windows WSL2 环境，需预先安装 Git 和 Go 1.21+。

```bash
# 1. 克隆代码仓库
git clone https://github.com/resourcebridge/resourcebridge.git
cd resourcebridge

# 2. 安装依赖工具（包含静态分析工具 golangci-lint）
make setup

# 3. 构建静态站点与 API 服务
make build

# 4. 启动本地开发服务器（默认监听 :8080）
./bin/resourcebridge -port 8080 -data ./data -output ./dist

# 5. （可选）执行一次性外链状态检测
./bin/resourcebridge check -timeout 5s -concurrency 10
```

构建完成后，访问 `http://localhost:8080` 即可预览资源导航界面。若需生成静态文件以部署至 Nginx，执行 `make static` 后，将 `/dist` 目录整体上传至服务器。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Go 工具链 | 1.21.0 或更高 | 用于编译核心服务与 API 解析器，推荐使用官方二进制安装 |
| GNU Make | 4.0 或更高 | 用于执行构建脚本、测试套件和代码格式化任务 |
| Git | 2.30.0 或更高 | 用于版本控制、元数据提交历史追溯及自动部署钩子 |
| yq (Go 版本) | 4.35.0 或更高 | 用于在 CI 流水线中解析和校验 YAML 元数据文件 |
| curl / wget | 最新稳定版 | 用于本地开发时手动测试 API 端点及调试网络探测模块 |
| Node.js（仅前端开发） | 18.17.0 或更高 | 若需修改 CSS / JavaScript 资源，需安装以运行 PostCSS 与 ESBuild |

## 文档导航

| 层面 | 目录 / 文档 | 回答的问题 |
|---|---|---|
| 用户手册 | `/docs/user-guide/interface.md` | 如何使用搜索、过滤、自定义视图与批量导出功能？ |
| 管理员手册 | `/docs/admin/deployment.md` | 如何配置生产环境反向代理、SSL 证书与定时检测任务？ |
| 元数据规范 | `/docs/specs/metadata-schema-v2.md` | 每个 YAML 字段的含义、合法值及扩展约定是什么？ |
| 开发指南 | `/docs/development/architecture.md` | 项目分层设计、依赖注入方式与新增探测协议的方法 |
| API 参考 | `/docs/api/endpoints.md` | 所有公开接口的请求示例、返回字段与错误码定义 |
| 故障排查 | `/docs/troubleshooting/common-issues.md` | 探测超时、证书错误或构建失败时的常见处理步骤 |

## 资源列表

### 主分类：在线影视与字幕资源

<code>zaixianbofang2.org.cn</code>

<code>zhubofuli.org.cn</code>

<code>zhongwenzimudianying.org.cn</code>

<code>zhongwenzimuwangzhan.org.cn</code>

<code>zhongwenzimuzaixianmianfeikan2.org.cn</code>

### 主分类：在线播放与网页视频

<code>zaixianguankanwangyeshipin2.org.cn</code>

<code>zaixianzhongwenzimu.org.cn</code>

## 项目结构

```text
.
├── cmd
│   ├── resourcebridge          # 主入口二进制包，含 server/check/static 子命令
│   └── migration               # 历史元数据格式迁移工具，支持 v1->v2 增量升级
├── internal
│   ├── checker                 # 外链可用性探测核心逻辑，包含重试策略与超时控制
│   ├── parser                  # YAML 前端数据块解析器，支持多文档拆分与校验
│   ├── api                     # RESTful API 路由注册与中间件（日志、CORS、限流）
│   ├── render                  # Go 模板渲染引擎，含原子化组件与布局缓存
│   └── store                   # 内存缓存层与文件系统抽象接口，用于元数据热加载
├── data
│   ├── batch_003               # 第 3/63 批次资源 YAML 文件，按批次隔离便于审核
│   ├── tags                    # 标签别名与分类层级定义，支持多语言显示名称
│   └── allowlist               # 域名白名单与风险等级标记，用于探测结果评分
├── web
│   ├── static                  # 最终发布的 CSS、JavaScript 与字体文件（经压缩混淆）
│   ├── templates               # 基础布局、列表视图、详情卡片等可复用模板
│   └── assets                  # 源 SCSS 与 ES6 模块源文件，由构建流水线处理
├── scripts
│   ├── ci-check.sh             # GitHub Actions 中执行的完整检测流水线脚本
│   └── import-csv.sh           # 将外部 CSV 转换为内部 YAML 格式的辅助工具
├── test
│   ├── fixture                 # 模拟元数据样本与预期 HTML 输出快照
│   └── integration             # API 端到端测试用例，基于 httpexpect 框架
├── go.mod                      # Go 模块依赖声明（仅包含标准库与最少第三方库）
├── go.sum                      # 依赖校验哈希，确保构建一致性
└── Makefile                    # 统一构建入口，包含 setup/build/test/static/check 目标
```

## 贡献指南

1. 在 GitHub Issue 中认领或创建您希望解决的问题或功能建议，等待维护者添加 `accepted` 标签后再开始开发，避免重复劳动。

2. 派生（Fork）本项目至您的个人账户，并在本地新建一个描述性的分支名称，例如 `feat/batch-import-csv` 或 `fix/checker-timeout`。

3. 编写代码或修改元数据时，请严格遵守 `.golangci.yml` 中的静态检查规则，并运行 `make test` 确保所有单元测试与集成测试通过。对于新增的外链资源，需在 `/data/batch_003` 下补充对应的 YAML 条目，且必须包含 `original_url`、`source_batch`、`content_type` 三个字段。

4. 提交前执行 `make fmt` 自动格式化代码与元数据文件，并确保您的 commit message 遵循约定式提交规范（如 `feat: add HEAD request retry logic` 或 `docs: update metadata schema link`）。

5. 发起 Pull Request 至主仓库的 `main` 分支，PR 描述中请链接相关 Issue，并附上本地运行 `make check` 的完整日志截图。至少需要一位维护者批准后，CI 流水线全部通过方可合并。

## 常见问题

**Q：为什么某些外链在探测结果中显示为“无法访问”，但浏览器中手动打开正常？**

A：ResourceBridge 的探测模块默认使用 `HEAD` 方法且不携带浏览器 User-Agent 头，部分源站会对非浏览器请求返回 403 或 429。您可以登录管理后台将该域名加入“忽略 User-Agent 检测”白名单，或调整探测策略为 `GET` 方法（消耗更多流量，但更接近真实用户访问）。该配置位于 `/data/allowlist/ua_exceptions.yaml`。

**Q：如何迁移已录入的资源到新的批次编号？**

A：请使用 `cmd/migration` 工具，执行 `./bin/migration move --from batch_003 --to batch_004 --ids id1,id2,id3`。该命令会自动更新 YAML 文件中的 `batch` 字段并生成一条审计 commit。移动后，旧批次中会保留一个符号链接文件指向新位置，保证历史 API 请求不发生 404。

**Q：前端搜索功能无法匹配到带有中文括号或特殊符号的链接，如何处理？**

A：这是因为默认的分词器仅支持英文与数字。您需要在 `web/assets/search-index.js` 中启用 `pinyin` 插件选项，并重新运行 `make static`。该插件会将中文标题转换为拼音首字母索引，同时保留原始字符串用于精确匹配。详细配置请参阅 `/docs/user-guide/search-tuning.md`。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-25 21:59:37
