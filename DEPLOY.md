# 部署指南（给想自己部署的人）

这个工具就是一个**静态 HTML 文件**，没有后端、不联网、不需要构建，
所以放到任何能提供静态文件的地方都能跑。

---

## 方案 0：根本不用部署（最省事）

只是自己用、不需要分享给别人：

1. 下载 [`index.html`](https://github.com/24683456/legado-cache-to-txt/raw/main/index.html)
   （仓库页面点开文件 → 右上角 **Raw** → 另存为）
2. **电脑上**：双击打开，把压缩包拖进虚线框即可
3. **手机上**：把文件传到手机，用文件管理器「打开方式 → Chrome」

因为整页不引用任何外部资源，断网也能用。

---

## 方案 1：Fork 后开启 Pages（推荐，约 30 秒）

1. 打开 https://github.com/24683456/legado-cache-to-txt ，点右上角 **Fork** → **Create fork**
2. 进**你自己**的这个仓库 → **Settings** → 左侧菜单 **Pages**
3. **Source** 选 `Deploy from a branch`；**Branch** 选 `main`，目录选 `/(root)` → **Save**
4. 等 1~2 分钟，访问 `https://<你的用户名>.github.io/legado-cache-to-txt/`

> ⚠️ Fork 出来的仓库，Pages 默认是**关闭**的，必须自己去 Settings 里开一次。

---

## 方案 2：不用 Fork，自己新建仓库

1. 打开 https://github.com/new ，仓库名随意（例如 `legado-cache-to-txt`），
   可见性选 **Public**（免费账号只有公开仓库能开 Pages），点 **Create repository**
2. 往仓库**根目录**放这几个文件（Add file → Upload files，或直接拖拽上传）：

   | 文件 | 必要性 | 说明 |
   |---|---|---|
   | `index.html` | **必须** | 工具本体，唯一必需的文件 |
   | `.nojekyll` | 建议 | 空文件，跳过 Jekyll 处理，避免下划线开头的文件被忽略 |
   | `README.md` | 可选 | 仓库说明 |

3. **Settings** → **Pages** → **Source** 选 `Deploy from a branch` →
   **Branch** 选 `main`、目录选 `/(root)` → **Save**
4. 等 1~2 分钟，访问 `https://<你的用户名>.github.io/<仓库名>/`

---

## 方案 3：塞进你已有的个人主页仓库

如果你已经有 `<你的用户名>.github.io` 这个仓库：

1. 在里面新建一个文件夹（比如 `legado/`），把 `index.html` 放进去
2. 推送后访问 `https://<你的用户名>.github.io/legado/`
   （文件夹里的 `index.html` 会自动成为该路径的首页）
3. 直接放在根目录也行，那就是 `https://<你的用户名>.github.io/`

---

## 方案 4：其它静态托管

它就是个普通静态文件，随便挑一个平台：

| 平台 | 做法 |
|---|---|
| Cloudflare Pages | 连接仓库；构建命令留空，输出目录填 `/` |
| Netlify / Vercel | 把文件夹拖进后台即可，不用配置构建 |
| Gitee Pages | 建仓库上传后开启 Pages（需实名认证） |
| 对象存储（OSS / COS / 七牛） | 上传后开启「静态网站托管」 |
| 自己的服务器 | 丢进网站根目录，nginx 里 `root /path; index index.html;` |

---

## 方案 5：只在局域网 / 本机用

```bash
# 在 index.html 所在目录执行，然后手机访问 http://<电脑IP>:8000
python -m http.server 8000
```

---

## 常见问题

**访问显示 404？**

- Pages 首次构建要 1~2 分钟，稍等再刷新
- 确认 Source 是 `main` 分支、目录 `/(root)`，并且 `index.html` 在仓库**根目录**
  （不是放在某个子文件夹里）
- 文件名大小写敏感，必须是 `index.html`，不能是 `Index.html`

**为什么仓库里没有 GitHub Actions 工作流？**

用的是**分支部署**，不需要工作流。用 `actions/configure-pages` 自动开启 Pages
需要仓库管理员权限，仓库默认 token 会在这步失败，所以选了更稳妥的分支部署。

**我想改成 Actions 部署？**

Settings → Pages → Source 改成 `GitHub Actions`，然后自己加一个部署工作流即可
（`actions/configure-pages` + `upload-pages-artifact` + `deploy-pages`）。

**浏览器提示不支持 / 点了没反应？**

解压用到 `DecompressionStream`，需要 Chrome 80+ / Edge 80+ / Safari 16.4+。
老浏览器请改用桌面版，或换 Chrome。

**能改造成自己的工具吗？**

可以。单文件、零依赖、无构建步骤，直接改 `index.html` 里的 HTML / CSS / JS 就行
（逻辑都集中在底部 `<script>` 里，有中文注释）。

**要加个开源许可证吗？**

看你自己。想允许别人自由使用 / 修改 / 再发布，通常加一个 `LICENSE`（如 MIT）；
不加则默认「保留所有权利」。本仓库未附许可证，请按原作者的意思来。

---

## 免责声明

本工具只做**本地格式转换**，不提供、不下载、不分发任何小说内容。
部署和使用时请仅处理你自己有权处理的文件。
