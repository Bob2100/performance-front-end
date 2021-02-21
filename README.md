目录

<!-- TOC -->

- [1. 什么是性能优化](#1-什么是性能优化)
- [2. DNS](#2-dns)
  - [2.1. 优化](#21-优化)
- [3. TCP](#3-tcp)
  - [HTTP](#http)

<!-- /TOC -->

# 1. 什么是性能优化

简单地说就是使应用更快，包括：

1. 加载文件更快
2. 运行更快

从浏览器输入 url 到页面加载完成，发生了什么？

1. 用户输入 xxx.com
2. 浏览器通过 DNS 把 URL 解析为 IP
3. 和 IP 地址建立 TCP 连接，发送 HTTP 请求
4. 服务器接收请求，查数据库、读取文件等，拼接好返回的 HTTP 响应
5. 浏览器收到首屏 html，并开始渲染
6. 解析 html 为 dom
7. 解析 css 为 css-tree
8. dom+css 生成 render-tree 树
9. 加载 script 的 js 文件
10. 执行 js

# 2. DNS

domain name service
DNS 基于什么协议？
DNS 服务器 把 域名 xxx.com 解析为 xx.xx.xx.xxx IP 返回给浏览器，浏览器缓存这些 IP

DNS 查找过程，先查本地，层层查找

## 2.1. 优化

1. DNS 预解析

   > 一些 link 标签，不用每次都解析 DNS

   <link rel="dns-prefetch" href="xxx.xxx.com">

   解析 html 时发现域名就会先把 dns 解析做了。下次使用时就不用在解析域名了

   看下淘宝的例子

# 3. TCP

IP TCP HTTP 的关系

1. IP 负责找到
2. TCP 负责数据的完整性和有序性，三次握手、粘包，滑动窗口等机制

TCP 与 UDP

UDP 只管发送，允许丢包
性能高
包本身小，不需要拆分数据
，重发机制好写，dns 发送 3. http 应用层

优化策略：

1. 长连接
2. 减少文件体积
   js 打包压缩
   图片压缩
   gzip
3. 减少文件请求次数
   雪碧图
   js,css 打包合并
   缓存控制
   懒加载
4. 减少用户和服务器的距离
   cnd
5. 本地存储

文件多大比较好，越大越好吗？
比如 vue.js 专门拆离，如果达成一个包，稍微修改，浏览器缓存就失效，所以共用库拆成一个包，具体页面有些打包有些懒加载

前端代码如何上线？
部署：

1. 原始的时候，html.css，js 直接复制到 ngixn 目录下
   问题：图片 icon 太多，文件名没变，用户需要强刷
2. 现代：
   webpack 雪碧图，文件名修改 index.asdf3223.js，gzip,nginx 层面的负载均衡

网站图片加载慢？图片格式，懒加载，cdn

## HTTP

书籍《图解 HTTP 协议》
浏览器缓存机制
http cache,
service worker cache,
memory cache,
push cache

代码层面的优化

雅虎军规：
尽量减少 HTTP 请求的个数——需要权衡，
使用 CDN（内容分发网络），
为文件指定 Expire 或 Cache-Control,使内容具有缓存性，
避免空的 src 和 href,
使用 gzip 压缩内容,
css 放顶部，js 放底部，
css js 放到外部文件中,
减少 dns 查找次数，
精简 css 和 js,
避免跳转，
剔除重复的 js 和 css，
配置 ETags，
使 Ajex 可缓存,尽早刷新输出缓存，
使用 get 来完成 ajex 请求，
延迟加载，预加载，
减少 DOM 元素的个数，
根据域名划分页面内容，
尽量减少 iframe 的个数，
避免使用滤镜，优化图像，优化 css spirite,
不要在 HTML 中缩放图像——需权衡，
favicon.ico 要小而且可缓存，
保持单个内容小于 25K,
打包组件成复合文本
