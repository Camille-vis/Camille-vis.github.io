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

## 已知限制

- **骨架未经过实际 `hugo build` 验证。** Claude 的沙盒环境网络策略挡掉了 github.com release 二进制下载和 Ubuntu 官方 hugo 包镜像（403/连接被拦截），无法在这里装 Hugo 跑一次真实编译。模板是按标准 Hugo 模板语法（`baseof`/`block` 继承模式、`.Site.RegularPages`、front matter 类型）手动核对写的，逻辑上应该没问题，但没有"跑起来看过"这层保证。**用户本机装好 Hugo 后第一件事应该是跑一次 `hugo server -D`，肉眼确认没有报错、页面结构对得上**，如果有模板报错再反馈回来改。

## 待办（对应 STATUS.md 任务列表）

1. 用户提供真实内容：bio 文本、联系方式、教育背景、专利列表、GitHub 展示用户名。
2. 用户新建空 GitHub 仓库，把本目录内容整体拷贝过去，`git init` + push（Claude 无 GitHub 写权限，这一步需要用户操作，具体命令另行给出）。
3. 新仓库 Settings → Pages 启用 + 确认自定义域名 `lichengping.com`。
4. 用户注册 GoatCounter 账号，把统计脚本里的占位地址换成真实的。
5. favicon 换成用户提供的素材。
6. 讨论第一篇博客的具体主题、题目、大纲（发布前必须过用户逐字确认，见 `CLAUDE.md` 第 5 条）。
