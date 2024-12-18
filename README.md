# 私有化部署jitsi-meet

服务器：Centos 7.9

## 环境准备

#### 域名解析（没有也可以通过ip访问）

将准备好的域名解析到服务器

#### 安装docker
 `自行查找资料`

#### 项目下载

1. git拉取

   ```git clone https://github.com/jitsi/docker-jitsi-meet.git```

2. 下载zip文件，然后上传到服务器

## 部署

两种方法二选一，推荐第二种

安装解压工具
```
yum -y install unzip
```

解压文件
```unzip docker-jitsi-meet-master.zip```

解压完成，进入目录
```cd docker-jitsi-meet-master```

拷贝`env.example`文件，作为配置文件

```cp env.example .env```

修改配置文件`.env`

```vi .env```

```TZ=UTC ==> TZ=Asia/Shanghai （可不改）
PUBLIC_URL 去掉`#` 并改为 PUBLIC_URL=https://你的域名
ENABLE_GUESTS 去掉 `#`
AUTH_TYPE 去掉 `#`
```


生成密码```sh gen-passwords.sh```


启动
```docker-compose up -d```

等待启动完成


##### nginx安装

`nginx安装方法请自行查阅`


##### nginx配置代理

去掉443端口注释
替换location / 配置
```
location ^~ / {
    proxy_pass https://127.0.0.1:8443; 
    proxy_set_header Host $host; 
    proxy_set_header X-Real-IP $remote_addr; 
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; 
    proxy_set_header REMOTE-HOST $remote_addr; 
    proxy_set_header Upgrade $http_upgrade; 
    proxy_set_header Connection $http_connection; 
    proxy_set_header X-Forwarded-Proto $scheme; 
    proxy_http_version 1.1; 
    add_header X-Cache $upstream_cache_status; 
    add_header Cache-Control no-cache; 
    proxy_ssl_server_name off; 
    proxy_ssl_name $proxy_host; 
    add_header Strict-Transport-Security "max-age=31536000"; 
    proxy_set_header Accept-Encoding ""; 
}

```

启动nginx

访问`你的域名`







 

