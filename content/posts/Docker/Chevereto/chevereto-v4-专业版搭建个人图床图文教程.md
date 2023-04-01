---
title: "Chevereto v4 专业版搭建个人图床教程以及问题排查"
date: "2023-03-20"
categories: 
  - "docker"
featuredImage: https://cdn.lirica.cn/webp/00003.webp
license: CC BY-NC-SA 4.0

hiddenFromHomePage: false
hiddenFromSearch: false
---

## **开心版想体验的下载：[地址](https://alist.lycoris.beauty/d/onedrive/Wordpress-%20Plugins/chevereto_4.0.1.zip?sign=ejZIMIw2L9YgkwNXr5kNhh1cawOrWk6kCf8hwiyC2Ok=:0)**

## 1.宝塔面板安装

### 环境需求

PHP 版本要求 8.0 及以上，MySQL 版本支持 5.x 及 8.0， fileinfo、imagemagick 和 exif三个php扩展

![](https://cdn.lirica.cn/wordpress/2023/03/image-35.png)

### 上传获得授权的安装包并解压缩

![](https://cdn.lirica.cn/wordpress/2023/03/image-36.png)

### 配置nginx伪静态

```
location ~* /(importing|app|content|lib)/.*\.(po|php|lock|sql)$ {
    deny all;
}
location ~ \.(jpe?g|png|gif|webp)$ {
    log_not_found off;
    error_page 404 /content/images/system/default/404.gif;
}
location ~* /.*\.(ttf|ttc|otf|eot|woff|woff2|font.css|css|js)$ {
    add_header Access-Control-Allow-Origin "*";
}
location / {
    index index.php;
    try_files $uri $uri/ /index.php$is_args$query_string;
}
```

![](https://cdn.lirica.cn/wordpress/2023/03/image-37.png)

### 开始安装

访问域名或者ip进行安装

![](https://cdn.lirica.cn/wordpress/2023/03/image-38.png)

### 可能遇到的问题

![](https://cdn.lirica.cn/wordpress/2023/03/efdb0132bbb33d23935e0d0e1e2e551c.png)

### 解决办法

关闭宝塔网站目录里的防跨站攻击

![](https://cdn.lirica.cn/wordpress/2023/03/image-39.png)

## 2.docker 安装（推荐）

## [#](#advantages)优点

- 🤹 多个网站实例

- 📱 便携带性

- 🌈易于维护

- 🔐 自动 HTTPS 设置

- 🎨 定制

- 👮‍♂️更安全

- 🌎 CloudFlare 集成

## 要求

- Chevereto 许可证（付费版）
    - [购买（打开新窗口）](https://chevereto.com/pricing) 新执照
    
    - [使用权（打开新窗口）](https://chevereto.com/panel/license) 现在有购买

- vps
    - `make`,和`unzip` `curl git`
    
    - ssh权限

- 指向vps的主机域名

## Docker一键安装

```
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```

## 克隆 chevereto/docker

```
git clone https://github.com/chevereto/docker.git
cd docker
```

## 设置 Cron

此过程创建一个 Cron 文件，`/etc/cron.d/chevereto`该文件将为服务器中的所有 Chevereto 实例运行后台作业。

```
make cron
```

## 创建代理

此过程创建代理服务来处理传入的网络流量到服务器。它还将为 HTTPS 提供自动安全证书。

在`EMAIL_HTTPS`选项中传递您的电子邮件。

```
make proxy EMAIL_HTTPS=mail@yourdomain.tld
```

## 构建 Chevereto

此过程为 Chevereto 应用程序构建容器。

```
make image
```

## [#](#setup-namespace)设置命名空间

为您要部署的每个 Chevereto 实例创建一个[命名空间(opens new window) 。](https://github.com/chevereto/docker/blob/4.0/docs/NAMESPACE.md)

为vps创建`**example**`命名空间`**img.chevereto.dev**`（需要替换成你要部署的域名）：

```
make namespace NAMESPACE=example HOSTNAME=img.chevereto.dev
```

## 启动 Chevereto

`make spawn`通过传递 NAMESPACE 选择项来创建实例运行命令。

```
make spawn NAMESPACE=example
```

🎉恭喜，Chevereto现在已经启动。

要创建更多实例，请为您要生成的每个其他网站重新[设置命名空间](#setup-namespace)中的步骤。
