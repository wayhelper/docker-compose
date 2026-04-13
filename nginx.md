# Nginx 反向代理配置

>docker compose :
```
version: "3.9"

services:
  nginx:
    image: nginx:latest
    container_name: nginx
    network_mode: host #共享主机ip端口
    environment:
      - TZ=Asia/Shanghai # 设置时区
    volumes:
      - /root/docker/nginx/conf:/etc/nginx
      - /root/docker/nginx/html:/usr/share/nginx/html
      - /root/docker/nginx/logs:/var/log/nginx
    restart: unless-stopped

```
>conf -> nginx.conf
```
events {
    worker_connections 16;
}

http {
    server_tokens off;
    #配置认证文件
    include /etc/nginx/ssl.conf;

    server {
	    listen 443 ssl;
	    server_name yuming.com; # 替换为你的域名
	    charset utf-8;
        #mail-api接口服务
	    location / {
	        proxy_pass http://ip:5010;
	        proxy_set_header X-Real-IP $remote_addr;
	        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		   # 确保 Authorization 头被正确转发
		   proxy_set_header Authorization $http_authorization;
		   # 如果客户端发送 Authorization 头，但 Nginx 默认不传递，可以强制设置：
		   proxy_pass_request_headers on;  # 默认是 on，但可以显式声明
	    }
    }
}
```