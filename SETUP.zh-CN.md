# GitHub 上手指南（产品仓 Sonora）

本仓库是 **公开产品仓**：官网 + README + Releases，不含完整应用源码。

## 0. 准备

1. 记下你的 GitHub **用户名**（下面写成 `OWNER`）
2. 可选：安装 [GitHub Desktop](https://desktop.github.com/)（图形界面，适合新手）
3. 或安装 [GitHub CLI](https://cli.github.com/) 后在终端登录：`gh auth login`

## 1. 在网页上新建仓库

1. 打开 https://github.com/new
2. Repository name：`Sonora`
3. 选 **Public**
4. **不要**勾选 “Add a README”
5. Create repository

## 2. 把本文件夹推上去

在 `sonora-site` 目录打开终端（或用 GitHub Desktop：Add existing repository）：

```powershell
cd C:\Users\jianchenyang\Projects\sonora-site
git init
git add .
git commit -m "Initial Sonora product site and docs"
git branch -M main
git remote add origin https://github.com/OWNER/Sonora.git
git push -u origin main
```

把 `OWNER` 换成你的用户名。推送时浏览器会提示登录。

## 3. 替换占位符

在下列文件里，把 `OWNER` 换成真实用户名，并填写捐助链接：

- `README.md`
- `landing/index.html`（顶部 `CONFIG`：`repo`、`donateUrl`）
- 本地应用里的 `src/links.ts`（Website / Donate）

## 4. 开启官网（GitHub Pages）

1. 仓库 → **Settings** → **Pages**
2. Build and deployment → Source：**Deploy from a branch**
3. Branch：`main`，Folder：`/landing`
4. Save
5. 几分钟后打开：`https://OWNER.github.io/Sonora/`

## 5. 发布安装包（Release）

1. 仓库 → **Releases** → **Draft a new release**
2. Tag：`v0.9.1`（与版本一致）
3. 上传：
   - `Sonora_0.9.1_x64-setup.exe`
   - `sonora.exe`（可改名为 `Sonora_0.9.1_portable.exe`）
4. 写几句更新说明 → Publish

落地页会自动从 GitHub API 读取 latest release 的下载链接。

## 6. 自定义域名（可选，以后再做）

1. 在域名服务商为 `sonora` 子域添加 CNAME → `OWNER.github.io`
2. 在 `landing/` 下放文件 `CNAME`，内容一行：`sonora.soundoer.com`
3. Pages 设置里填写 Custom domain，勾选 Enforce HTTPS

不懂域名也可以一直用 `github.io` 地址。
