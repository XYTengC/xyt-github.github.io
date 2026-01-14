# GitHub OAuth应用设置指南

## 1. 创建GitHub OAuth应用

### 步骤1：登录GitHub账号
访问 [GitHub](https://github.com/) 并登录您的账号。

### 步骤2：进入Settings页面
点击右上角的头像，选择「Settings」。

### 步骤3：进入Developer settings
在左侧菜单中，找到并点击「Developer settings」。

### 步骤4：创建OAuth App
1. 点击「OAuth Apps」
2. 点击「New OAuth App」按钮

### 步骤5：填写应用信息

| 字段 | 值 |
|------|-----|
| Application name | 您的博客名称（如：Xyteng Blog CMS） |
| Homepage URL | `https://xytengc.github.io` |
| Application description | 可选，如：用于管理博客文章的Netlify CMS |
| Authorization callback URL | `http://localhost:4000/admin/` |

**重要：** 对于本地开发，回调URL设置为 `http://localhost:4000/admin/`；对于生产环境，设置为 `https://xytengc.github.io/admin/`。

### 步骤6：注册应用
点击「Register application」按钮创建应用。

## 2. 获取Client ID

创建成功后，您将看到应用的详细信息页面，其中包含：
- **Client ID**：一串字符，如 `Ov23liXGBUIHeygkv3Vu`
- **Client Secret**：69535cc4894f41436160ea0348e907adb4147bec点击「Generate a new client secret」生成

## 3. 更新Netlify CMS配置

获取Client ID后，将其添加到 `admin/config.yml` 文件中：

```yaml
# Netlify CMS configuration
backend:
  name: github
  repo: xytengc/xytengc.github.io
  branch: main
  client_id: YOUR_CLIENT_ID_HERE  # 添加这一行
```

## 4. 部署到GitHub Pages

### 步骤1：提交修改

```bash
git add .
git commit -m "Update Netlify CMS configuration with GitHub OAuth client ID"
git push origin main
```

### 步骤2：等待部署

GitHub Pages会自动部署您的网站，通常需要1-2分钟。

### 步骤3：验证部署

访问 `https://xytengc.github.io/admin/` 检查是否能正常登录。

## 5. 登录Netlify CMS

1. 访问 `https://xytengc.github.io/admin/`
2. 点击「Login with GitHub」按钮
3. 使用您的GitHub账号登录
4. 授权应用访问您的仓库
5. 成功登录后，您将看到Netlify CMS的管理界面

## 6. 故障排除

### 常见问题

1. **登录后跳转回登录页面**
   - 检查Client ID是否正确
   - 确保回调URL设置为 `https://api.netlify.com/auth/done`

2. **显示"Invalid client_id"**
   - 检查Client ID是否正确复制
   - 确保应用已在GitHub上成功创建

3. **显示"Permission denied"**
   - 确保您对仓库有写权限
   - 检查GitHub OAuth应用的权限设置

## 7. 安全建议

- 不要将Client Secret存储在代码中
- 定期更新Client Secret
- 只授予应用必要的权限
- 监控应用的访问日志

## 8. 完成

设置完成后，您就可以通过Netlify CMS轻松管理您的博客文章了！