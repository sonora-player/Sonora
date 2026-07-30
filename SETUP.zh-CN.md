# GitHub 上手指南（产品仓 Sonora）

仓库地址：https://github.com/JANFNASY/Sonora  
官网（Pages 开启后）：https://janfnasy.github.io/Sonora/

## 推送本文件夹（第二步）

本地内容已准备好。在 PowerShell 中执行：

```powershell
cd C:\Users\jianchenyang\Projects\sonora-site
git remote add origin https://github.com/JANFNASY/Sonora.git
git push -u origin main
```

若提示 remote 已存在，改用：

```powershell
git remote set-url origin https://github.com/JANFNASY/Sonora.git
git push -u origin main
```

推送时会弹出登录窗口：用浏览器登录 GitHub 即可（不要在网页那页再执行 `echo "# Sonora"` 那套「新建空仓」命令，本地已经有完整文件）。

## 开启官网（第三步）

1. 打开 https://github.com/JANFNASY/Sonora/settings/pages
2. Source → **Deploy from a branch**
3. Branch：`main`，Folder：选 **`/docs`**（GitHub 只支持 root 或 docs，不要找 /landing）→ Save
4. 几分钟后打开：https://janfnasy.github.io/Sonora/

## 发布安装包（第四步）

1. https://github.com/JANFNASY/Sonora/releases/new
2. Tag：`v0.9.2`
3. 上传 `dist-app` 里的 `Sonora_0.9.2_x64-setup.exe` 和 `Sonora-0.9.2-portable.exe`
4. Publish release

## 支持开发（Support Sonora）

开通后确认：

- 官网：`docs/index.html` → Support 区块（Ko-fi / 爱发电二级选择）
- 应用：`src/links.ts` → `donate` 指向官网 `#support`
- GitHub：`.github/FUNDING.yml` + README「Support Sonora」

免责声明（请保留）：

> 赞助完全自愿，不代表购买软件所有权、优先技术支持或特定功能承诺。
