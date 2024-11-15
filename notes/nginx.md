## Nginx 配置示例

```
server {
    listen 80;
    listen 443 ssl;
    server_name mange.webrtc.win;
    client_max_body_size 100M;


    # ssl on;
    ssl_certificate /etc/nginx/ca_certs/certificates/webrtc.win.crt;
    ssl_certificate_key /etc/nginx/ca_certs/certificates/webrtc.win.key;

    location / {
        proxy_pass http://127.0.0.1:8989;
        proxy_http_version 1.1;
        proxy_set_header Connection "";
        proxy_set_header  Host  $host;
        proxy_set_header  X-Real-IP  $remote_addr;
        proxy_set_header  X-Forwarded-For $proxy_add_x_forwarded_for;
   }

}
```

这段Nginx配置文件定义了一个Web服务器，该服务器监听80端口（HTTP）和443端口（HTTPS），并为名为`mange.webrtc.win`的域名提供服务。下面是对每个部分的详细分析：

### 监听端口
- `listen 80;`: 这行让Nginx监听80端口，这是标准的HTTP端口。
- `listen 443 ssl;`: 这行让Nginx同时监听443端口，这是标准的HTTPS端口，并且启用了SSL加密。

### 服务器名称
- `server_name mange.webrtc.win;`: 定义了此Nginx配置将响应哪个主机名或域名的请求。在这个例子中，是`mange.webrtc.win`。

### 客户端最大请求体大小
- `client_max_body_size 100M;`: 设置客户端能够发送的最大请求体大小为100MB。这对于上传文件的场景特别有用，可以防止因为文件过大而导致的请求失败。

### SSL设置
- 虽然配置中有注释掉的`# ssl on;`，但在`listen 443 ssl;`中已经启用了SSL，因此不需要这行。
- `ssl_certificate /etc/nginx/ca_certs/certificates/webrtc.win.crt;`: 指定了用于HTTPS连接的SSL证书的路径。
- `ssl_certificate_key /etc/nginx/ca_certs/certificates/webrtc.win.key;`: 指定了与上述证书匹配的私钥文件的路径。

### 代理设置
- `location / { ... }`: 定义了如何处理对根路径`/`的所有请求。
- `proxy_pass http://127.0.0.1:8989;`: 将所有请求代理到本地运行在8989端口的服务上。这意味着Nginx作为一个反向代理服务器，转发来自客户端的请求到后端服务。
- `proxy_http_version 1.1;`: 设置了代理使用HTTP 1.1版本，这通常是推荐的做法，特别是当后端服务支持WebSocket等需要持久连接的功能时。
- `proxy_set_header Connection "";`: 清空`Connection`头信息，这是为了确保HTTP 1.1的行为正确，特别是在使用WebSocket的情况下。
- `proxy_set_header Host $host;`: 将原始请求中的Host头信息传递给后端服务。
- `proxy_set_header X-Real-IP $remote_addr;`: 将客户端的真实IP地址传递给后端服务，这对于日志记录和安全目的非常有用。
- `proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;`: 将客户端的IP地址添加到`X-Forwarded-For`头信息中，如果请求经过多个代理，则会保留所有中间代理的IP地址。

综上所述，这个Nginx配置主要用于将HTTP和HTTPS请求转发到本地的另一个服务（如一个Web应用或API服务），同时提供了必要的SSL加密和合理的请求头信息处理，以确保后端服务能够正确处理请求。


## 查看nginx状态
sudo systemctl status nginx

## 停止Nginx
sudo systemctl stop nginx

## 启动Nginx
sudo systemctl start nginx

## 重新加载Nginx服务
sudo systemctl reload nginx
