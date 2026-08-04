# 90_personal_site — 个人主页 + 博客重建

## 决策记录（本轮对话，2026-07-16）

- **放弃 HugoBlox，换极简手写 Hugo 骨架。** 理由：HugoBlox 的重（go.mod 远程主题模块、package.json/pnpm 前端构建链、CMS/多部署目标配置）来自框架自身的功能层，不是 Hugo 本身的负担；精简配置改变不了这层重量，只有换掉框架才行。
- **新建 GitHub 仓库覆盖旧仓库**，不在 `Camille-vis/my-academic-cv` 上改。DNS 记录不受影响（域名解析层跟仓库无关），只需新仓库里重新启用 Pages + 放 `CNAME` 文件。
- **博客分类用标签（tags），不用固定顶级栏目。** 以后新增方向只需加标签，不用动站点结构。用户原话："组合优于继承"。若以后标签机制证明不便，可迁移到顶级栏目——迁移成本是"挪文件+改列表页模板"量级，不是重来。
- **模块取舍**：
  - 教育经历不单开框，并入首页简介段落文字里；原来的位置留空，作为以后"项目经历"板块的占位（不是教育经历）。
  - Publications（论文列表）暂不做——用户明确表示不擅长/抵触写论文，这个话题以后单独开一个"AI 时代如何写论文"的讨论，不在这里处理。
  - 专利列表替代 publications，粒度"标题+年份"即可。
  - 不做评论区（担心广告 + 更希望用邮箱直接联系）。
  - 访问统计：装 GoatCounter（免费、无需自建服务器、不用 cookie 弹窗、隐私友好），Claude 直接拍板，用户无偏好。
  - Projects 独立展示页：用户不确定用途，本轮不做。
- **姓名展示格式确定为 "LI Chengping"**（首页署名 + 浏览器标签页标题统一用这个）。
- **头像**：先用首字母占位圈（"LC"），用户后续会上传真实照片替换。
- **favicon**：用户有素材，占位实现，格式要求另行告知。
- **博客列表只显示标题 + 日期**，不写摘要（用户原话："又不是论文要写 abstract"）。

## 当前骨架状态

见本目录 `hugo.toml` / `content/` / `layouts/` / `static/` / `.github/workflows/deploy.yml`。所有真实数据（邮箱、GitHub 用户名、教育背景、专利条目、bio 正文）均为占位，标注 `占位` 字样，等用户提供真实信息后替换——不编造任何数据。

## 决策记录（追加，用户已上线调试阶段）

- **页面文案英文化**：Patents / Experience / Blog / Home 等界面标签改成英文，中文只保留用户需要的地方（正文可中英混写）。`hugo.toml` 的 `languageCode` 改成 `en`，`<html lang>` 同步改。
- **"项目经历"占位插槽改成数据驱动**：新增 `data/experience.yaml`，跟 `patents.yaml` 同一套路（role/org/period 字段），首页模板改成 `{{ if .Site.Data.experience }}` 循环渲染，不再是写死的空文字。

## 决策记录（追加，头像 + 首篇博文模板）

- **头像从"CSS 首字母圆圈"改成真实图片文件**：`static/img/avatar.png` 现在是一张占位图（Claude 生成的通用轮廓图形，非真人照片），模板改成 `<img>` 标签指向这个文件。以后拿到真实照片，**直接用同名文件覆盖 `static/img/avatar.png` 就行，不用改任何代码**。
- **新增 `archetypes/blog.md`**：Hugo 的标准机制，运行 `hugo new blog/文件名.md` 会自动照这个模板生成新文章（自动填日期、根据文件名猜标题），比手动复制别的文章文件更规范，以后写新博客用这个命令。

## 决策记录（追加，favicon 补做）

- 之前决策记录里写了"favicon 占位实现"但没真的做，现在补上：`static/favicon.ico`（多尺寸）+ `static/favicon.png`（32×32），都是从占位头像图裁的，`baseof.html` 加了对应 `<link rel="icon">`。**你有真实素材时，把方形图发给我，我按同样流程重新生成正确尺寸的 ico/png 替换这两个文件**，不建议你自己转格式（ico 是多分辨率打包格式，直接改文件名不会真的生效）。

## 已知限制

