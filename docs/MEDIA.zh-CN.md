# 官网截图 / GIF 怎么提供

首期官网媒体**精简**：先 1 张主图 + 4 段短 GIF 就够。

## 需要哪些文件

把文件放到本仓库的 `docs/assets/`，**文件名固定**（页面会自动识别）：

| 文件名 | 内容 | 建议时长 |
| --- | --- | --- |
| `hero.gif` 或 `hero.png` | 主窗口全貌（Browser + 列表 + 波形 + 电平） | 静图或 3–6 秒循环 |
| `wave.gif` | 波形 / 频谱切换、拖动播放头 | 3–5 秒 |
| `meters.gif` | 播放时电平 / 响度读数变化 | 3–5 秒 |
| `folder.gif` | 点文件夹、列表切换、过滤 | 3–5 秒 |
| `edit.gif` | 拖选区 / A-B / 导出或 Cue | 3–5 秒 |

也支持同名的 `.webp` / `.png` / `.jpg` / `.mp4`（静图或短视频）。有图就会替换占位框。

## 怎么拍（推荐）

1. 安装免费工具 **[ShareX](https://getsharex.com/)** 或 **[ScreenToGif](https://www.screentogif.com/)**
2. 打开 Sonora，准备好一段有内容的测试音频
3. **只框选窗口或局部控件**，不要录全桌面
4. GIF 建议：
   - 时长 **≤ 5 秒**
   - 分辨率宽度大约 **1280–1600 px**（太大上传慢、页面卡）
   - 帧率 10–15 fps 即可
   - 尽量暗色主题，和官网一致更好看
5. 不要出现隐私路径、客户工程名；可用测试文件夹

## 交给我 / 自己上传

任选一种：

- **发给我**：把 5 个文件发到对话里，或放进  
  `C:\Users\jianchenyang\Projects\sonora-site\docs\assets\`  
  然后说一声，我帮你提交推送
- **自己推**：复制到 `docs/assets/` 后执行

```powershell
cd C:\Users\jianchenyang\Projects\sonora-site
git add docs/assets
git commit -m "Add landing media"
git push
```

刷新 https://sonora-player.github.io/Sonora/ 即可看到图。

## 国内网络说明

官网托管在 **GitHub Pages**（`*.github.io`）。在大陆移动网络 / 无 VPN 时，整站或大图（尤其 1–2MB 的 GIF）经常打不开，这不是「只给外网看」的单独图床，而是 GitHub 访问受限。

页面已做两层兜底：

1. **优先加载较小的静图**（`.png` / `.webp` / `.jpg`），再尝试 GIF  
2. 同源失败时，改从 **jsDelivr CDN** 拉取同仓资源：  
   `https://cdn.jsdelivr.net/gh/sonora-player/Sonora@main/docs/assets/...`

若连 HTML 都打不开，需要镜像站或自建 Pages（Cloudflare / 国内对象存储），可另开任务。

## 不必一次做齐

可以先只给 `hero.png`（Win+Shift+S 截全窗也行），功能区 GIF 以后再补。
