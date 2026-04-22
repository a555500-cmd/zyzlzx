# 中医治疗执行系统 - Netlify 部署指南

## 缓存问题解决方案

当你在Netlify上更新网页后发现页面不更新，可能是由于以下原因：

### 1. 浏览器缓存

**解决方案：**
- 按 `Ctrl + Shift + R` (Windows) 或 `Cmd + Shift + R` (Mac) 强制刷新页面
- 清除浏览器缓存后重新访问

### 2. Service Worker 缓存

本项目使用了Service Worker进行缓存管理，为了解决更新问题：

- 已修改为网络优先策略，确保优先从网络获取最新内容
- 缓存名称会自动添加时间戳，确保每次更新都会生成新缓存
- 旧缓存会在新Service Worker激活时自动清理

### 3. Netlify CDN 缓存

**Netlify 缓存清除方法：**

1. **通过 Netlify 管理界面清除缓存：**
   - 登录 Netlify 账户
   - 进入你的站点控制台
   - 点击 "Deploys" 选项卡
   - 点击 "Clear cache and deploy site" 按钮
   - 等待部署完成

2. **通过 Netlify API 清除缓存：**
   ```bash
   # 使用 curl 命令
   curl -X POST "https://api.netlify.com/api/v1/sites/YOUR_SITE_ID/clear_cache" \
     -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
   ```

3. **通过 Netlify CLI 清除缓存：**
   ```bash
   # 安装 Netlify CLI
   npm install -g netlify-cli
   
   # 登录并清除缓存
   netlify login
   netlify clear-cache
   ```

### 4. 最佳实践

- **每次更新时修改版本号：** 可以在 `service-worker.js` 中手动更新版本号
- **使用内容哈希：** 对于静态资源，可以在文件名中添加内容哈希
- **设置合理的缓存头：** 已在 `index.html` 中添加了缓存控制元标签
- **监控部署状态：** 确保Netlify部署成功完成

### 5. 常见问题排查

- **检查部署状态：** 确认Netlify部署是否成功
- **检查浏览器开发者工具：** 在Network选项卡中查看资源加载情况
- **检查Service Worker状态：** 在Application选项卡中查看Service Worker状态
- **尝试不同浏览器：** 确认是否是特定浏览器的缓存问题

## 部署步骤

1. 将代码推送到GitHub仓库
2. 在Netlify中连接该仓库
3. 配置部署设置（默认即可）
4. 等待自动部署完成
5. 访问生成的Netlify URL

## 技术支持

如果遇到持续的缓存问题，请：

1. 尝试上述所有缓存清除方法
2. 检查Netlify部署日志
3. 确认代码更改已正确提交
4. 联系Netlify支持团队
