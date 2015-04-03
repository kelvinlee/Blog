需求说明
---
买了 2 台阿里云 ECS, 用其中一台设置 1M 带宽, 然后另外一台 0 带宽,其中 S(主机有带宽)开启 squid, C(客户机 0 带宽同局域网) 代理上网. (就是为了省钱)


服务器设置
---

首先安装 
```
$ yum install squid 
```
根据需要配置 conf (我用的阿里云直接 yum 出来的不用配置)
```
$ vim /etc/squid/squid.conf
```
启动服务
```
$ service squid start
```


客户端设置
---

里面的星号换成对应的 主机(也就是能上网的那台 VPS)的IP
```
$ vim /etc/profile
```

```
http_proxy=http://*.*.*.*:3128
export http_proxy
```

```
$ source /etc/profile
```

然后客户端就可以上网了.