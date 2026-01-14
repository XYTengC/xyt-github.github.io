# GitHub App设置指南

## 1. 问题分析

之前的登录问题是因为Netlify CMS默认尝试使用Netlify的认证服务，而我们的网站部署在GitHub Pages上，不需要也无法使用Netlify的认证服务。

## 2. 解决方案

我们将使用GitHub App直接进行认证，完全绕过Netlify的认证服务。

## 3. 创建GitHub App

### 步骤1：登录GitHub账号
访问 [GitHub](https://github.com/) 并登录您的账号。

### 步骤2：进入Settings页面
点击右上角的头像，选择「Settings」。

### 步骤3：进入Developer settings
在左侧菜单中，找到并点击「Developer settings」。

### 步骤4：创建GitHub App
1. 点击「GitHub Apps」
2. 点击「New GitHub App」按钮

### 步骤5：填写基本信息

| 字段 | 值 |
|------|-----|
| GitHub App name | 您的博客名称（如：Xyteng Blog CMS） |
| Homepage URL | `https://xytengc.github.io` |
| Description | 可选，如：用于管理博客文章的Netlify CMS |
| Webhook URL | 留空 |
| Active (Webhook) | 取消勾选 |

### 步骤6：设置权限

#### Repository permissions
- **Contents**：选择「Access: Read & Write」（必须，用于读写文章）
- **Pull requests**：选择「Access: Read & Write」（可选，用于创建PR）

#### User permissions
- 无需设置

### 步骤7：设置回调URL

在「Callback URL」字段中填写：
```
http://localhost:4000/admin/
https://xytengc.github.io/admin/
```

**重要：** 可以添加多个回调URL，用于本地开发和生产环境。

### 步骤8：创建应用
点击「Create GitHub App」按钮创建应用。

## 4. 获取App ID

创建成功后，您将在应用的详细信息页面中找到：
- **App ID**：一串数字，如 `123456`

## 5. 更新Netlify CMS配置

### 步骤1：编辑admin.html文件

打开 `admin.html` 文件，找到以下代码：

```javascript
window.CMS_CONFIG = {
  backend: {
    name: 'github',
    repo: 'xytengc/xytengc.github.io',
    branch: 'main',
    auth_type: 'implicit',
    app_id: 'YOUR_GITHUB_APP_ID' // 替换为您的GitHub App ID
  },
  // ... 其他配置
};
```

### 步骤2：替换App ID

将 `YOUR_GITHUB_APP_ID` 替换为您从GitHub获取的App ID，例如：

```javascript
app_id: '123456' // 替换为您的GitHub App ID
```

## 6. 部署到GitHub Pages

### 步骤1：提交修改

```bash
git add .
git commit -m "Update Netlify CMS configuration with GitHub App ID"
git push origin main
```

### 步骤2：等待部署

GitHub Pages会自动部署您的网站，通常需要1-2分钟。

## 7. 登录Netlify CMS

1. 访问 `https://xytengc.github.io/admin/`
2. 点击「Login with GitHub」按钮
3. 使用您的GitHub账号登录
4. 授权应用访问您的仓库
5. 成功登录后，您将看到Netlify CMS的管理界面

## 8. 故障排除

### 常见问题

1. **登录后跳转回登录页面**
   - 检查App ID是否正确
   - 确保回调URL设置正确
   - 检查应用权限是否设置为「Read & Write」

2. **显示"Invalid app_id"**
   - 检查App ID是否正确复制
   - 确保应用已在GitHub上成功创建

3. **显示"Permission denied"**
   - 确保您对仓库有写权限
   - 检查GitHub App的权限设置

## 9. 本地开发测试

1. 启动本地服务器：`bundle exec jekyll serve`
2. 访问 `http://localhost:4000/admin/`
3. 点击「Login with GitHub」按钮
4. 登录并授权
5. 测试管理功能

## 10. 安全建议

- 定期检查应用的访问日志
- 只授予应用必要的权限
- 定期更新应用的密钥
- 启用双因素认证

## 11. 完成

设置完成后，您就可以通过Netlify CMS轻松管理您的博客文章了！