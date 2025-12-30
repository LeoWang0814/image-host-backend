# GitHub 图床上传器（本地部署版） 🖼️🚀  
**GitHub Image Hosting Uploader (Local Deployment)**

一个 **纯前端、零后端** 的本地上传工具：  
拖拽文件 → 自动生成 **UUIDv4 文件名** → 通过 GitHub Contents API 上传到指定仓库目录。  
上传完成后自动生成 **RAW / Blob / Website** 三种链接，支持并发队列、失败重试和一键复制。

A **pure frontend, zero-backend** local uploader:  
Drag & drop files → auto-generate **UUIDv4 filenames** → upload via GitHub Contents API.  
Outputs **RAW / Blob / Website** links with concurrency control, retry, and one-click copy.

---

> ⚠️ **安全提示 / Security Warning**  
> 当前版本将 **GitHub Token 写在 HTML 文件中**。  
> 仅适合 **个人本地使用（localhost / 本机）**。  
> **不要** 将包含 Token 的文件上传到公开仓库或部署到线上。

---

## ✨ Features | 功能特性

- ✅ 拖拽 / 多文件选择  
  Drag & drop / multi-file selection
- ✅ 自动 UUID 文件名（保留扩展名）  
  Auto UUID filenames (extension preserved)
- ✅ 上传队列 + 并发控制  
  Upload queue with concurrency control
- ✅ 实时进度、失败重试  
  Progress bar & retry on failure
- ✅ 单文件 / 批量复制 RAW 链接  
  Copy single or all RAW links
- ✅ GitHub Token 一键自检  
  One-click GitHub token validation

---

## 🚀 Quick Start | 快速开始

### 1️⃣ 准备 GitHub 仓库  
Prepare a GitHub repository

你需要一个 GitHub 仓库用作图床，例如：

- `REPO_OWNER / REPO_NAME`
- 分支：`main`
- **默认上传目录：`public/`（强烈推荐）**

> 📌 **说明 / Note**  
> 默认使用 `public/` 作为上传路径，是因为：  
> 👉 **这是在 Vercel / Cloudflare Pages / GitHub Pages 上最省事、最通用的部署方案**。  
> 该目录通常会被直接映射为站点根目录，上传后即可访问。

The default upload directory is `public/` because:  
👉 **It’s the most convenient setup for Vercel / Cloudflare Pages / GitHub Pages**.  
Files in `public/` are usually served directly from the site root.

---

### 2️⃣ 创建 GitHub Token  
Create a GitHub Personal Access Token 🔑

- 推荐使用 **Fine-grained token**
- 仅授权 **目标仓库**
- 需要 **写入仓库内容（Contents API）** 权限

> ✅ 最小权限原则：只给一个仓库、只给必要权限

---

### 3️⃣ 配置 `index.html`  
Edit configuration constants

在 HTML 中找到并修改以下配置：

```js
const GITHUB_TOKEN = "YOUR-GITHUB-TOKEN";
const REPO_OWNER = "YOUR-NAME";
const REPO_NAME  = "YOUR-REPO-NAME";
const TARGET_DIR = "public/";
const BRANCH     = "main";
const COMMIT_MESSAGE_PREFIX = "upload:";
const CONCURRENCY = 3;
const YOUR_WEBSITE_BASE = "https://your-web-site.com/";
````

#### 参数说明 | Config Explanation

* **GITHUB_TOKEN**：GitHub Token（不要泄露）
* **REPO_OWNER / REPO_NAME**：仓库信息
* **TARGET_DIR**：上传目录（默认 `public/`，推荐）
* **BRANCH**：目标分支
* **CONCURRENCY**：并发上传数（过大会触发限流）
* **YOUR_WEBSITE_BASE**：

  * 可选，用于生成 Website 链接
  * If empty, Website links will be omitted

---

### 4️⃣ 本地运行

👉直接打开配置好的```index.html```即可  
👉Directly open ```index.html```  

---

## 🧩 Usage | 使用方法

1. 点击 **Test Token** 验证 Token 是否有效
   Click **Test Token** to validate credentials
2. 拖拽或选择文件
   Drag & drop or select files
3. 上传成功后生成三种链接：

   * **RAW**：直链（最适合 Markdown / 博客）
   * **Blob**：GitHub 页面
   * **Website**：你的网站域名（可选）
4. 点击 **Copy** 复制单个链接
5. 点击 **一键复制全部链接** 批量复制

---

## 🔗 Link Rules | 链接生成规则

**上传路径 / Upload Path**

```
{TARGET_DIR}/{uuid}.{ext}
```

**RAW**

```
https://raw.githubusercontent.com/{owner}/{repo}/{branch}/{path}
```

**Blob**

```
https://github.com/{owner}/{repo}/blob/{branch}/{path}
```

**Website（可选 / optional）**

```
YOUR_WEBSITE_BASE/{uuid}.{ext}
```

> ⚠️ Website 链接默认不包含 `public/`
> 因为在 Vercel / Pages 中，`public/` 通常会被映射到站点根目录。

---

## 🛠️ Troubleshooting | 常见问题

### ❌ Token Error / 401 / 403

* Token 无效或权限不足
* Fine-grained token 未授权该仓库
* API 触发限流（并发过高）

✅ 解决方案：

* 重新生成 Token
* 确认仓库写权限
* 降低 `CONCURRENCY`

---

### ❌ Website 链接打不开

* 站点未正确映射 `public/`

✅ 解决方案：

* 确认 Vercel / Pages 构建规则
* 或自行修改 `buildWebsiteUrl()` 逻辑

---

## 🔐 Security Notes | 安全须知

* ❌ 不要把含 Token 的文件上传或分享
* ✅ 仅限本地使用
* ✅ 使用最小权限 Token
* 🚀 若要公网部署：
  **必须引入后端 / OAuth 流程**，不要在前端暴露 Token

---

## 📄 License | 许可说明

* 代码部分：可使用 **MIT License**
* 内容（图片、文章）：**版权所有，未经许可禁止转载**

> 推荐做法：
> `LICENSE` 使用 MIT，README 中单独声明内容版权。

---

✨ Happy hacking & enjoy your self-hosted image bed!
✨ 祝你用得开心，图床飞起！
