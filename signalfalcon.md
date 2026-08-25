# ZY-Resource Hub

ZY-Resource Hub 是一个专注于中文多媒体资源索引的开源导航聚合项目，旨在解决中文互联网环境下优质影视、字幕、在线播放等资源分散、检索效率低下的问题。本项目不存储、不分发任何受版权保护的内容，仅作为公开可用网页链接的结构化整理与分类索引，服务于需要快速定位中文影视资源站点的普通用户、内容研究者以及开源社区开发者。

本项目定位于技术资源与外链汇总站，通过人工筛选与社区贡献相结合的方式，维护一份高可用性、分类清晰、更新及时的中文资源导航库。项目本身以静态 Markdown 与 JSON 数据格式呈现，可轻松部署为个人起始页或嵌入其他开源应用中。

## 功能概览

- **站点分类索引** 按资源类型（字幕下载、在线观看、影视论坛等）对收录 URL 进行一级分类，并附带标签系统，支持多维度筛选。
- **可用性健康检查** 内置简易的 HTTP 状态码检测脚本，可定期对收录链接进行存活探测，并在文档中标记异常状态。
- **快速模糊检索** 基于前端静态 JSON 数据的 JavaScript 模糊搜索功能，支持按域名关键词、分类标签进行实时查找。
- **社区提交钩子** 提供标准化的 Issue 模板与 Pull Request 流程，允许社区成员提交新的资源链接或报告失效链接。
- **数据导出兼容** 所有收录数据以 YAML 与 JSON 双格式存储于仓库 `/data` 目录，便于其他应用或脚本二次处理。
- **响应式起始页模版** 附带一份可直接运行的 HTML 起始页模版，将收录链接渲染为卡片布局，适配移动端与桌面端。
- **更新日志聚合** 通过 GitHub Actions 自动生成每月更新摘要，展示新增、移除或状态变更的链接记录。

## 应用场景

- **个人浏览器的起始页替代方案** 用户可将本项目提供的 HTML 模版设置为浏览器新标签页，快速访问常用中文影视资源站点，避免记忆大量域名。
- **内容研究者的采样池** 研究人员可通过本项目的分类数据，快速获取中文在线视频生态的站点样本，用于网络分析或内容可及性研究。
- **开源社区共建导航** 开发者可 Fork 本项目，根据自身地区或语言偏好定制私有导航，并选择是否向主仓库发起合并请求以回馈社区。
- **本地开发环境测试数据源** 前端或爬虫开发者可使用 `/data` 目录下的结构化 JSON 数据作为开发联调或功能演示的模拟数据源。
- **网络运维的链接巡检辅助** 运维人员可基于项目提供的健康检查脚本，定期扫描企业内部收藏夹中的外部资源可用性，本项目数据可作为巡检样本集。

## 快速开始

以下命令将项目克隆至本地，安装基础依赖，并运行内置的静态页面预览服务。

```bash
# 克隆仓库到本地
git clone https://github.com/zy-resource-hub/zy-resource-hub.git
cd zy-resource-hub

# 安装依赖（使用 npm 或 yarn）
npm install

# 运行本地开发预览服务（默认端口 8080）
npm run serve
```

访问控制台输出的本地地址（例如 `http://localhost:8080`）即可查看起始页模版。如需更新数据索引，请编辑 `/data/sources.yml` 文件后执行 `npm run build` 重新生成 JSON。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Node.js | >= 16.0.0 | 用于运行构建脚本、本地预览服务及依赖管理 |
| npm | >= 8.0.0 或 yarn >= 1.22.0 | 包管理器，用于安装项目依赖 |
| Git | >= 2.25.0 | 用于克隆仓库及版本控制操作 |
| 现代浏览器 | 最新版 Chrome / Firefox / Edge | 用于正常预览 HTML 起始页及使用搜索功能 |
| 网络连接 | 稳定访问公网 | 用于健康检查脚本探测外部链接状态（可选功能） |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|-----|------|----------|
| 用户手册 | `/docs/user-guide.md` | 如何使用起始页、如何进行检索、如何自定义卡片布局 |
| 贡献者指南 | `/CONTRIBUTING.md` | 提交新链接的规范、Issue 模板填写要求、PR 流程说明 |
| 数据维护 | `/docs/maintenance.md` | 如何更新 YAML 数据、健康检查脚本的运行参数与解释 |
| 开发者文档 | `/docs/developer-api.md` | 数据 JSON 结构定义、构建工具链的 API 说明及扩展方式 |

