# 私有 Composer 仓库部署指南

## 项目概述

本项目使用 [Satis](https://github.com/composer/satis) 搭建私有 Composer 包仓库，支持：
- ✅ 托管私有 PHP 包
- ⚡ 镜像加速公共包
- 📦 离线包缓存
- 🔒 安全访问 Git 私有仓库

## 快速开始

### 1. 客户端配置

在项目 `composer.json` 中添加：

```json
{
    "repositories": [
        {
            "type": "composer",
            "url": "https://yourname.github.io/composer"
        }
    ]
}
```

## 私有仓库配置指南(以Codeup为例)

### 1. Codeup SSH 密钥配置

#### 1.1 生成部署密钥

```bash
ssh-keygen -t ed25519 -C "codeup-deploy@yourcompany.com" -f codeup-deploy-key
```

#### 1.2 添加公钥到 Codeup
1. 复制公钥内容：
   ```bash
   cat codeup-deploy-key.pub
   ```
2. 登录 Codeup → 项目设置 → 部署密钥 → 添加公钥

#### 1.3 在 GitHub 中添加私钥
1. 进入仓库 Settings → Secrets → Actions
2. 添加新 secret：
   - 名称：`CODEUP_SSH_KEY`
   - 值：粘贴 `codeup-deploy-key` 文件内容

#### 1.4 修改 satis.json

```json
{
  "name": "your-repo-name",
  "repositories": [
    {
      "type": "git",
      "url": "git@codeup.aliyun.com:your-group/your-repo.git"
    }
  ],
  "require": {
    "your-group/your-repo": "*"
  }
}
```

### 2. 工作流说明

本仓库使用 GitHub Actions 自动构建，主要流程：

1. **配置 SSH 访问**：
   - 加载存储在 Secrets 的 SSH 私钥
   - 添加 Codeup 到 known_hosts
   - 测试连接

2. **构建环境准备**：
   - 缓存 Composer 依赖
   - 安装 PHP 和指定工具
   - 安装 Satis 工具

3. **仓库构建**：
   - 根据 satis.json 构建仓库
   - 缓存构建结果

4. **发布到 GitHub Pages**

## 构建与部署

### 手动触发构建

1. 修改 satis.json 或添加新包
2. 推送更改到 main 分支
3. 自动触发 GitHub Actions 工作流

### 查看构建结果

构建完成后，访问：
```
https://yourname.github.io/composer
```

## 常见问题

### Q: 如何测试 SSH 连接是否正常？

在 GitHub Actions 日志中查看 "测试连接" 步骤的输出，应显示：
```
Welcome to Codeup, your_username!
```

### Q: 构建失败如何排查？

1. 检查 Actions 运行日志
2. 验证 SSH 密钥是否正确配置
3. 确认 satis.json 格式正确

### Q: 如何更新私有包？

1. 在私有仓库发布新版本
2. 推送更改到 main 分支触发自动构建
3. 客户端运行 `composer update`

---