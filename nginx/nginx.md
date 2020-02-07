# nginx 是什么？

nginx 是俄罗斯人 Igor Sysoev 为俄罗斯访问量第二的 Rambler.ru 站点开发的一个十分轻量级的 HTTP 服务器。它是一个高性能的 HTTP 和反向代理服务器，同时也可以作为 IMAP/POP3/SMTP 的代理服务器。nginx 使用的是 BSD 许可。

Nginx 以事件驱动的方式编写，所以有非常好的性能，同时也是一个非常高效的反向代理、负载平衡。

Nginx 因为它的稳定性、丰富的模块库、灵活的配置和低系统资源的消耗而闻名。

Nginx 适合用来做 mongrel clusters 的前端 HTTP 响应。

## Nginx 负载均衡的实现 - Nginx 负载均衡的实现

Nginx 是一款可以通过反向代理实现负载均衡的服务器，使用 Nginx 服务实现负载均衡的时候，用户的访问首先会访问到 Nginx 服务器，然后 Nginx 服务器再从服务器集群表中选择压力较小的服务器，然后将该访问谓求引向该服务器。若服务器集群中的某个服务器崩溃，那么从特选服务器列表中将该服务器删除，也就是说一个服多器假如崩溃了，那么 Nginx 就肯定不会将访问请求引入该服务器了。那么下面，我们通过实例来讲解一下 Nginx 负载均衡的实现。

## 什么是反向代理与负载均衡

- 什么是负载均衡一台服务器的单位时间内的访问量越大的时候，服务器的压力会越大。当一台服务器压力大得超过自身的承受能力的时候，服务器会崩溃。为了避免服务器崩溃，让用户有更好地体验，我们通常通过负载均衡的方式来分担服务器的压力。那么什么是负载均衡昵?是这样，我们可以建立很多很多个服务器，这些服务器组成一个服务器集群，然后，当用户访问我们网站的时候，先访问一个中间服务器，再让这个中间服务器在服务器集群中选择一个压力较小的服务器，然后将该访问谓求引入选择的服务器。这样，用户的每次访问，都会保证服务器集群中的每个服务器的压力趋于平衡，分担了服务器压力，避免了服务器崩溃的情况。

- 什么是反向代理我们有时候，用自己的计算机 A 想访问国外的某个网站 B，但是访问不了，此时，一台中间服务器 C 可以访问国外的网站 B，那么，我们可以用自己的电脑访问 C，通过 C 来访问 B 这个网站。那么这个时候，服务器 C 称为代理服务器，这种访问方式叫做正向代理。正向代理有一个特点，就是我们明确知道要访问哪个网站。再如，当我们有一个服务器集中，并且服务器集群中的每台服务器的内容一样的时候，同样我们要直接从个人电脑访问到服务器集中的服务器时候无法访问，且此时三方服务器能访问集群，这个时候，我们通过第三方服务器访问服务器集群的内容，但是此时我们并不知道是哪一台服务器提供的内容，此时的代理方式称为反向代理。

## 为什么要用 nginx，nginx 有什么特点？

nginx 的特点：

- 核心特点：高并发请求的同时保持高效的服务
- 热部署
- 低内存消耗
- 处理响应请求很快
- 具有很高的可靠性
- 同时，nginx 也可以实现高效的反向代理、负载均衡。

前端可以用 nginx 做些什么？

- 搭建静态资源服务器
- 反向代理分发后端服务（可以和 nodejs 搭配实现前后端分离）和跨域问题
- 根据 User Agent 来重定向站点
- 开发环境或测试环境切换（切换 host）
- url 重写，使用 rewrie 规则本地映射
- 资源内容篡改
- 获取 cookie 做分流
- 资源合并
- gzip 压缩
- 压缩图片
- sourceMap 调试
- 如何安装 nginx？

## nginx.conf 基本配置有哪些？

nginx 配置文件主要分成四个部分：

main，全局设置，影响其它部分所有设置
server，主机服务相关设置，主要用于指定虚拟主机域名、IP 和端口
location，URL 匹配特定位置后的设置，反向代理、内容篡改相关设置
upstream，上游服务器设置，负载均衡相关配置他们之间的关系式：server 继承 main，location 继承 server；upstream 既不会继承指令也不会被继承。

如下是一份通用的配置和详解：

