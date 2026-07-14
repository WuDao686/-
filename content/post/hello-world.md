+++
date = '2026-07-14T09:19:10+08:00'
draft = false
title = '欢迎来到雾之岛'
categories = ['博客']
tags = ['Hugo', '博客搭建', '部署']
+++

## 缘起

一直想有一个属于自己的小角落，写点东西，记录生活。于是就有了「雾之岛」这个博客。

从零到上线，整个过程断断续续折腾了两天，这里记录一下，也给想自己搭博客的朋友一个参考。

## 技术选型

| 环节       | 选择             | 原因                                      |
| ---------- | ---------------- | ----------------------------------------- |
| 静态生成器 | Hugo             | 极速构建，Go 语言编写，一个二进制文件搞定 |
| 主题       | hugo-theme-reimu | 简洁优雅，喜欢灵梦                        |
| 图标       | iconfont         | 阿里巴巴图标库，自定义灵活                |
| 代码托管   | GitHub           | 免费，配合 Actions 自动部署               |
| 服务器     | 百度云 BCC 4C4G  | 国内访问快，备案方便                      |
| Web 服务   | Nginx            | 轻量高性能                                |
| HTTPS      | Let's Encrypt    | 免费 SSL 证书                             |

## 本地搭建

Hugo 的安装很简单，下载一个 exe 文件就行。选好主题后，主要做了这些定制：

### 图标替换

使用的是 iconfont，创建了一个项目，选了四个图标（首页、归档、关于、友链）。下载后将字体文件放到 `static/fonts/iconfont/` 目录下，在 `hugo.toml` 里配置图标编码即可。

### 点击特效

主题自带了 `mouse-firework` 点击特效。不过默认是红色圆点，我想要金色爱心的效果。原库不支持爱心形状，所以通过 `injector` 注入了一段 JS，用贝塞尔曲线绘制爱心注册进去。

### 备案信息

国内服务器必须备案。主题的 footer 模板已经内置了 ICP 和公安备案的支持，配置两个参数就行：

```toml
[params.icp]
  icpnumber = "闽ICP备2025121748号"
  beian = "闽公网安备35082202820160号"
  recordcode = "35082202820160"
```

## 服务器部署

服务器是百度云的，Debian 12 系统。整个部署架构如下：

```
本地写文章 → git push → GitHub Actions 自动构建 → rsync 推送到服务器 → 上线
```

### 安装依赖

```bash
apt install -y nginx git rsync certbot python3-certbot-nginx
```

### 配置自动部署

1. 服务器上创建 `deploy` 用户，生成 SSH 密钥对
2. GitHub 仓库配置两个 Secrets：`SSH_PRIVATE_KEY` 和 `SERVER_HOST`
3. 创建 `.github/workflows/deploy.yml`，每次 push 自动构建 Hugo 并 rsync 到服务器

### HTTPS

```bash
certbot --nginx -d lctnb.top -d www.lctnb.top
```

Let's Encrypt 的证书 90 天过期，certbot 会自动续期，不用操心。

## 写文章

日常更新只需要三步：

```bash
hugo new post/文章标题.md  # 创建文章
# 编辑 content/post/xxx.md
git add . && git commit -m "新文章" && git push  # 推送自动部署
```

## 最后

从零到一，搭建个人博客这件事本身并不难，难的是坚持写下去。希望「雾之岛」能成为一片安静的小天地，记录下那些值得留下的人和事。

共勉。
