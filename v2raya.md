>docker compose :
```
services:
  v2raya:
    image: mzz2017/v2raya:latest          # 官方镜像
    container_name: v2raya                # 容器名称
    restart: always                       # 自动重启
    ports:
      - "2017:2017"                       # Web管理界面端口
      - "2334:20170"                       # HTTP代理端口 注意要开启端口共享
    volumes:
      # 请将 ./v2raya 替换为你想要存放配置文件的路径，例如：/home/user/v2raya
      - ./v2raya:/etc/v2raya              
    privileged: true                      # 需要此权限来管理网络
```