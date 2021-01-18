HTTP 反向代理

一些对安全性要求比较高的站点，可能会使用 HTTPS（一种使用 ssl 通信标准的安全 HTTP 协议）。

这里不科普 HTTP 协议和 SSL 标准。但是，使用 Nginx 配置 https 需要知道几点：

HTTPS 的固定端口号是 443，不同于 HTTP 的 80 端口

SSL 标准需要引入安全证书，所以在 nginx.conf 中你需要指定证书和它对应的 key

其他和 HTTP 反向代理基本一样，只是在 Server 部分配置有些不同。

#HTTP 服务器
  server {
      #监听 443 端口。443 为知名端口号，主要用于 HTTPS 协议
      listen       443 ssl;

      #定义使用 www.xx.com 访问
      server_name  www.helloworld.com;

      #ssl 证书文件位置（常见证书文件格式为：crt/pem）
      ssl_certificate      cert.pem;
      #ssl 证书 key 位置
      ssl_certificate_key  cert.key;

      #ssl 配置参数（选择性配置）
      ssl_session_cache    shared:SSL:1m;
      ssl_session_timeout  5m;
      #数字签名，此处使用 MD5
      ssl_ciphers  HIGH:!aNULL:!MD5;
      ssl_prefer_server_ciphers  on;

      location / {
          root   /root;
          index  index.html index.htm;
      }
  }