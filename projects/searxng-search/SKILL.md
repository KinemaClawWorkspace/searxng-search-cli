---
name: searxng-search
description: |
  使用自托管 SearXNG 搜索引擎进行搜索。SearXNG 是一个免费的元搜索引擎，
  聚合 200+ 搜索引擎（Google, Bing, Brave、GitHub 等），完全免费且可自托管。
  触发条件：用户需要搜索查询、调研信息、查找资源等。
---

# SearXNG Search - 搜索工具

使用 SearXNG 自托管搜索 API 进行快速、准确的搜索。

## 快速开始

```bash
# 一键安装 + 启动
searxng-search install

# 使用
searxng-search search "你的查询"
```

## 命令列表

| 命令 | 说明 |
|------|------|
| install | 一键安装 SearXNG |
| start | 启动服务 |
| stop | 停止服务 |
| restart | 重启服务 |
| status | 查看服务状态 |
| search \<query\> | 搜索 |
| enable | 开机自启 |
| disable | 取消开机自启 |

## 安装 (install)

自动完成以下步骤：
1. 安装 uv (如未安装)
2. 克隆 SearXNG 到 ~/projects/searxng
3. 创建 Python 虚拟环境
4. 安装依赖
5. 启用 JSON API
6. 启动服务

```bash
searxng-search install
```

### 配置

安装后可配置以下环境变量：
- `SEARXNG_PORT` - 端口 (默认 8888)
- `SEARXNG_HOST` - 绑定地址 (默认 127.0.0.1)
- `SEARXNG_SECRET` - 认证密钥 (自动生成)

## 服务管理

```bash
# 启动
searxng-search start

# 停止
searxng-search stop

# 重启
searxng-search restart

# 状态
searxng-search status

# 开机自启
searxng-search enable

# 取消自启
searxng-search disable
```

## 搜索

### 命令行

```bash
searxng-search search "Python 教程"

# 指定引擎
searxng-search search "git clone" --engine github

# 指定语言
searxng-search search "AI 新闻" --lang zh

# 分页
searxng-search search "llm" --page 2

# 时间过滤
searxng-search search "python" --time-range month
```

### API 调用

```bash
# 基本搜索
curl "http://127.0.0.1:8888/search?q=test&format=json"

# 指定引擎和语言
curl "http://127.0.0.1:8888/search?q=python&format=json&engines=github&lang=en"

# 时间过滤
curl "http://127.0.0.1:8888/search?q=llm&format=json&time_range=month"
```

## 参数说明

### search 参数

| 参数 | 简写 | 说明 | 示例 |
|------|------|------|------|
| --engine | -e | 指定引擎 | github, google |
| --lang | -l | 语言 | zh, en, auto |
| --page | -p | 页码 | 1, 2, 3 |
| --time-range | -t | 时间范围 | day, week, month, year |
| --safe-search | -s | 安全搜索 | 0, 1, 2 |

### 可用引擎

通用搜索：google, bing, brave, duckduckgo, yandex, startpage, qwant
代码/开发：github, gitlab, stackoverflow, npm, pypi
学术：arxiv, pubmed, wikipedia, google-scholar
视频/图片：youtube, vimeo, pexels, pixabay

## 故障排除

| 问题 | 原因 | 解决方案 |
|------|------|---------|
| connection refused | 服务未运行 | `searxng-search start` |
| 403 Forbidden | JSON 未启用 | 重新 `install` |
| timeout | 引擎被封 | 换引擎或加代理 |
| 安装失败 | 权限不足 | 检查 uv 路径 |

## 项目结构

```
searxng-search/
├── SKILL.md
├── scripts/
│   └── searxng_cli.py      # CLI 主程序
└── references/
    └── settings.yml        # SearXNG 配置
```

## 相关文档

- SearXNG 官方文档: https://docs.searxng.org
- GitHub: https://github.com/searxng/searxng
