# 方案：Nginx + Docker Compose

```
/your-project-root
├── docker-compose.yml
├── Dockerfile          <-- 您的应用 Dockerfile
├── nginx/
│   ├── app.conf        <-- Nginx 配置文件
│   ├── certs/          <-- 用于存放 SSL/TLS 证书
│   │   ├── fullchain.pem
│   │   └── privkey.pem
└── ... (apps/ packages/ 等您的 Monorepo 文件)


```

```
version: '3.8'

services:
  # 您的应用服务
  tsdd-app:
    # 假设您已经构建了名为 tsdd-image 的镜像
    # image: tsdd-image:latest 
    # 或者使用 build
    build: .
    # 在 Docker 网络内部，您的应用容器监听 82 端口
    expose:
      - 82
    # 确保应用容器在网络中的名称，供 Nginx 引用
    container_name: tsdd-app
    restart: always

  # Nginx 反向代理服务
  nginx:
    image: nginx:stable-alpine
    container_name: nginx-proxy
    restart: always
    # 将容器的 80 和 443 端口映射到宿主机外部
    ports:
      - "80:80"   # 映射 HTTP 流量
      - "443:443" # 映射 HTTPS 流量
    volumes:
      # 挂载您的 Nginx 配置文件
      - ./nginx/app.conf:/etc/nginx/conf.d/default.conf:ro
      # 挂载您的 SSL 证书
      - ./nginx/certs:/etc/nginx/certs:ro
    # 确保 Nginx 在应用启动后才启动
    depends_on:
      - tsdd-app
```

```

# ------------------------------------
# HTTP 重定向到 HTTPS (监听 80 端口)
# ------------------------------------
server {
    listen 80;
    server_name yourdomain.com; # 替换为您的域名

    # 强制将所有 HTTP 请求重定向到 HTTPS
    return 301 https://$host$request_uri;
}


# ------------------------------------
# HTTPS 服务配置 (监听 443 端口)
# ------------------------------------
server {
    listen 443 ssl;
    server_name yourdomain.com; # 替换为您的域名

    # ========================================
    # 1. 配置 HTTPS 证书 (TLS Termination)
    # ========================================
    # 这些路径与 docker-compose.yml 中定义的 /nginx/certs 挂载点对应
    ssl_certificate /etc/nginx/certs/fullchain.pem;
    ssl_certificate_key /etc/nginx/certs/privkey.pem;

    # 推荐的 SSL/TLS 安全配置
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers 'ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384';
    ssl_prefer_server_ciphers on;

    # ------------------------------------
    # 2. 流量转发配置
    # ------------------------------------
    location / {
        # ========================================
        # 3. 将流量转发到应用容器的 82 端口
        #    tsdd-app 是 docker-compose.yml 中定义的 服务名称
        # ========================================
        proxy_pass http://tsdd-app:82;

        # ========================================
        # 4. 配置 WebSocket 和 SSE 升级头 (关键！)
        #    这是支持 WebSocket/SSE 长连接必须的配置
        # ========================================
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        
        # 其他常用的转发头
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}

```