## 资源列表

以下为项目当前收录的全部原始资源链接，按类别分组呈现。每个链接均严格按照提供原样列出，未做任何格式修改。

**字幕资源类**

- <code>zuixinzhongwenzimuzaixian.org.cn</code>
- <code>zhongwenzimuzaixianyingyuan.org.cn</code>
- <code>zhongwenzimuzaixiankanpian.org.cn</code>
- <code>mianfeishipinzhongwenzimu.com.cn</code>

**在线观看类**

- <code>zhongwenzaixianguankanshipin.org.cn</code>
- <code>renqizaixianmianfeishipin.org.cn</code>

**综合导航类**

- <code>zaixianzhongwenzimuwangzhan.com.cn</code>

## 项目结构

```
zy-resource-hub/
├── .github/                         # GitHub 工作流与模板
│   ├── ISSUE_TEMPLATE/              # 新链接提交与问题反馈模板
│   └── workflows/                   # 定时健康检查与更新日志生成
├── data/
│   ├── sources.yml                  # 主数据源（YAML 格式，人工维护）
│   ├── sources.json                 # 构建生成的 JSON 数据（供前端使用）
│   └── health-report.json           # 健康检查结果缓存（自动生成）
├── docs/                            # 完整文档目录
│   ├── user-guide.md                # 用户使用手册
│   ├── maintenance.md               # 维护操作指南
│   └── developer-api.md             # 开发者 API 说明
├── src/
│   ├── index.html                   # 起始页主 HTML 文件
│   ├── style.css                    # 响应式卡片布局样式
│   ├── search.js                    # 模糊检索逻辑
│   └── render.js                    # 数据渲染与卡片生成脚本
├── scripts/
│   ├── health-check.js              # 链接存活探测脚本
│   ├── build-json.js                # YAML 转 JSON 构建脚本
│   └── update-log.js                # 更新日志聚合生成脚本
├── test/                            # 单元测试与集成测试
│   ├── data-validate.test.js        # 数据格式校验
│   └── search.test.js               # 检索功能单元测试
├── package.json                     # npm 依赖与脚本配置
├── README.md                        # 项目入口文档（本文件）
└── LICENSE                          # MIT 许可证文件
```

## 贡献指南

1. **阅读贡献者守则** 在提交任何 Issue 或 Pull Request 之前，请务必阅读 `CONTRIBUTING.md` 文件，了解行为准则与代码规范。
2. **提交新资源链接** 通过 GitHub Issue 使用「新链接提交」模板填写完整信息（分类、域名、简要描述），等待维护者审核。
3. **本地验证与自测** 若希望直接提交数据变更，请 Fork 本仓库，编辑 `/data/sources.yml` 文件，并在本地运行 `npm run test` 确保数据格式合规且所有链接可达。
4. **发起 Pull Request** 提交 PR 时请关联对应的 Issue 编号，并在 PR 描述中说明变更动机、测试结果及是否影响现有功能。
5. **参与健康检查维护** 社区成员可定期关注 `/data/health-report.json` 中的异常链接列表，并通过 Issue 报告持续不可用的站点，协助保持导航质量。

## 常见问题

**Q1: 项目是否提供在线播放或下载功能？**

A1: 否。本项目纯粹是一个 URL 索引导航，不托管、不代理、不缓存任何音视频文件或字幕文件。所有链接均指向第三方公开站点，用户访问第三方网站时需遵守该网站的使用条款。

**Q2: 收录链接无法访问怎么办？**

A2: 您可以先自行确认网络环境是否限制该域名。若确认全局网络下该链接持续不可用，请在本项目的 Issues 中提交「链接失效报告」，维护团队会在下一个健康检查周期后核实并从索引中移除或替换。

**Q3: 如何自定义起始页的配色或布局？**

A3: 您可以直接修改 `/src/style.css` 文件中的 CSS 变量（位于文件头部 `:root` 选择器中），调整主色、圆角、间距等样式。若需大幅改动布局，请参考 `/docs/developer-api.md` 中的渲染接口说明。

## 许可证

MIT License。允许自由使用、修改、分发，仅需保留原始版权声明。详见项目根目录下的 `LICENSE` 文件。

> 外链数量: 7 | 生成时间: 2026-08-25 22:00:21