- **骨架未经过实际 `hugo build` 验证。** Claude 的沙盒环境网络策略挡掉了 github.com release 二进制下载和 Ubuntu 官方 hugo 包镜像（403/连接被拦截），无法在这里装 Hugo 跑一次真实编译。模板是按标准 Hugo 模板语法（`baseof`/`block` 继承模式、`.Site.RegularPages`、front matter 类型）手动核对写的，逻辑上应该没问题，但没有"跑起来看过"这层保证。**用户本机装好 Hugo 后第一件事应该是跑一次 `hugo server -D`，肉眼确认没有报错、页面结构对得上**，如果有模板报错再反馈回来改。

## 决策记录（追加，折叠排版，2026-07-25）

- **新增折叠内容支持**：用户反馈主线外的题外话内容太多，希望能折叠改善阅读体验。方案是原生 HTML `<details>`/`<summary>` 标签（`hugo.toml` 的 `unsafe = true` 已支持，不用加 shortcode 或第三方插件），配合 `static/css/style.css` 里新加的样式（圆角框、summary 加粗、箭头图标随展开/折叠转向）。
- 用法写进了 `编辑指南.md`（"公式/图片/视频"那节新增"折叠内容"小节），演示放在 `content/blog/all-features-template.md`（"十三、折叠内容"）。
- 关键坑：`<summary>` 和正文之间、`</details>` 前都要空一行，不然里面的 Markdown 语法不会被解析。

## 待办（对应 STATUS.md 任务列表）

1. 用户提供真实内容：bio 文本、联系方式、教育背景、专利列表、GitHub 展示用户名。
2. 用户新建空 GitHub 仓库，把本目录内容整体拷贝过去，`git init` + push（Claude 无 GitHub 写权限，这一步需要用户操作，具体命令另行给出）。
3. 新仓库 Settings → Pages 启用 + 确认自定义域名 `lichengping.com`。
4. 用户注册 GoatCounter 账号，把统计脚本里的占位地址换成真实的。
5. favicon 换成用户提供的素材。
6. 讨论第一篇博客的具体主题、题目、大纲（发布前必须过用户逐字确认，见 `CLAUDE.md` 第 5 条）。

## 决策记录（追加，写作方式声明，2026-08-04）

### 为什么加这段声明

用户提出："文章其实都是 Claude 写的，我想有个通用声明表明 AI 的贡献。" 但实际分工不是"全是 AI 写的"——以《当被测物变成了光学器件》那篇为例，用户提供了物理现象、划定了不写解法的边界、否掉了一条评审意见、并且贡献了"把振荡反过来当干涉测厚"这个关键洞察；Claude 做的是推导、数值、行文和对抗式审查。所以声明写成**分工式**，而不是"本文由 AI 撰写"——后者不准确，而且用户以后真自己写一篇时那个声明就变成谎言了。

### "我"指谁——这是最容易搞错的一点

声明里的第一人称**指 LI Chengping，不指 Claude**。理由：网站绑实名、署名是用户，而 Claude 不承担任何后果。责任只能落在能承担它的一方。所以声明的性质是**加责不是免责**：说清 AI 做了什么，然后声明"即便如此我仍然为它背书"。

### 措辞的落点

初稿写的是"其中的数据与出处我逐条核对过"。用户指出他不敢保证每条都核过——这个反馈是对的，而且很重要：**一句自动生成的、关于自己尽职程度的承诺，如果做不到，比不写声明糟得多。** 不写只是信息少，写了没做到是失信，白纸黑字挂在实名站上。

最终措辞把"承诺覆盖率"换成了"描述方法 + 划出边界"，三句话每一句读者都能自己验证：

1. **关键数据都注明了出处** —— 可查证的事实（文末有参数出处表）
2. **数值结论我尽量交叉验证过** —— 描述方法，不承诺覆盖率
3. **但不保证每一条都亲自复核** —— 主动划出边界

用户原稿里的"不敢保证……但可以大概率确信"被替换掉，原因是"不敢"读起来像心虚而非坦诚，"大概率"是在一篇满是真实数字的文章里非定量地使用定量词汇，读者会注意到。用户的两处实质改动（"共同完成"替代"由 AI 完成"、用坦白替代承诺）都保留了。

### 技术实现

