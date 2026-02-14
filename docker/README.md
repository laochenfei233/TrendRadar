# Docker 镜像使用说明

## 拉取镜像

镜像已发布到 GitHub Container Registry (GHCR)。使用以下命令拉取：

### 最新版本
```bash
docker pull ghcr.io/your-username/trendradar:latest
```

### 指定版本
```bash
docker pull ghcr.io/your-username/trendradar:v1.0.0
```

### 使用语义化版本标签
```bash
# 主要版本（始终指向该主版本的最新次版本）
docker pull ghcr.io/your-username/trendradar:v1

# 次要版本
docker pull ghcr.io/your-username/trendradar:v1.0
```

## 运行容器

### 基本运行
```bash
docker run -d \
  --name trendradar \
  -p 8000:8000 \
  -v $(pwd)/config:/app/config \
  -v $(pwd)/output:/app/output \
  ghcr.io/your-username/trendradar:latest
```

### 使用 docker-compose
```yaml
version: '3.8'

services:
  trendradar:
    image: ghcr.io/your-username/trendradar:latest
    container_name: trendradar
    ports:
      - "8000:8000"
    volumes:
      - ./config:/app/config
      - ./output:/app/output
    environment:
      - CONFIG_PATH=/app/config/config.yaml
      - FREQUENCY_WORDS_PATH=/app/config/frequency_words.txt
    restart: unless-stopped
```

## 配置说明

在运行容器前，请确保 `config` 目录包含以下文件：
- `config.yaml` - 主配置文件
- `frequency_words.txt` - 频率词文件

## GitHub Actions 自动构建

本项目配置了 GitHub Actions，在以下情况下自动构建并推送镜像：
- 推送到 main/master 分支
- 发布新版本标签（v*.*.*）
- 相关文件变更时的 Pull Request

镜像标签说明：
- `latest` - 最新构建的镜像
- `v1.0.0` - 具体版本号
- `v1` - 主要版本
- `v1.0` - 主要和次要版本
- `sha-xxxxxxx` - 基于 Git commit SHA
