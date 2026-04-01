# SearXNG Search CLI Onboarding

> 本文档指导 AI Agent 完成首次环境配置。按顺序执行，遇到问题时参考 Troubleshooting。

## Prerequisites | 前置条件

- Python 3.x（运行 `python3 --version` 确认）
- 网络（需克隆 SearXNG 仓库）
- 可用端口（默认 8888）

## Step 1: 检查 Python 环境

### 检测
```bash
python3 --version
```
期望输出：`Python 3.x.x`

如果不存在，尝试：
```bash
python --version
which python3 || which python
```

如果完全没有 Python，告知用户需要安装 Python 3。

## Step 2: 安装 uv

### 检测
```bash
command -v uv && uv --version
```

如果已安装，跳到 Step 3。

### 安装
```bash
# 尝试 1: 直接安装
curl -LsSf https://astral.sh/uv/install.sh | sh

# 尝试 2: 如果 PATH 未生效
export PATH="$HOME/.local/bin:$PATH"

# 尝试 3: 如果无 curl
wget -qO- https://astral.sh/uv/install.sh | sh
```

### 验证
```bash
uv --version
```

## Step 3: 一键安装 SearXNG

### 检测
```bash
searxng-search status 2>&1
```

如果显示服务信息，跳到 Step 5。

### 安装
```bash
# 确保脚本可执行
chmod +x <skill_dir>/scripts/searxng_cli.py

# 创建 symlink（如果不存在）
which searxng-search || sudo ln -sf <skill_dir>/scripts/searxng_cli.py /usr/local/bin/searxng-search

# 一键安装（自动完成：克隆、venv、依赖、配置、启动）
searxng-search install
```

> 安装过程会自动克隆 SearXNG 到 `~/projects/searxng`，创建 venv，安装依赖，启用 JSON API，并启动服务。

### 验证
```bash
searxng-search status
```
期望输出：显示服务运行中（running, port 8888）

## Step 4: 验证搜索功能

### 验证
```bash
searxng-search search "test"
```
期望输出：JSON 格式的搜索结果列表

```bash
curl -s "http://127.0.0.1:8888/search?q=test&format=json" | python3 -m json.tool | head -5
```
期望输出：有效的 JSON 响应

## Onboarding 完成

以上步骤全部通过后，onboarding 完成。后续可直接使用 `searxng-search` 命令。

## Troubleshooting | 故障排除

| 错误 | 原因 | 解决 |
|------|------|------|
| `command not found: searxng-search` | symlink 未创建 | Step 3 重新创建 symlink |
| `command not found: uv` | uv 未安装或 PATH 未生效 | Step 2 重新安装，检查 PATH |
| `connection refused` | 服务未启动 | `searxng-search start` |
| `403 Forbidden` | JSON API 未启用 | 重新运行 `searxng-search install` |
| `timeout` | 搜索引擎被屏蔽 | 更换引擎 `--engine` 或添加代理 |
| 安装失败 `Permission denied` | 无写入权限 | 检查 `~/projects/` 目录权限 |
| 端口 8888 被占用 | 其他服务占用 | 设置 `SEARXNG_PORT` 环境变量后重新安装 |
| `No module named 'xxx'` | venv 依赖缺失 | 删除 `~/projects/searxng` 后重新 `searxng-search install` |