```sh
#定义 Nginx 运行的用户和用户组,默认由 nobody 账号运行, windows 下面可以注释掉。
user nobody;

#nginx 进程数，建议设置为等于 CPU 总核心数。可以和 worker_cpu_affinity 配合
worker_processes 1;

#全局错误日志定义类型，[ debug | info | notice | warn | error | crit ]
#error_log logs/error.log;
#error_log logs/error.log notice;
#error_log logs/error.log info;

#进程文件，window 下可以注释掉
#pid logs/nginx.pid;

# 一个 nginx 进程打开的最多文件描述符(句柄)数目，理论值应该是最多打开文件数（系统的值 ulimit -n）与 nginx 进程数相除，

# 但是 nginx 分配请求并不均匀，所以建议与 ulimit -n 的值保持一致。

worker_rlimit_nofile 65535;

#工作模式与连接数上限
events {
  # 参考事件模型，use [ kqueue | rtsig | epoll | /dev/poll | select | poll ]; # epoll 模型是 Linux 2.6 以上版本内核中的高性能网络 I/O 模型，如果跑在 FreeBSD 上面，就用 kqueue 模型。
  #use epoll;
  #connections 20000; # 每个进程允许的最多连接数

  # 单个进程最大连接数（最大连接数=连接数\*进程数）该值受系统进程最大打开文件数限制，需要使用命令 ulimit -n 查看当前设置

  worker_connections 65535;
}

#设定 http 服务器
http {
  #文件扩展名与文件类型映射表
  #include 是个主模块指令，可以将配置文件拆分并引用，可以减少主配置文件的复杂度
  include mime.types; #默认文件类型
  default_type application/octet-stream;
  #charset utf-8; #默认编码

  #定义虚拟主机日志的格式
  #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
  #                  '$status $body_bytes_sent "$http_referer" '
  #                  '"$http_user_agent" "$http_x_forwarded_for"';

  #定义虚拟主机访问日志
  #access_log  logs/access.log  main;

  #开启高效文件传输模式，sendfile指令指定nginx是否调用sendfile函数来输出文件，对于普通应用设为 on，如果用来进行下载等应用磁盘IO重负载应用，可设置为off，以平衡磁盘与网络I/O处理速度，降低系统的负载。注意：如果图片显示不正常把这个改成off。
  sendfile        on;
  #autoindex on; #开启目录列表访问，合适下载服务器，默认关闭。

  #防止网络阻塞
  #tcp_nopush     on;

  #长连接超时时间，单位是秒，默认为0
  keepalive_timeout  65;

  # gzip压缩功能设置
  gzip on; #开启gzip压缩输出
  gzip_min_length 1k; #最小压缩文件大小
  gzip_buffers    4 16k; #压缩缓冲区
  gzip_http_version 1.0; #压缩版本（默认1.1，前端如果是squid2.5请使用1.0）
  gzip_comp_level 6; #压缩等级
  # 压缩类型，默认就已经包含text/html，所以下面就不用再写了，写上去也不会有问题，但是会有一个warn。
  gzip_types text/plain text/css text/javascript application/json application/javascript application/x-javascript application/xml;
  gzip_vary on; //和http头有关系，加个vary头，给代理服务器用的，有的浏览器支持压缩，有的不支持，所以避免浪费不支持的也压缩，所以根据客户端的HTTP头来判断，是否需要压缩
  # limit_zone crawler $binary_remote_addr 10m; #开启限制IP连接数的时候需要使用

  # http_proxy服务全局设置
  client_max_body_size   10m;
  client_body_buffer_size   128k;
  proxy_connect_timeout   75;
  proxy_send_timeout   75;
  proxy_read_timeout   75;
  proxy_buffer_size   4k;
  proxy_buffers   4 32k;
  proxy_busy_buffers_size   64k;
  proxy_temp_file_write_size  64k;
  proxy_temp_path   /usr/local/nginx/proxy_temp 1 2;

  # 设定负载均衡后台服务器列表

  upstream  backend.com  {
    # ip_hash; # 指定支持的调度算法
    # upstream 的负载均衡，weight 是权重，可以根据机器配置定义权重。weigth 参数表示权值，权值越高被分配到的几率越大。
    server   192.168.10.100:8080 max_fails=2 fail_timeout=30s;
    server   192.168.10.101:8080 max_fails=2 fail_timeout=30s;
  }

  #虚拟主机的配置
  server {
    #监听端口
    listen       80;
    #域名可以有多个，用空格隔开
    server_name  localhost fontend.com;
    # Server Side Include，通常称为服务器端嵌入
    #ssi on;
    #默认编码
    #charset utf-8;
    #定义本虚拟主机的访问日志
    #access_log  logs/host.access.log  main;

    # 因为所有的地址都以 / 开头，所以这条规则将匹配到所有请求
    location / {
      root   html;
      index  index.html index.htm;
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
      root   html;
    }

    # 图片缓存时间设置
    location ~ .*.(gif|jpg|jpeg|png|bmp|swf)$ {
      expires 10d;
    }

    # JS和CSS缓存时间设置
    location ~ .*.(js|css)?$ {
      expires 1h;
    }

    # 代理配置
    # proxy the PHP scripts to Apache listening on 127.0.0.1:80
    # location /proxy/ {
    #   proxy_pass   http://127.0.0.1;
    # }

    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000

    # location ~ \.php$ {
    #   root           html;
    #   fastcgi_pass   127.0.0.1:9000;
    #   fastcgi_index  index.php;
    #   fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
    #   include        fastcgi_params;
    # }

    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one

    # location ~ /\.ht {
    #   deny  all;
    # }
  }

  # another virtual host using mix of IP-, name-, and port-based configuration
  #
  # server {
  #   listen       8000;
  #   listen       somename:8080;
  #   server_name  somename  alias  another.alias;

  #   location / {
  #     root   html;
  #     index  index.html index.htm;
  #   }
  # }

  # HTTPS server
  #
  # server {
  #   listen       443 ssl;
  #   server_name  localhost;

  #   ssl_certificate      cert.pem;
  #   ssl_certificate_key  cert.key;

  #   ssl_session_cache    shared:SSL:1m;
  #   ssl_session_timeout  5m;

  #   ssl_ciphers  HIGH:!aNULL:!MD5;
  #   ssl_prefer_server_ciphers  on;

  #   location / {
  #     root   html;
  #     index  index.html index.htm;
  #   }
  # }

}
```

