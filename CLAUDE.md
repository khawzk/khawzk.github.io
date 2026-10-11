# 作品集网站维护指南

## 定位
- 我是 Mark Khaw,GenAI solution architect,常驻吉隆坡
- 目标读者:新加坡 AI Solution Architect、FDE、应用 AI 岗的招聘方和用人经理
- 网站要在 30 秒内传达三件事:我是谁、我在 GenAI 做出过商业结果、我能动手做东西
- 核心主张:I turn model capability into numbers a customer can sign off on

## 硬规则(任何改动都不能违反)
- 不使用阿里云或任何前雇主、现雇主的 logo、品牌和内部名称
  (例如 "Telesales MYS"),项目一律用中性名称
  - 例外:经历、证书、做过的平台里写雇主和证书的真实名称(纯文字,如 "Alibaba Cloud")是事实,可以写
- 不公开在职公司的客户名称、具体消耗金额和内部业务数字,只用模糊说法
- 案例内容必须与事实一致:越南客户案例写 "cloned to a second region",
  两套环境、两朵云,不写具体是哪个地区。网站和 cv.pdf 都不出现 Hong Kong、Taiwan
- 任何要求输入 API key 的 demo,必须注明是个人项目(Personal project)、
  与所调用模型的提供方无关,以及 "Key stays in your browser"。
  不要用 "vendor" 这个说法;ModelStudio 现在的写法
  "Personal project, not affiliated with Alibaba Cloud. Your key stays in your browser…" 符合要求
- 不在页面代码里放任何 API key 或密钥
- 不确定的事实不要自己编,先问我

## 项目结构

纯静态站,没有 npm、没有打包步骤。GitHub Pages 自带的 Jekyll 负责构建(见"部署")。

| 路径 | 作用 |
| --- | --- |
| `index.html` | 首页。全部 CSS 和 JS 都内联在这一个文件里 |
| `cv.pdf` | "Download CV" 下载的简历(公开版,不含电话号码和内部数字) |
| `blog/index.html` | 笔记列表页 `/blog/`,带 All / English / 中文 筛选 |
| `_posts/` | 笔记文章,Markdown,文件名 `YYYY-MM-DD-英文短名.md`(中文版加 `-zh`) |
| `_layouts/default.html` | 笔记页面的外壳:导航、页脚、meta |
| `_layouts/post.html` | 单篇笔记:日期、标签、正文、"Read in English / 阅读中文版" 切换 |
| `_data/i18n.yml` | 笔记页面的中英界面文字和日期格式 |
| `assets/blog.css` | 笔记页面样式,颜色和字体变量与 `index.html` 相同 |
| `_config.yml` | Jekyll 配置:文章网址 `/blog/:year/:month/:title/`、RSS 插件、文章默认值 |
| `README.md` | 写笔记和本地预览的说明(构建时排除,不会发布) |

### 首页 `index.html` 各区块(从上到下)

1. **首屏 `#top`**:小标题 + 一句主张 + 展示台轮播 `.stage`。目前 3 页:
   1. 按可用产出给视频生成定价(方法拼贴)
   2. AI 模型网关(AISmallRouter)
   3. 新加坡技能雷达:读取 `/SGJobRadar/data/latest.json` 的实时数据;读不到时显示脚本里写死的快照(目前是 2026-10-05 那周)
   - 加一页 = 复制一个 `<div class="slide" data-title="…" hidden>`。底部圆点和"下一页"按钮由脚本根据页数自动生成
2. **平台和地区 `.trust`**:做过的平台(纯文字,不放 logo);客户地区为马来西亚、印尼、越南、泰国、日本、韩国,只写名字,不用国旗
3. **数字条 `.stats`**:4 个数字
4. **Field work `#work`**:2 个客户案例卡片
5. **Things I build `#builds`**:作品列表。已发布的用 `<a class="row">` 加箭头;进行中的用 `<div class="row">`,不放链接
6. **The path so far `#log`**:
   - 时间线图 `.tl`:工作和学历两条轨道,颜色代表方向(数据 `#8C867B` → 云 `#5E7A5A` → GenAI `#B0623F`)。位置是按 2019-01 到 2026-12 共 96 个月算的百分比,写死在 HTML 里
   - 下面是经历文字列表 `.logrow` 和证书卡片 `.cert`
   - 注意:"Now" 竖线目前固定在 2026 年 11 月,坐标轴只到 2026 年。进入 2027 年前要把范围和所有百分比一起重算
7. **Notes `#notes`**:自动列出最新 3 篇笔记。这是首页唯一用到 Jekyll 模板(Liquid)的地方,所以 `index.html` 开头有一段空的 front matter(`layout: null`)
   - 因此在 `index.html` 的 CSS/JS 里不要写 `{{` 或 `{%`,否则会被 Jekyll 当成模板处理