- 渲染位置：`layouts/_default/single.html`，在文章标题/日期下方、目录上方。样式 `.ai-note` 在 `static/css/style.css`。
- **默认开启**（不写 front matter 就显示）。这是刻意的：用户说"我在 draft 状态下负责少量删改调整"，默认开启才不会漏掉。
- 单篇覆盖：front matter 写 `aiNote: "自定义文字"`（支持 markdown）；单篇关闭：`aiNote: false`。
- 改默认措辞只需要改 `single.html` 里那一段，全站生效，**不需要逐篇改**。
- `archetypes/blog.md` 里加了注释说明用法，`hugo new` 出来的新文章自带提示。

### 一个必须记住的副作用

**这个改动对已发布的老文章同样生效**——模板是全站共用的。加声明时 FBG 系列已有两篇 `draft: false`，因为该系列在文章开头已经自述过 Claude 的参与，用户决定不重复，所以给 FBG 系列全部 7 个文件（含 `abd_` 前缀和 `fbgx0*` 那几个副本）都加了 `aiNote: false`。

**以后新增模板级的全站改动时，先想一遍老文章会不会被误伤。**

### 顺带修正的一处

`diamond-mpcvd-pyrometer-oscillation` 当时是 `draft: false`（用户为了预览改的），但用户尚未按 CLAUDE.md 第 3 节逐字过题目/摘要/首段。已改回 `draft: true`，等复核后再由用户手动放开。

### 修正（同日）：文字从模板搬进 front matter

**问题**：初版把声明文字写死在 `layouts/_default/single.html` 里，用户打开文章 `.md` 找不到这段话，没法按当初说好的"在 draft 状态下少量删改调整"。这是设计不匹配需求，不是 bug——模板链路本身是通的（确认过没有 `layouts/blog/single.html` 覆盖 `_default`）。

**改法**：
- `archetypes/blog.md` 里加 `aiNote: |` 多行字段，带完整默认文字。`hugo new` 出来的文章自带，草稿阶段直接改。
- `single.html` 保留一段默认措辞作为**回退**：front matter 没有 `aiNote` 字段时才用（老文章走这条路）。
- 判断条件从 `{{ if ne .Params.aiNote false }}` 改成先 `printf "%v"` 转字符串再比较，不依赖 Hugo 对 nil/bool 比较的具体行为——原写法未经真实 Hugo 验证，属于盲写。

**为什么不追求全站统一措辞**：用户的判断是"每篇分工不一样，没必要统一"。这个判断是对的，而且和他拒绝"逐条核对过"那句是同一个理由——**声明的价值全在准确，不准的声明比没有声明糟**。所以措辞副本分散在各篇是这个设计的正常状态，不是需要收敛的技术债。想批量改时写脚本扫 front matter 即可。

**代价（要记住）**：改 `single.html` 里的默认措辞，**不会影响已经带了 `aiNote` 字段的文章**。这是"可逐篇微调"的必然代价。

**沙盒限制复现**：这次仍然装不上 Hugo（npm 全局装 hugo-bin 权限失败、本地装超时），模板改动依然没跑过真实构建。用户本地 `hugo server -D` 是唯一验证途径。

### 声明的排版（`.ai-note` 样式，同日）

用户要求"声明的字体和排版跟正文不一样，因为这是声明不是正文"。给了四个方案，用户指出 C（底色卡片）和 D（上下细线+衬线体）**不矛盾**——C 是容器、D 的实质是字体族，两者正交，可以叠加。这个判断是对的，是选项设计有问题，把正交的维度摆成了互斥选项。

最终 = C 的框 + D 的字体，与正文的差异落在四个维度：

| 维度 | 声明 | 正文 |
|---|---|---|
| 字体族 | 衬线（宋体） | 无衬线（黑体） |
| 底色 | `#f5f0e2` 色块 | 无 |
| 字号 | 13px | 15px |
| 行距 | 1.75 | 1.85 |

**换字体族是关键的一笔**：其余三项只是"同一个声音小声说话"，只有换字体族能让读者不读内容就知道说话的人换了。

几个不能忘的点：

- **中文衬线要写全字体栈**：`Songti SC`(macOS) / `SimSun`(Windows) / `Noto Serif CJK SC`(Linux)，只写 `serif` 在部分系统上不会落到宋体。
- **字号没有往小调**（12.5 → 13px）：宋体笔画比黑体细，同字号观感更小，不补一点整块会发灰。
- **不要用 `border-left`**：初版用的 `border-left: 3px solid #d8ccb4` 跟 `.content blockquote` 的样式一模一样，读者会把声明误认成引用块。这是初版的实际缺陷，改成底色卡片顺带修掉了。
- **和目录框 `.toc` 的区分**：两个框在页面上紧挨着（声明在上、目录在下）。靠底色深浅（`#f5f0e2` vs `#fdfaf2`）+ 字号（13 vs 13.5）+ 字体族拉开。以后动这两处任一的配色，记得同时看另一个。
- 排除了斜体方案：中文没有真正的斜体字形，浏览器靠几何倾斜硬造，小字号下会糊。

