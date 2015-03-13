# Web 应用服务器 - Apache, Nginx

近几年 Web 应用服务器市场可以说是 Nginx 一个人的舞台，凭借良好的性能表现、稳定可靠的发挥，Nginx 从 Lighttpd 等高性能 Web 应用服务器竞争者中脱颖而出，一路高歌猛进，短短几年时间就拿下了近百分之二十的市场份额。但是凭着历史积累下的庞大用户数，Apache 目前仍然是 [最广为使用的 Web 服务器（根据 NetCraft 2015 年 2 月数据，在全球访问量最大的一百万个网站中 Apache 市场占有率为 50%，二三名分别为 Nginx 21%、IIS 12%）](http://news.netcraft.com/archives/2015/02/24/february-2015-web-server-survey.html)，依然有大量历史长期项目以及更注重 Apache 丰富的功能且对性能要求不那么高的新项目选择并使用 Apache。

因为上述原因，在目前 Nginx 已逐步是多数互联网公司首选的情况下，我们仍包括了 Apache 的内容。

## Apache HTTP Server（简称 Apache）
[Apache](http://httpd.apache.org/) 是一个跨平台、采用模块化设计的 Web 服务器，由于其简单高效、稳定安全的特性，被广泛应用于计算机技术的各个领域。

### 一些 Apache 配置、模块

> **粗体部分** - 个人认为可多关注

* **URL 重写 - [mod_rewirte](http://httpd.apache.org/docs/2.4/rewrite/)**
> TODO: 内容待补充
* Options
    * 符号连接 - FollowSymLinks
     开启后，服务器将允许在此目录中使用符号连接，Apache 会检查每个请求中是否包含对符号链接的引用，对请求中包含的每个路径做一次 lstat() 系统调用。如考虑安全防护：永远不要允许使用符号链接；如考虑性能：永远使用 Options FollowSysLinks 且绝不使用 Options SysLinkIfOwnerMatch
    * 目录浏览 - Indexes, 建议关闭
* ServerRoot, DocumentRoot
* 网站默认首页文件 - DirectoryIndex
* **虚拟主机 - NameVirtualHost, ServerName, VirutalHost, ServerAlias**
* **.htaccess 支持 - AllowOverride, .htaccess**
 只在必要的目录中启用 AllowOverride（2.4 版本默认关闭）。.htaccess 文件可以极大便利 Apache 参数设置，而无需每次修改都要编辑 Apache 的配置文件（需要重启服务生效），但是也降低了服务器的性能。
* 保持连接 - KeepAlive, KeepAliveTimeout, MaxKeepAliveRequests
 Apache 的 KeepAlive 参数让服务器和客户端之间在指定一段时间保持同一个连接。这个特性有好处也有坏处，好处是如果客户端发出多个请求，服务端不必每次都花时间去创建连接，坏处是这段时间内即使客户端不再发出新的请求、访问新的页面，这个连接也会被占用，这对服务器资源来说是一种浪费。
* [**工作模式/多处理模块（MPM）**](http://httpd.apache.org/docs/2.4/mpm.html)：
    * 查看当前工作模式：`httpd -l`
    * [Prefork 模式](http://httpd.apache.org/docs/2.4/mod/prefork.html)
        - StartServers: 服务器启动时默认开启的进程数
        - MinSpareServers: 最小的空闲进程数
        - MaxSpareServers: 最大的空闲进程数
        - ServerLimit: 在Apache的生命周期内，限制MaxClients的最大值
        - MaxClients: 最大的并发请求数，最大值不能超过 ServerLimit 设置的值
        - MaxRequestsPerChild: 一个进程可以处理的最多的请求数（进程复用），如请求超过该设置则杀死进程，0表示永不过期。
    * [Worker 模式](http://httpd.apache.org/docs/2.4/mod/worker.html)
        - StartServers: 服务器启动时默认开启的进程数
        - MaxClients: 最大的并发请求数
        - MinSpareThreads: 最小的线程空闲数
        - MaxSpareThreads: 最大的线程空闲数
        - ThreadsPerChild: 每一个进程可以产生的线程数
        - MaxRequestsPerChild: 一个线程可以处理的最多的请求数（线程复用），如请求超过该设置则杀死线程，0表示永不过期。
    * [Event 模式](http://httpd.apache.org/docs/2.4/mod/event.html)
* 日志级别、路径、格式 - [LogLevel](http://httpd.apache.org/docs/2.4mod/core.html#loglevel), [ErrorLog](http://httpd.apache.org/docs/2.4/mod/core.html#errorlog), [CustomLog](http://httpd.apache.org/docs/2.4/mod/mod_log_config.html#customlog)
* 认证，授权与访问控制
    * Order, Deny, Allow, [Apache: Access Control](http://httpd.apache.org/docs/2.4/howto/access.html)
    * [Apache: Authentication and Authorization](http://httpd.apache.org/docs/2.4/howto/auth.html)
* SSL 支持 - [mod_ssl](http://httpd.apache.org/docs/2.4/mod/mod_ssl.html)
* 关闭不用的模块及功能 - LoadModule
* 显示版本信息及操作系统版本信息 - ServerSignature, ServerTokens
    * ServerTokens - 是否在错误页面、目录列表等页面的底部显示服务器操作系统信息
    * ServerSignature - HTTP 头中是否回显 Apache 版本信息
* 避免 DNS 查询 - HostnameLookups off
* 请求超时 - [Timeout](http://httpd.apache.org/docs/2.4/mod/core.html#timeout)
* 文件包含 - [Include](http://httpd.apache.org/docs/2.4/mod/core.html#include)
* 缓存 - [Cache Guide](http://httpd.apache.org/docs/2.4/caching.html)
* 代理 - Proxy, [mod_proxy](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html)

### 扩展阅读
* [**Apache: Frequently Asked Questions**](http://wiki.apache.org/httpd/FAQ) - 英文，Apache 官方问题汇总，篇幅有点长，不过还是建议过一遍
* [**Apache Cookbook 中文版**](http://book.douban.com/subject/3356185/)


## Nginx

Nginx ("engine x") 是一个高性能的 HTTP 和反向代理服务器，也是一个 IMAP/POP3/SMTP 邮件代理服务器 。 Nginx 是由 Igor Sysoev 为俄罗斯访问量第二的 Rambler.ru 站点开发的，自发布以来的很长一段时间里，Nginx 为 [Yandex](http://www.yandex.ru/), [Mail.Ru](http://mail.ru/), [VK](http://vk.com/), 以及 [Rambler](http://www.rambler.ru/) 等俄罗斯网站经受住了强大的访问考验。Igor 将源代码以类 BSD 许可证的形式（[2-clause BSD-like license](http://nginx.org/LICENSE)）发布。自 Nginx 发布四年来，Nginx 已经因为它的稳定性、丰富的功能集、 示例配置文件和低系统资源的消耗而闻名了。目前国内各大门户网站已经部署了 Nginx，如新浪、网易、腾讯等，国内几个重要的视频分享网站也部署了 Nginx，如六房间、酷6等。 新近发现Nginx 技术在国内日趋火热，越来越多的网站开始部署 Nginx。

### 扩展阅读
* Tenginx


## Web 服务器配置实践

* [最小权限原则](http://en.wikipedia.org/wiki/Principle_of_least_privilege) 设置文件、文件夹权限
    * chmod, chown
    * rwx? 755？ - [Linux 文件系统权限 - 英文](http://en.wikipedia.org/wiki/File_system_permissions#Notation_of_traditional_Unix_permissions)
* 单独建立运行用户及用户组
    * Apache - [User](http://httpd.apache.org/docs/current/mod/mod_unixd.html#user), [Group](http://httpd.apache.org/docs/current/mod/mod_unixd.html#group)
    * Nginx - [user](http://nginx.org/en/docs/ngx_core_module.html#user)
* 合理使用浏览器缓存 - Expires, Etag, Cache-Control, Last-Modified
 浏览器缓存（Browser Caching）是为了节约网络的资源、加快浏览速度，浏览器在用户磁盘上对最近请求过的文档进行存储，当访问者再次请求这个页面时，浏览器尝试直接从本地磁盘获取文档内容，加速页面访问速度的一种方法。

 浏览器缓存主要有两种方式，
    * 缓存协商 - Etag, Last-modifie
     缓存协商的方式去服务器端询问页面有没有修改过，没有修改则返回 304 直接使用缓存内容，否则返回新内容
    * 彻底缓存 - Cache-Control，Expires
     彻底缓存的方式在缓存失效之前不再跟服务器交互，直接使用缓存内容

 您可以参考这篇文章了解到更多信息 - [Heroku: Increasing Application Performance with HTTP Cache Headers](https://devcenter.heroku.com/articles/increasing-application-performance-with-http-cache-headers#cache-prevention)
* 使用压缩 - 节省网络带宽
    * Apache - [mod_deflate](http://httpd.apache.org/docs/2.4/mod/mod_deflate.html), 
    * Nginx - [gzip](http://nginx.org/en/docs/http/ngx_http_gzip_module.html)
* 配置文件推荐目录结构

    ```sh
    mkdir ./site-available ./site-enabled
    echo 在 site-available 中写好网站配置文件
    cd ../site-enabled
    ln -s ../site-available/xxx.conf
    ```

* 计算进程平均占用内存 (kb) - 以便决定服务器需要多少内存空间
 以 Apache 为例

    ```sh
    ps aux | grep -v grep | awk '/sbin\/httpd/ {sum+=$6;n++}; END {print sum/n}'
    ```
