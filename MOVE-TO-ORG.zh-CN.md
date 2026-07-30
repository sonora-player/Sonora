# 迁到组织 sonora-player（去掉网址里的个人名）

完成后对外地址：

- 官网：https://sonora-player.github.io/Sonora/
- 说明书：https://sonora-player.github.io/Sonora/guide/
- 仓库：https://github.com/sonora-player/Sonora

以后若买域名或 NAS 反代，把 DNS / 反代指到该 Pages 即可，不必再改组织。

## 网页操作（约 5 分钟）

### 1. 创建组织

1. 打开 https://github.com/organizations/plan  
2. **Create a free organization**  
3. 名称填 `sonora-player`（占用则用 `sonora-app` 等并告诉我）  
4. 创建完成  

### 2. 转移仓库

1. https://github.com/JANFNASY/Sonora/settings  
2. 底部 **Danger Zone** → **Transfer ownership**  
3. 新拥有者：`sonora-player` → 确认转移  

### 3. 打开 Pages

1. https://github.com/sonora-player/Sonora/settings/pages  
2. Branch `main`，Folder `/docs` → Save  
3. 打开 https://sonora-player.github.io/Sonora/

### 4. 本地 remote（转移完成后）

```powershell
cd C:\Users\jianchenyang\Projects\sonora-site
git remote set-url origin https://github.com/sonora-player/Sonora.git
```

做完回复「已转移」即可。