位置维持在标题下、目录上（用户确认）。

## 决策记录（追加，IndexNow 接入，2026-08-04）

- **背景**：用户问怎么让搜索引擎及时抓新博客。查证结论——IndexNow 协议只被 Bing/Yandex/Naver/Seznam 采用，**Google 不支持**（试用过又放弃了，2026-02 前后的多个来源确认）。用户明确表示"能覆盖 Bing 就满意了，Google 爬虫够快，不用管"，所以只做 IndexNow，不追加 Google Indexing API（而且那个 API 本来就只对 JobPosting/BroadcastEvent 结构化数据开放，博客用不上，追加了也没用）。
- **实现方式**：
  - key 文件：`static/9b32de599c3545380a49343e51cdf9a6.txt`（32 位随机 hex，Claude 生成，内容就是 key 本身）。这个 key **不是账号凭证**，不需要在任何第三方平台注册——跟 GoatCounter 那种需要用户自己开账号的情况不一样，纯粹是域名归属校验用的随机字符串，所以这次直接由 Claude 生成实装，没有违反"不代管第三方账号"的规矩。
  - `deploy.yml` 新增第三个 job `notify-indexnow`，`needs: deploy`（部署成功之后才跑，跟部署本身解耦）。build job 里新增一步把 `public/sitemap.xml` 存成普通 artifact（`actions/upload-artifact`，区别于原来给 `deploy-pages` 用的 pages-artifact，两者不通用，这是当时选型时踩的一个理解点：pages-artifact 只能被 `deploy-pages` 消费，普通 job 间传文件必须用另一套 artifact 机制）,notify-indexnow job 里 `download-artifact` 取回来，`grep -oP` 抽取所有 `<loc>` 里的 URL,`jq` 拼成 JSON 数组,POST 给 `https://api.indexnow.org/indexnow`。
  - 策略是**每次全量提交 sitemap 里的所有 URL**，不做"只提交本次新增/改动文章"的增量判断——IndexNow 一天一万条配额，个人博客体量提交全量完全跑不满，图简单可靠优先于精细化。
  - `continue-on-error: true` 挂在提交那一步，避免 IndexNow 端网络抖动时把整个 Actions run 标红，误导用户以为网站部署失败了。
- **DNS/Cloudflare**：查过仓库和 notes 里没有任何 Cloudflare 相关配置记录，域名解析层面目前判断是走 GitHub Pages 默认方式，IndexNow 这次不需要额外动 DNS/CDN 配置。如果以后接入 Cloudflare 之类的代理型 DNS，需要回头检查其 Bot 管理规则有没有误伤 Bingbot。
- **文档同步**：`编辑指南.md` 新增"八、搜索引擎收录（IndexNow）"一节，说明这是全自动、用户不需要操作，供以后排查用。

## 决策记录（追加，RSS discovery + 首页订阅按钮，2026-08-04）

