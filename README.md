## Surge配置详解

基于`ZHUANGZHUANG`由 [@LABKNS](https://github.com/LABKNS) 编辑，自用:</strong><code>Surfboard</code> </strong><code>Clash</code>



* [General](#General)
* [Replica](#Replica)
* [Proxy ](#Proxy)
* [Proxy Group](#Proxy-Group)
* [Rule](#Rule)
* [Host](#Host)
* [URL Rewrite](#URL-Rewrite)

## General
日志等级: warning, notify, info, verbose (默认值: notify)
<pre><code> loglevel = notify </code></pre>
跳过某个域名或者 IP 段, 这些目标主机将不会由 Surge Proxy 处理 (macOS 版本中, 如果启用了 Set as System Proxy, 这些值会被写入到系统网络代理设置)
<pre><code> skip-proxy = 127.0.0.1, 192.168.0.0/16, 193.168.0.0/24, 10.0.0.0/8, 172.16.0.0/12, 100.64.0.0/10, localhost, *.local </code></pre>
强制使用特定的 DNS 服务器
<pre><code> dns-server = 223.5.5.5, 119.29.29.29, 114.114.114.114, 8.8.8.8, system </code></pre>
允许外部控制器访问 Surge, Surge Dashboard 或 Surge CLI 进行管理控制
<pre><code> external-controller-access = passw@127.0.0.1:6170 </code></pre>
是否启动完整的 IPv6 支持 (默认值: false)
<pre><code> ipv6 = false </code></pre>
TUN规则匹配模式 (默认值: false)
<pre><code>enhanced-mode-by-rule = 1 </code></pre>
拒绝页面显示错误
<pre><code> show-error-page-for-reject = true </code></pre>
不包括简单主机名
<pre><code> exclude-simple-hostnames = true </code></pre>
Surge 作为 HTTP/SOCKS5 代理服务器向 Wi-Fi 网络下的其他设备提供服务器
<pre><code> allow-wifi-access = true </code></pre>
* Surge Mac 供外网访问的服务端口
  >HTTP 代理服务端口 (默认值: 6152)
  <pre><code> http-listen = 0.0.0.0:8888 </code></pre>
  >SOCKS5 代理服务端口 (默认值: 6153)
  <pre><code> socks5-listen = 0.0.0.0:8889 </code></pre>
* Surge iOS 供外网访问的服务端口
  >HTTP 代理服务端口 (默认值: 6152)
  <pre><code> wifi-access-http-port = 8888 </code></pre>
  >SOCKS5 代理服务端口 (默认值: 6153)
  <pre><code> wifi-access-socks5-port = 8889 </code></pre>
兼容模式（默认禁用）
* 禁用
  <pre><code> compatibility-mode = 0 </code></pre>
* Proxy with Loopback Address
  <pre><code> compatibility-mode = 1 </code></pre>
* Proxy Only
  <pre><code> compatibility-mode = 2 </code></pre>
* TUN Only  
  <pre><code> compatibility-mode = 3 </code></pre>
启用 Network.framework (默认值: false) 
<pre><code> network-framework = true </code></pre>
INTERNET 测试 URL (使用网络诊断功能时访问的 URL)
<pre><code> internet-test-url = http://wwww.gstatick.com/generate_204 </code></pre>
代理测速 URL (测试代理策略时的 URL)
<pre><code> proxy-test-url = http://wwww.gstatick.com/generate_204 </code></pre>
测速超时 (秒)
<pre><code> test-timeout = 5 </code></pre>

## Replica
`该段定义抓取流量的过滤`</p>
隐藏所有发往 *.Apple.com 和 *.icloud.com 的请求（该选项只是在抓取结果中隐藏了请求）
<pre><code> hide-apple-request = true </code></pre>
隐藏Crashlytics请求
<pre><code> hide-crashlytics-request = true </code></pre>
隐藏UDP会话（默认值: false）
<pre><code> hide-udp = false </code></pre>
使用关键词过滤器（默认值: false）
<pre><code> use-keyword-filter = false </code></pre>
* 仅记录不包含关键字的请求
  <pre><code> keyword-filter-type = blacklist </code></pre>
* 仅记录包含关键字的请求
  <pre><code> keyword-filter-type = whitelist </code></pre>
关键字 (例：abc def)
<pre><code> keyword-filter = abc,def </code></pre>

## Proxy
`该段定义可用的代理策略`</p>
策略名不可重复, 策略名须先定义才能在其它部分引用</p>
> 写法是：策略名 = 代理类型, 代理地址, 端口号,加密方式, 用户名, 密码
<pre><code>ss_obfs = custom, 1.1.1.1, 80, chacha20-ietf-poly1305, password
ss = custom, 1.1.1.1, 80, chacha20-ietf-poly1305, password
socks5 = socks5, 1.1.1.1, username, password
http = http, 1.1.1.1, username, password
https = https, 1.1.1.1, username, password </code></pre>
利用服务器定义的方式实现的广告通过选择
> Ad-Pass 不拦截广告, Ad-Block 直接拒绝, Ad-GIF 返回一个透明像素图
<pre><code>Ad-Pass = direct
Ad-Block = reject
Ad-GIF = reject-tinygif  </code></pre>

## Proxy Group
`该段定义可用的策略组`</p>
__有 4 种策略组类型: “select”, “url-test”, “fallback” 和 “ssid”__</p>
* select: 具体哪个子策略将被使用, 由用户界面上进行选择</p>
  > 手动选择：Auto, Proxy01, Proxy02, Proxy03
  <pre><code> Proxy = select, Auto, Proxy01 , Proxy02, Proxy03 </code></pre>
* ssid: 具体哪个子策略将被使用, 根据 Wi-FI 的 SSID 决定</p>
  > 根据 Wi-FI 的 SSID 决定：默认策略 Auto, 数据网络策略 ProxyA, 连接到 123 的 Wi-FI 网络策略 ProxyB, 连接到 456 的 Wi-FI 网络策略 ProxyC
  <pre><code> Scene = ssid, default = Auto, cellular = ProxyA, “123” = ProxyB, “456” = ProxyC </code></pre>
* url-test: 具体哪个子策略将被使用, 通过测试到具体 URL 的访问速度选择
  > interval 单位秒，指定间隔多长时间后需要重新发起测试，默认值：600，interval 并不是指每隔多少秒就会发起测试，而是只有超过了这个时间才会重新发起测试。若使用该 group 的 rule 一直没有触发，那么并不会引发再次测试</p>
timeout 单位秒，每次测试最长持续时间，默认值：5
tolerance 单位毫秒，只有当 原来优胜者的成绩 - 新的优胜者的成绩 > tolerance 时，才会进行线路更换，避免因为较小线路波动频繁引发线路变换。默认值：200
  <pre><code> AutoGroup = url-test, ProxyA, ProxyB, ProxyC, url = http://www.gstatic.com/generate_204, interval = 600, tolerance = 200, timeout = 5 </code></pre>
* fallback: 具体哪个子策略将被使用, 通过测试到具体 URL 的可用性决定</p>
  > 包含策略 Proxy01, Proxy02, Proxy03, 测试 url 为 http://www.bing.com, 600s后上次的测试结果将被抛弃,重新测试 如果比原线路的响应时间, 好100ms以上的时候, 触发线路变更, 如果某策略在5s后依然没有完成, 放弃该策略。 选出延迟最低的策略
  <pre><code> Auto = url-test, Proxy01, Proxy02, Proxy03, url = http://www.bing.com/, interval = 600s, tolerance = 100ms, timeout = 5s </code></pre>
以代理服务器的选择模式实现广告的通过选择
<pre><code> AdBlock = select, Ad-GIF, Ad-Block, Ad-Pass </code></pre>

## Rule


| 类型 | 值 | 策略 | Surfboard | 
| --- | --- | --- | --- |  
| DOMAIN, | www.apple.com, | DIRECT | --- | 
| DOMAIN-SUFFIX, | apple.com, | DIRECT | --- | 
| DOMAIN-KEYWORD, | apple, | DIRECT | --- | 
| IP-CIDR, | 10.0.0.0/8, | DIRECT | --- | 
| GEOIP, | CN, | DIRECT | --- | 
| USER-AGENT, | Instagram*, | Proxy | NO |
| URL-REGEX, | ^http:\/\/google\.com, | Proxy | NO |
| PROCESS-NAME, | Telegram, | Proxy | BETA |
| RULE-SET, | SYSTEM, | DIRECT | --- |
| AND, | ((DOMAIN, abc.com), (USER-AGENT, Surge*)), | DIRECT | --- |  
| OR, | ((DOMAIN, abc.com), (USER-AGENT, Surge*)), | DIRECT | --- |  
| NOT, | ((DOMAIN, abc.com)), | Proxy | --- |  
| DEST-PORT, | 80, | DIRECT | --- |  
| SRC-IP, | 192.168.20.100, | DIRECT | --- |  
| IN-PORT, | 6152, | DIRECT | --- |  
| FINAL, | --- | Proxy | --- |  

__有三种基于域名的规则: “DOMAIN”, “DOMAIN-SUFFIX” 和 “DOMAIN-KEYWORD”__
* 如果请求域完全匹配, 则匹配规则
  <pre><code> DOMAIN, www.apple.com, DIRECT </code></pre>
* 如果请求的域与后缀匹配, 则匹配规则。例如:google.com匹配www.google.com, mail.google.com和google.com, 但不匹配content-google.com
  <pre><code> DOMAIN-SUFFIX, google.com, DIRECT </code></pre>
* 如果请求的域包含关键字, 则匹配规则
  <pre><code> DOMAIN-KEYWORD, apple, DIRECT </code></pre>
* 参数: force-remote-dns (默认值: false) 如果某请求被该规则匹配, 且策略不是DIRECT. 那么 DNS 查询将永远在远端代理服务器执行, 即使该请求由 Surge TUN 处理
  <pre><code> DOMAIN-KEYWORD, google, ProxyHTTP, force-remote-dns </code></pre>
__有两种基于IP的规则: “IP-CIDR” , “GEOIP”__
* 如果是一个使用域名进行访问的请求, 那么 Surge 将进行 DNS 查询以确认是否应该被该规则匹配. 若 DNS 查询失败, 将放弃规则匹配过程并直接给出错误
  <pre><code> IP-CIDR, 10.0.0.0/8, DIRECT </code></pre>
* OPTIONS: no-resolve:(默认值: false) 如果是一个使用域名进行访问的请求, 跳过该条规则, 不触发 DNS 查询
  <pre><code> IP-CIDR, 192.168.0.0/16, DIRECT, no-resolve </code></pre>
* GeoIP CN, 基于 GeoIP 数据库判断域名和 IP 的归属地
  <pre><code> GEOIP, CN, DIRECT </code></pre>
__有两种HTTP规则: “USER-AGENT”, “URL-REGEX”__</p>
HTTP规则用于HTTP请求或HTTPS请求。它不会影响TCP连接
* </strong><code>NO</code>    USER-AGENT, 如果请求的用户代理匹配, 则匹配规则。通配符*和?都受支持
  <pre><code> USER-AGENT, Instagram*, DIRECT </code></pre>
* </strong><code>NO</code>    URL-REGEX, 如果URL与正则表达式匹配, 则匹配规则
  <pre><code> URL-REGEX, ^http://google\.com, DIRECT </code></pre>
* </strong><code>BETA</code>    PROCESS-NAME, 可以为指定的进程分配策略
  <pre><code> PROCESS-NAME, Telegram, Proxy </code></pre>
  
## RULESET规则集
规则集包含多条子规则, 可以是另一个本地 list 文件, 或者是一个远程 URL
* 内置了两个规则集：SYSTEM 和 LAN
  <pre><code>RULE-SET, SYSTEM, DIRECT
  RULE-SET, LAN, DIRECT </code></pre>
* 远程list 文件是一个纯文本文件, 每一行为一个规则, 最后不可写上策略名
  <pre><code> RULE-SET, URL, List, update-interval=300 </code></pre>
__逻辑规则三种：“AND”, “OR”和“NOT”__</p>
可以组合多个子规则, 且可进行多层嵌套, 用于某些复杂场景的判断
* AND 运算符表示所有子规则都匹配时, 使用该策略
  <pre><code> AND, ((#Rule1), (#Rule2), (#Rule3)...), Policy </code></pre>
* OR 运算符表示任何子规则匹配时, 使用该策略
  <pre><code> OR, ((#Rule1), (#Rule2), (#Rule3)...), Policy </code></pre>
* NOT 运算符表示子规则未匹配时, 使用该策略
  <pre><code> NOT, ((#Rule1)), Policy </code></pre>
__Miscellaneous规则三种:“DEST-PORT”, “SRC-IP”和“IN-PORT”__</p>  
* DEST-PORT 如果请求的目标端口匹配, 则规则匹配  
  <pre><code> DEST-PORT, 80, DIRECT </code></pre>
* SRC-IP 如果请求的客户端IP地址匹配, 则规则匹配。仅适用于远程机器
  <pre><code> SRC-IP, 192.168.20.100, DIRECT </code></pre>
* IN-PORT 如果请求的传入端口匹配, 则规则匹配。Surge在多个端口上监听时很有用
  <pre><code> IN-PORT, 6152, DIRECT </code></pre>

## Final规则
`FINAL规则必须在所有其他规则之后编写。它定义了与任何其他规则不匹配的请求的默认策略`
* DNS 查询失败走 Final 规则
  <pre><code> FINAL, Proxy, dns-failed </code></pre>
__触发通知__
* 匹配规则时弹出 notification-text 定义的字符串
  <pre><code> AND, ((DOMAIN, raw.githubusercontent.com), (USER-AGENT, Surge*)), DIRECT, notification-text=“规则集更新”, notification-interval=3 //更新提醒 </code></pre>

## Host
`该段定义本地 DNS 记录`</p>
该功能等同于 /etc/hosts, 加上了泛解析和别名支持
<pre><code>*.taobao.com = server:223.5.5.5
*.jd.com = server:223.5.5.5
*.tmall.com = server:223.5.5.5 </code></pre>

## URL Rewrite
`该段定义针对 HTTP 请求的 URL 重定向规则`</p>
__Header模式__
* Surge将修改Header, 并在必要时将请求重定向到另一台主机。客户端不会注意到这个重定向操作
  <pre><code> ^http://www\.google\.cn http://www.google.com header </code></pre>
__302模式__
* Surge只会返回302重定向响应。如果启用了主机名的MitM, 则可以重定向HTTPS请求
  <pre><code> ^http://yachen\.com https://yach.me 302 </code></pre>
__Reject模式__
* 如果模式匹配, 则拒绝请求。替换参数将被忽略。如果启用了主机名的MitM, 将拒绝HTTPS请求
  <pre><code> ^http://ad\.com/ad\.png _ reject </code></pre>