### location 如何匹配？

示例：

```sh
location = / {

# 精确匹配 / ，主机名后面不能带任何字符串

  [ configuration A ]
}

location / {

# 因为所有的地址都以 / 开头，所以这条规则将匹配到所有请求
# 但是正则和最长字符串会优先匹配

  [ configuration B ]
}

location /documents/ {

# 匹配任何以 /documents/ 开头的地址，匹配符合以后，还要继续往下搜索
# 只有后面的正则表达式没有匹配到时，这一条才会采用这一条

  [ configuration C ]
}

location ~ /documents/Abc {

# 匹配任何以 /documents/ 开头的地址，匹配符合以后，还要继续往下搜索
# 只有后面的正则表达式没有匹配到时，这一条才会采用这一条

  [ configuration CC ]
}

location ^~ /images/ {

# 匹配任何以 /images/ 开头的地址，匹配符合以后，停止往下搜索正则，采用这一条。

  [ configuration D ]
}

location ~\* \.(gif|jpg|jpeg)$ {

# 匹配所有以 gif,jpg 或 jpeg 结尾的请求
# 然而，所有请求 /images/ 下的图片会被 config D 处理，因为 ^~ 到达不了这一条正则

  [ configuration E ]
}

location /images/ {

# 字符匹配到 /images/，继续往下，会发现 ^~ 存在

  [ configuration F ]
}

location /images/abc {

# 最长字符匹配到 /images/abc，继续往下，会发现 ^~ 存在
# F 与 G 的放置顺序是没有关系的

  [ configuration G ]
}

location ~ /images/abc/ {

# 只有去掉 config D 才有效：先最长匹配 config G 开头的地址，继续往下搜索，匹配到这一条正则，采用

  [ configuration H ]

}

location ~_ /js/._/\.js
```

- 以=开头表示精确匹配
- ^~ 开头表示 uri 以某个常规字符串开头，不是正则匹配
- ~ 开头表示区分大小写的正则匹配;
- ~\_ 开头表示不区分大小写的正则匹配
- / 通用匹配, 如果没有其它匹配,任何请求都会匹配到优先级：
- (location =) > (location 完整路径) > (location ^~ 路径) > (location ~,~\_ 正则顺序) > (location 部分起始路径) > (/)

