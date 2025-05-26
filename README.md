# Satis 私有 Composer 仓库部署指南

## 项目概述
本项目使用 [Satis](https://github.com/composer/satis) 搭建私有 Composer 包仓库，支持：
- ✅ 托管私有 PHP 包
- ⚡ 镜像加速公共包
- 📦 离线包缓存

## 快速开始

### 客户端配置
在项目 `composer.json` 中添加：

```json
{
    "repositories": [
        {
            "type": "composer",
            "url": "https://yourname.github.io/composer-repo"
        }
    ]
}