- **背景**：Hugo 默认就会给每个 section 生成 RSS（`index.xml`），这个骨架没关掉这个 output kind，所以 `/index.xml`（全站）和 `/blog/index.xml`（博客）其实**一直都在正常生成**，只是没人知道——`baseof.html` 没有声明这个 feed 的存在。用户判断"低频更新恰恰更需要 RSS"（读者不用记着回来看），认同后决定做两件小事，都不涉及新起服务或账号。
- **改动一：discovery 标签**。`layouts/_default/baseof.html` 的 `<head>` 里加了 `{{ with .OutputFormats.Get "RSS" }}<link rel="alternate" ...>{{ end }}`——这是 Hugo 官方推荐写法，`.OutputFormats.Get` 会按当前页面上下文自动选对应的 feed（首页拿到 `/index.xml`，博客列表页/文章页拿到 `/blog/index.xml`），不用手写死链接，也不用为不同页面写不同模板。
- **改动二：首页复古橙色 RSS 按钮**。用户原话是"很多年前看到的 RSS 是这样的"，指的是 2000 年代博客常见的那种纯橙色方块图标。`layouts/index.html` 在 `.contact` 区块加了一个 `<a class="rss-badge">`，内嵌一个小 SVG（标准 feed 图标的雷达波纹路径），链到 `blog/index.xml`。样式 `.rss-badge` 在 `static/css/style.css`，故意用 `#ee6a1a` 这个跟网站主题色（`#bc5e3c` 系）不同的橙色——**这是刻意的怀旧设计，不是配色失误**，以后统一改色调时这个按钮不用跟着改。
- **一个顺手修的小问题**：`.contact` 这个 div 原来整个包在 `{{ if .Params.github }}` 里，因为 `github` 字段目前是空的（占位未填），这个 div 现在整体不渲染——如果 RSS 按钮直接塞进去，会跟着 github 链接一起被隐藏，等于白做。改成先渲染 `.contact` 外壳，`github` 链接单独判断是否显示,RSS 按钮不受影响。**这个改动的副作用**：等用户以后填了真实 GitHub 链接，`.contact` 的展示逻辑不用再改，本来就是对的结构。
- **验证**：还是那句老话——沙盒装不上 Hugo，这次的模板改动（Go template 语法、SVG 内嵌）没跑过真实构建，用户本机 `hugo server -D` 时留意一下首页右下角（contact 区域）有没有正常显示橙色 RSS 按钮，浏览器地址栏有没有出现订阅图标。

## 决策记录（追加，RSS 折叠说明文案，2026-08-04）

- **背景**：RSS 按钮加上之后，用户提出"华语用户很少用 RSS"，想在按钮旁边加一段折叠说明,讲清楚为什么值得订阅。写作过程分两轮：
  1. 第一版角度是"低频更新站更需要 RSS"（避免读者白跑/漏更新），这版是 Claude 对用户说话的语气，用户指出这版需要改写成**对访客说话**的口吻。
  2. 用户自己给了第二版角度——**从推荐算法手里拿回信息筛选权，反信息茧房**,并明确说"我不确定我说的对不对"，主动邀请 Claude 判断。Claude 的判断：框架基本站得住脚，但有一处该抠精度——RSS 消除的是"算法二次筛选排序"这一层，不等于自动保证不被困在茧房里（如果自己只订阅同质信源，一样会形成茧房，只是这次是自己选的）。最终文案把"保证不被困"改成了"拿回筛选权"，保留了用户的核心论点，没有推翻。
- **第三轮加了实操信息**（本次）：用户追问"Thunderbird 这类邮箱客户端支不支持 RSS，还有哪些常见软件/知名站点支持"，Claude 联网核实后确认：
  - 桌面邮箱：Thunderbird 原生支持（且未计划做到移动端）；经典版 Outlook 支持，但**新版 Outlook for Windows 已经砍掉 RSS 功能**；苹果 Mail 自 OS X Mountain Lion（10.8，约 2012 年）起就已移除。
  - 浏览器：Vivaldi 侧边栏内置 RSS 阅读器；Chrome/Safari/当前版 Firefox 都没有原生 RSS。
  - 移动端：iOS/Android 系统自带邮件/新闻 App 都不支持,绕不开装第三方——常见选择 NetNewsWire（iOS，免费开源）、Feeder（Android，免费开源）、Feedly/Inoreader（跨平台）。
  - 知名站点仍支持 RSS：GitHub（仓库 releases/commits）、Reddit（`.rss` 后缀）、YouTube（频道 feed）、Wikipedia、主流新闻机构、大多数 WordPress 站点。
  - 这些查证结果整合进了折叠框正文，用来支撑"真正做事的站点还在支持 RSS，不是过时功能"这句话，不是凭印象写的。
- **最终文案落地位置**：`layouts/index.html`，`<details class="rss-note">`，紧跟在 RSS 按钮所在的 `.contact` 区块之后。样式 `.rss-note` 加在 `static/css/style.css`，复用了正文 `.toc` 那一套折叠框视觉语言（边框、底色、箭头图标随展开转向），保持全站折叠内容的视觉一致性。
- **注意**：这个 `<details>` 是写在 Hugo 模板文件（`layouts/index.html`）里的原生 HTML，不是 Markdown 正文，不依赖 `hugo.toml` 里 `unsafe = true` 那个 goldmark 配置——那个配置只影响 `content/` 下经过 Markdown 渲染的内容，模板文件本身输出的 HTML 从来不需要这个开关。
