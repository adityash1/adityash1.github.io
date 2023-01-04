---
author: adityash1
title: "Using HTTPS on Local Environment with Nginx"
date: 2023-01-02T17:30:10+05:30
tags: [
    "nginx",
    "ssl", 
    "web development"
]
categories: [
    "webdev",
    "tooling",
]
---

Today, Most of the sites we use are protected with HTTPS protocol for better security. Generally, we use HTTP in our local development environment, and it works for most cases, but if we want to test things how they would be like in the production (environment parity) or wanted to run a project which requires SSL then we should use this.

There are more advantages to it. Reading about different protocols and computer networking will be very beneficial while developing software. I recommend reading this [article](https://www.smashingmagazine.com/2021/08/http3-core-concepts-part1/) if you want to understand all core concepts about HTTP.

First, We'll generate SSL certificate using [mkcert](https://github.com/FiloSottile/mkcert):-

```bash
brew install mkcert
```

```bash
mkcert -install
```

```bash
mkcert -key-file <key-name>.pem -cert-file <cert-name>.pem localhost
```

Now we want to proxy our traffic through [nginx](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages). Install it using:-

```bash
sudo apt update
```

```bash
sudo apt install nginx
```

For starting nginx we'll use -

```bash
sudo systemctl start nginx
```

Now create a conf file (etc/nginx/conf) (e.g., sample.conf) and paste the following:-

```nginx
server {
  server_name localhost;
  listen 80;
  listen 443 ssl;

  # SSL certificate configuration 
   ssl_certificate ~/cert.pem; 
   ssl_certificate_key ~/key.pem; 

  location / {
    proxy_pass http://localhost:3000/;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "Upgrade";
    proxy_set_header Host $host;
  }
}
```

Restart Nginx :-

```bash
sudo systemctl restart nginx
```

Now you can open https://localhost

