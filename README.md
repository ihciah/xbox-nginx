# xbox-nginx
加速 xbox 国内下载

## 说明
xbox 在国内默认从 `assets1.xboxlive.com` 下载游戏，该域名的 DNS 解析一塌糊涂哪的都有，所以一个办法是做测速并强制解析。但即便是这样也很慢。

还有一个域名是 `assets1.xboxlive.cn`。这个域名可以解析到多个国内 CDN，速度非常 OK 了。一个简单的 idea 是直接把 `assets1.xboxlive.com` 强制解析到 cn 域名对应的 IP，但我挑了几个国内 IP 带着 com 的 Host 去 curl，都 403 了。查了下很多人也反馈这种把 com 强制解析到 cn 的行为有时候会出现速度掉零的问题，猜测是部分 CDN 厂商返回了 403，少部分还是可用的。

但是如果带着 cn 的 Host 去请求，一模一样的 path 是可以正确拿到资源的。所以我们只需要启动一个简单的 nginx，将 com 301 到 cn 即可。当然，cn 的解析结果你也可以做手动测速并强制解析，不属于本文内容了（dnsmasq 或 adguardhome 等可以很方便地设置，直接改 hosts 也可以）。

## 环境依赖
1. 安装 docker 和 docker-compose。
2. 80 端口可用。

## 如何配置
1. 在内网机器（公网也行）上下载本项目后，在文件夹内执行 `docker-compose up -d`。
2. 强制将 `assets1.xboxlive.com` 解析过来。
3. 如果你想手选国内节点，强制解析 `assets1.xboxlive.cn` 即可。