### 如何配置反向代理？

详解：

```sh
# 对 “/” 启用反向代理
location / {
  proxy_pass http://127.0.0.1:3000; # 设置要代理的 uri，注意最后的 /。可以是 Unix 域套接字路径，也可以是正则表达式。
  proxy_redirect off; # 设置后端服务器“Location”响应头和“Refresh”响应头的替换文本
  proxy_set_header X-Real-IP $remote_addr; # 获取用户的真实 IP 地址 #后端的 Web 服务器可以通过 X-Forwarded-For 获取用户真实 IP，多个 nginx 反代的情况下，例如 CDN。参见：http://gong1208.iteye.com/blog/1559835 和 http://bbs.linuxtone.org/thread-9050-1-1.html
  proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; #以下是一些反向代理的配置，可选。
  proxy_set_header Host $host; # 允许重新定义或者添加发往后端服务器的请求头。

  client_max_body_size 10m; #允许客户端请求的最大单文件字节数
  client_body_buffer_size 128k; #缓冲区代理缓冲用户端请求的最大字节数，
  proxy_connect_timeout 90; #nginx 跟后端服务器连接超时时间(代理连接超时)
  proxy_send_timeout 90; #后端服务器数据回传时间(代理发送超时)
  proxy_read_timeout 90; #连接成功后，后端服务器响应时间(代理接收超时)
  proxy_buffer_size 4k; #设置代理服务器（nginx）保存用户头信息的缓冲区大小
  proxy_buffers 4 32k; #proxy_buffers 缓冲区，网页平均在 32k 以下的设置
  proxy_busy_buffers_size 64k; #高负荷下缓冲大小（proxy_buffers\*2）
  proxy_temp_file_write_size 64k; #设定缓存文件夹大小，大于这个值，将从 upstream 服务器传
}
```

举例：

```sh
location ^~ /service/ {
  proxy_pass http://192.168.60.245:8080/;
  proxy_redirect default;
  proxy_set_header Host $host
  proxy_set_header X-Real-IP $remote_addr;
  proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
}
```

简化：

```sh
location /proxy/ {
  proxy_pass http://backend.com/;
  proxy_redirect default;
}
```

### 如何配置 rewrite？

rewrite 功能就是集合正则表达式和标志位实现 url 重写和重定向。rewrite 只能放在 server{}、location{}、if(){}块中，并且只能对域名后边的出去传递参数外的字符串起作用。如 URL：
http://microloan-sms-platform.yxapp.xyz/proxy/sms/task/querydeleted?page=1&pagesize=10
只对/proxy/sms/task/querydeleted 进行重写。

如果相对域名或参数字符串起作用，可以使用全局变量匹配，也可以使用 proxy_pass 反向代理。

表明看 rewrite 和 location 功能有点像，都能实现跳转，主要区别在于 rewrite 是在同一域名内更改获取资源的路径，而 location 是对一类路径做控制访问或反向代理，可以 proxy_pass 到其他机器。很多情况下 rewrite 也会写在 location 里，它们的执行顺序是：

- 执行 server 块的 rewrite 指令
- 执行 location 匹配
- 执行选定的 location 中的 rewrite 指令

如果其中某步 URI 被重写，则重新循环执行 1-3，直到找到真实存在的文件；循环超过 10 次，则返回 500 Internal Server Error 错误。

rewrite 规则后边，通常会带有 flag 标志位：

- last : 相当于 Apache 的[L]标记，表示完成 rewrite
- break : 停止执行当前虚拟主机的后续 rewrite 指令集
- redirect : 返回 302 临时重定向，地址栏会显示跳转后的地址
- permanent : 返回 301 永久重定向，地址栏会显示跳转后的地址

last 和 break 区别：

- last 一般写在 server 和 if 中，而 break 一般使用在 location 中
- last 不终止重写后的 url 匹配，即新的 url 会再从 server 走一遍匹配流程，而 break 终止重写后的匹配
- break 和 last 都能组织继续执行后面的 rewrite 指令

rewrite 常用正则：

- `.` ： 匹配除换行符以外的任意字符
- `?` ： 重复 0 次或 1 次

- `*` ： 重复 1 次或更多次

