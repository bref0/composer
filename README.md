# Composer Private Repository

[![Deploy to Cloudflare](https://github.com/yourname/composer-repo/actions/workflows/deploy.yml/badge.svg)](https://github.com/yourname/composer-repo/actions)

## 📦 功能特性
- 基于 [Satis](https://github.com/composer/satis) 构建
- 通过 Cloudflare Pages 全球加速
- 支持私有包离线缓存

## 🚀 快速开始
### 服务端配置
```bash
# 安装 Satis
composer global require composer/satis
```

### 客户端使用
在 `composer.json` 中添加：
```json
{
    "repositories": [
        {
            "type": "composer",
            "url": "https://your-repo.pages.dev"
        }
    ]
}
```

## 🔒 访问控制
通过环境变量配置认证：
```yaml
# deploy.yml 片段
env:
  AUTH_USER: ${{ secrets.REPO_USER }}
  AUTH_PASS: ${{ secrets.REPO_PASS }}
```
