# 个人博客搭建计划

## Context
搭建一个面向后端开发求职的个人博客，用于展示个人能力、项目经验和技术深度，替代传统简历，给面试官留下更深印象。

## 目标用户
- 技术面试官 / HR
- 技术社区同行
- 潜在的合作者

---

## 技术选型

| 层面 | 选择 | 理由 |
|------|------|------|
| 框架 | **Astro** (v5.x) | 专为内容网站设计，零 JS 默认输出，极快；支持 MDX；可嵌入 React/Vue 组件 |
| 模板 | **AstroPaper** | GitHub 4k+ stars，极简干净，SEO 友好，内置暗色模式、搜索、RSS、站点地图 |
| 样式 | Tailwind CSS | 模板自带，工具类优先，定制方便 |
| 内容 | Markdown / MDX | 写文章自然，支持内嵌组件 |
| 评论 | Giscus | 基于 GitHub Discussions，免费、无需后端、隐私友好 |
| 部署 | Vercel | 推代码自动部署，免费额度充足，国内访问尚可 |
| 域名 | 可选自定义域名 | Vercel 自带 `*.vercel.app` 域名 |

---

## 页面结构与模块

```
/
├── 首页           — 头像 + 一句话自我介绍 + 快速入口（文章/项目/技能）
├── /blog          — 技术文章列表，按时间倒序，支持标签筛选
│   └── /blog/[slug] — 文章详情（Markdown 渲染 + Giscus 评论）
├── /projects      — 项目卡片网格，每个项目：名称/描述/技术栈/GitHub 链接/在线 Demo
├── /skills        — 按类别可视化展示技能标签云
├── /about         — 详细自我介绍 + PDF 简历下载按钮
└── /contact       — GitHub/LinkedIn/邮箱 + 可选联系表单
```

### 全局组件
- **导航栏**：站内页面链接 + 暗色模式切换
- **页脚**：版权 + 社交图标（GitHub/LinkedIn/邮箱）
- **SEO**：每页独立 title/description/OG 标签
- **RSS Feed**：自动生成，方便订阅
- **站点搜索**：AstroPaper 自带 Fuzzy Search

---

## 实施步骤

### 第一步：初始化项目
1. 使用 AstroPaper 模板创建项目
2. 修改 `src/config.ts`：站点名、描述、作者信息、社交链接
3. 替换默认头像和 OG 图片
4. 本地 `npm run dev` 确认能跑

### 第二步：定制页面
1. **首页改造**：重写为自我介绍页，放头像 + 简短介绍 + 三个卡片入口（文章/项目/技能）
2. **项目展示页**：创建 `src/pages/projects.astro`，写一个项目数据数组，渲染卡片网格
3. **技能栈页**：创建 `src/pages/skills.astro`，按分类展示技能标签
4. **关于页**：创建 `src/pages/about.astro`，详细自我介绍 + 简历 PDF 下载
5. **联系页**：创建 `src/pages/contact.astro`，展示联系方式

### 第三步：内容创作
1. 写 2-3 篇高质量技术文章（Markdown），放在 `src/content/blog/`
2. 文章主题建议：
   - 你解决过的实际问题（踩坑记录）
   - 某个技术原理的深入分析
   - 项目开发中的架构决策与权衡

### 第四步：配置与优化
1. SEO：确认每页 meta 描述、Open Graph 标签
2. 评论：注册 Giscus，关联 GitHub Discussions，嵌入文章页
3. 性能：Lighthouse 跑分，确保 90+（Astro 默认就能达到）
4. 域名：可选，购买后绑定到 Vercel

### 第五步：部署
1. 代码推到 GitHub 仓库
2. Vercel 连接仓库，自动部署
3. 每次 push 自动更新

---

## 项目目录结构（关键文件）

```
blog/
├── src/
│   ├── config.ts              # 站点配置（标题/描述/社交链接）
│   ├── content/
│   │   └── blog/              # 所有文章 Markdown 文件放这里
│   │       ├── post-1.md
│   │       └── post-2.md
│   ├── pages/
│   │   ├── index.astro        # 首页（自我介绍）
│   │   ├── projects.astro     # 项目展示 ★ 新建
│   │   ├── skills.astro       # 技能栈 ★ 新建
│   │   ├── about.astro        # 关于我 ★ 新建
│   │   ├── contact.astro      # 联系方式 ★ 新建
│   │   └── blog/              # 博客列表和详情
│   │       ├── index.astro
│   │       └── [...slug].astro
│   ├── components/            # 可复用组件
│   ├── layouts/               # 页面布局
│   └── assets/                # 图片/简历 PDF
├── public/                    # 静态资源
│   └── resume.pdf             # 简历文件
└── astro.config.mjs           # Astro 配置
```

---

## 路线图

| 阶段 | 内容 | 预计时间 |
|------|------|----------|
| 1 | 初始化项目 + 配置 + 本地运行 | 30 分钟 |
| 2 | 定制首页和个人页面 | 1-2 小时 |
| 3 | 编写 2-3 篇技术文章 | 视内容而定 |
| 4 | SEO + 评论 + 性能优化 | 1 小时 |
| 5 | 部署到 Vercel | 15 分钟 |

---

## 验证方式

1. `npm run dev` 本地启动，确认所有页面渲染正常
2. `npm run build` 无报错
3. 部署后浏览器访问所有页面，检查：
   - 首页自我介绍显示正确
   - 项目卡片链接可点
   - 文章列表和详情阅读正常
   - 暗色模式切换正常
   - 搜索功能正常
   - 简历 PDF 可下载
   - Giscus 评论可加载
4. Lighthouse 评分 ≥ 90
5. 手机端响应式布局正常

---

## 已知问题

当前网络环境无法访问 GitHub（443 端口超时），导致 AstroPaper 模板无法通过 `npm create astro` 拉取。npm 包安装正常。

解决方案（按优先级）：
1. 配置代理/VPN 使其可访问 GitHub，然后使用模板创建
2. 手动搭建：`npm init` → 安装 astro、tailwind 等依赖 → 手写项目结构