- `*` ： 重复 0 次或更多次
- `\d` ：匹配数字
- `^` ： 匹配字符串的开始
- `$` ： 匹配字符串的介绍
- `{n}` ： 重复 n 次
- `{n,}` ： 重复 n 次或更多次
- `[c]` ： 匹配单个字符 c
- `[a-z]` ： 匹配 a-z 小写字母的任意一个

可以使用 `()` 来进行分组，可以通过 `$1` 的形式来引用。

示例：

```sh
location /proxy/ {
  proxy_pass http://microloan-notification-web.yxapp.in;
  rewrite /proxy/(.\*)$ /$1 break;
}
```

### 如何配置负载均衡？

示例：

```sh
upstream test.net{
  ip_hash;
  server 192.168.11.1:80;
  server 192.168.11.11:80 down;
  server 192.168.11.123:8009 max_fails=3 fail_timeout=20s;
  server 192.168.11.1234:8080;
}
```

upstream 是 Nginx 的 HTTP Upstream 模块，这个模块通过一个简单的调度算法来实现客户端 IP 到后端服务器的负载均衡。
Nginx 的负载均衡模块目前支持 4 种调度算法：

- `轮询（默认）`：每个请求按时间顺序逐一分配到不同的后端服务器，如果后端某台服务器宕机，故障系统被自动剔除，使用户访问不受影响。Weight 指定轮询权值，Weight 值越大，分配到的访问机率越高，主要用于后端每个服务器性能不均的情况下。
- `ip_hash`：每个请求按访问 IP 的 hash 结果分配，这样来自同一个 IP 的访客固定访问一个后端服务器，有效解决了动态网页存在的 session 共享问题。
- `fair`：这是比上面两个更加智能的负载均衡算法。此种算法可以依据页面大小和加载时间长短智能地进行负载均衡，也就是根据后端服务器的响应时间来分配请求，响应时间短的优先分配。Nginx 本身是不支持 fair 的，如果需要使用这种调度算法，必须下载 Nginx 的 upstream_fair 模块。
- `url_hash`：此方法按访问 url 的 hash 结果来分配请求，使每个 url 定向到同一个后端服务器，可以进一步提高后端缓存服务器的效率。Nginx 本身是不支持 url_hash 的，如果需要使用这种调度算法，必须安装 Nginx 的 hash 软件包。

upstream 可以设定每个后端服务器在负载均衡调度中的状态，支持的状态参数:

- `down`：表示当前的 server 暂时不参与负载均衡
- `backup`：预留的备份机器。当其他所有的非 backup 机器出现故障或者忙的时候，才会请求 backup 机器，因此这台机器的压力最轻。
- `max_fails`：允许请求失败的次数，默认为 1。当超过最大次数时，返回 proxy_next_upstream 模块定义的错误。
- `fail_timeout`：在经历了 max_fails 次失败后，暂停服务的时间。max_fails 可以和 fail_timeout 一起使用。

注：当负载调度算法为 ip_hash 时，后端服务器在负载均衡调度中的状态不能是 weight 和 backup。

### 如何设置页面缓存？

页面缓存设置指令：

- `proxy_cache_path`：指定缓存的路径和一些其他参数，缓存的数据存储在文件中，并且使用代理 url 的哈希值作为关键字与文件名。

```sh
proxy_cache_path /data/nginx/cache/webserver levels=1:2 keys_zone=webserver:20m max_size=1g;
```

`levels` 参数指定缓存的子目录数。`keys_zone` 指定活动的 key 和元数据存储在共享池（webserver 为共享池名称，20m 位共享池大小），`inactive` 参数指定的时间内缓存的数据没有被请求则被删除，默认 `inactive` 为 10 分钟 `max_size` 指定缓存空间的大小。

- `proxy_cache` : 设置一个缓存区域的名称，一个相同的区域可以在不同的地方使用。
- `proxy_cache_valid` : 为不同的应答设置不同的缓存时间。

### 如何设置读写分离？

```sh
server {
  listen       80;
  server_name  localhost;
  #charset koi8-r;
  #access_log  logs/host.access.log  main;
  location / {
    proxy_pass http://192.128.133.202;
    if ($request_method = "PUT"){
      proxy_pass http://192.128.18.201;
    }
  }
}
```

[1]: [前端 nginx 使用札记](https://segmentfault.com/a/1190000013781162?utm_source=weekly&utm_medium=email&utm_campaign=email_weekly#articleHeader8)
