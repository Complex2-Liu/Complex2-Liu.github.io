---
title: "Host your website on Vultr"
date: 2021-07-09T14:27:14+08:00
draft: true
---

本文将为详细讲述如何给自己搭建一个 Homepage.

## Deploy a cloud server

首先第一步, 我们要购买一个 cloud server, 个人建议从 [vultr](https://www.vultr.com/) 购买.
进入到 Deploy 页面后, 我们会有很多选择:

  - Choose Server: 选 Cloud Compute, 因为这个价格最便宜.
  - Server Location: 选 New York(NJ), 因为只有纽约地区拥有 3.5 美元一月的服务.
  - Server Type: 个人建议选 Debian 作为操作系统.
  - Server Size: 选 3.5 美元 per month 那个最便宜的套餐.
  - Additional Features: 勾选 Enable IPv6, 因为在未来 IPv6 会慢慢开始普及.
  - Server Hostname & Label: 我输入了自己在下一步购买的 domain name.

最后点击 `Deploy Now`.

<img src="/web-hosting/vultr-deploy-success.png" alt="vultr-deploy-success" class="img-center">


## Domain name and its DNS records

我们需要拥有一个 domain name(域名), 个人建议从 [namesilo](https://www.namesilo.com/) 购买,
它提供的 `.com` 域名价格为 8.99 美元一年, 算比较便宜.
登入自己的账户后, 我们会看到如下一个风格比较「简陋和传统」的 account home page

<img src="/web-hosting/namesilo-account-home.png" alt="namesilo-account-home" class="img-center">

点击 `domain manager`, 然后进入自己的域名, 然后再点击 `DNS Records:` 一项中的 `Update`.
现在我们需要添加 4 个 A Records, 具体来说, 是 2 个 Ipv4 A Records, 2 个 Ipv6 AAAA Records.

<img src="/web-hosting/update-dns-records.png" alt="update-dns-records" class="img-center">

其中你的 Ipv6 地址可以在这里找到:

<img src="/web-hosting/find-your-ipv6-address.png" alt="find-your-ipv6-address" class="img-center">

## Access your cloud server by using ssh

Typing
```bash
ssh root@complex2math.com
```
into your terminal to access your cloud server. Press `ctrl` + `d` to logout. 
We will do some general stuff first. Run
```bash
apt update
```
to update our packages. Then run
```bash
apt upgrade
```
to upgrade those upgradable packages.
You can also edit the `.bashrc`, say add some alias, to accommodate your need.

### Install nginx

[Nginx](https://www.nginx.com/) is what we are going to use as a web server.
```bash
apt install nginx python-certbot-nginx rsync
```
其中 `python-certbot-nginx` 能够给我们的网页提供 https,
`rsync` 是一个常用的 Linux 应用程序, 用于文件同步.

目录 `/etc/nginx/sites-available/` 包含了你的 server host 的所有网页,
如果你需要激活某个网页, 只需把它 `ln -s` 到目录 `/etc/nginx/sites-enabled`.
```bash
cp /etc/nginx/sites-available/default /etc/nginx/sites-available/complex2math
vim /etc/nginx/sites-available/complex2math
```
把 `listen 80` 以及 `listen [::]:80` 后面的两个 `default_server` 都删除掉.
将 `root /var/www/html` 修改成 `root /var/www/complex2math`,
将 `server_name` 一行修改成 `server_name complex2math.com www.complex2math.com`.
```bash
ln -s /etc/nginx/sites-available/complex2math /etc/nginx/sites-enabled/
s reload nginx
```
这样我们便重新启动了 nginx 服务.
```bash
mkdir /var/www/complex2math
vim /var/www/complex2math/index.html
```