8. **页脚 `#contact`**:邮箱、LinkedIn、GitHub

### 笔记(博客)

- 文章 front matter:`title`、`lang`(`en` 或 `zh`,默认 `en`)、`description`、`tags`,可选 `ref`
- 中英两版填同一个 `ref`,文章页会自动出现互相跳转的按钮
- RSS 在 `/blog/feed.xml`(jekyll-feed 插件生成)

### 相关子项目(各自是独立仓库,和本站共用 khawzk.github.io 域名)

| 子项目 | 网址 | 部署方式 | 本站怎么用 |
| --- | --- | --- | --- |
| [ModelStudio](https://github.com/khawzk/ModelStudio) | `/ModelStudio/` | 推送到 main 时,Actions(`pages.yml`)发布 `docs/` 目录 | Builds 列表第 1 行,名为 "Multimodal model console (demo)"。链接指向 GitHub 代码,不指向需要输入 key 的线上页面 |
| [SGJobRadar](https://github.com/khawzk/SGJobRadar) | `/SGJobRadar/dashboard/` | Actions 每周一 01:00 UTC 抓数据并提交,`deploy-pages.yml` 发布整个仓库 | 展示台第 3 页和 Builds 第 3 行。用到 `latest.json` 的字段:`week_start`、`overview.total_jobs`、`top_skills[].name/job_count`、`growing_skills[].name/growth_rate`。字段改名时页面会退回快照 |
| [AISmallRouter](https://github.com/khawzk/AISmallRouter) | 只有仓库,没有 Pages | — | 展示台第 2 页和 Builds 第 2 行,链接到仓库 |
| [RAG-LLM](https://github.com/khawzk/RAG-LLM)、[WhatZkEat](https://github.com/khawzk/WhatZkEat) | 只有仓库,没有 Pages | — | Builds 第 4、5 行,链接到仓库 |

改这些仓库的名字,或者关掉它们的 Pages,本站对应的链接就会失效。

## 部署

- **方式:** GitHub Pages 的 "Deploy from a branch",来源是 `main` 分支根目录。本仓库没有自己的 Actions workflow,也没有 CNAME,用默认域名 https://khawzk.github.io/
- **构建:** 每次推送到 `main`,GitHub 自动运行 "pages build and deployment",用 Jekyll 3.10(GitHub Pages 内置版本)构建。通常 1 分钟内上线,结果可以在仓库的 Actions 页面查看
  - 有 front matter 的文件(笔记、`blog/index.html`、`index.html`)会经过 Jekyll 处理;`cv.pdf` 原样发布
- **插件:** 只能用 GitHub Pages 白名单里的插件,目前只用 `jekyll-feed`
- **流程:** 在开发分支上改 → 开 PR → 合并到 `main`(到目前为止都用 squash)→ 自动上线
- **本地预览:**
  ```sh
  gem install jekyll -v 3.10.0 && gem install jekyll-feed kramdown-parser-gfm webrick
  jekyll serve
  ```
  在本地预览时,`/SGJobRadar/...` 的数据读不到,展示台第 3 页会显示快照,这是正常的

## 设计规范
- 配色:暖米白底 #F6F3EC,墨色文字 #1C1B19,点缀色陶土 #B0623F,只用在小地方
  - 辅助色:绿色 #5E7A5A,用于 "Shipped / Live" 状态圆点和时间线里的"云"
  - 灰色只用现有的几档(如 #8C867B,也是时间线里的"数据"),不再增加别的颜色
- 字体:Plus Jakarta Sans,全站统一,不混用衬线或等宽字体
  - 例外:笔记里的代码块用等宽字体;中文文字用系统中文字体(Plus Jakarta Sans 没有中文字)
- 风格:极简留白、细线分隔、圆角卡片,动效克制(进场淡入、悬停轻抬、缓慢漂浮)
- 必须支持 prefers-reduced-motion,手机版不能横向滚动
- 首屏是"小标题 + 大展示台",新作品以新的一页加进展示台轮播

## 常见任务怎么做
- 新增作品:展示台加一页(截图或录屏),Builds 列表加一行,状态写 Shipped 或 In progress
- 更新 CV:替换根目录的 cv.pdf,确认网站上的经历、日期和 CV 一致
- 每次改完:检查所有链接能打开、手机宽度排版正常、没有残留占位符

## 工作方式
- 先说你打算改什么、为什么,我同意后再动手
- 只改我要求的范围,不顺手重构或换设计
- 设计方向已经冻结,除非我明确要求,不要提出视觉改版
