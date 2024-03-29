# 什么是 FastDFS

## 百度百科

地址：https://baike.baidu.com/item/fastdfs/5609710?fr=aladdin

FastDFS是一个开源的轻量级分布式文件系统，它对文件进行管理，功能包括：文件存储、文件同步、文件访问（文件上传、文件下载）等，解决了大容量存储和负载均衡的问题。特别适合以文件为载体的在线服务，如相册网站、视频网站等等。

FastDFS为互联网量身定制，充分考虑了冗余备份、负载均衡、线性扩容等机制，并注重高可用、高性能等指标，使用FastDFS很容易搭建一套高性能的文件服务器集群提供文件上传、下载等服务。

## 简介

FastDFS服务端有两个角色：跟踪器（tracker）和存储节点（storage）。跟踪器主要做调度工作，在访问上起负载均衡的作用。

存储节点存储文件，完成文件管理的所有功能：就是这样的存储、同步和提供存取接口，FastDFS同时对文件的metadata进行管理。所谓文件的meta data就是文件的相关属性，以键值对（key valuepair）方式表示，如：width=1024，其中的key为width，value为1024。文件metadata是文件属性列表，可以包含多个键值对。

跟踪器和存储节点都可以由一台或多台服务器构成。跟踪器和存储节点中的服务器均可以随时增加或下线而不会影响线上服务。其中跟踪器中的所有服务器都是对等的，可以根据服务器的压力情况随时增加或减少。

为了支持大容量，存储节点（服务器）采用了分卷（或分组）的组织方式。存储系统由一个或多个卷组成，卷与卷之间的文件是相互独立的，所有卷的文件容量累加就是整个存储系统中的文件容量。一个卷可以由一台或多台存储服务器组成，一个卷下的存储服务器中的文件都是相同的，卷中的多台存储服务器起到了冗余备份和负载均衡的作用。

在卷中增加服务器时，同步已有的文件由系统自动完成，同步完成后，系统自动将新增服务器切换到线上提供服务。

当存储空间不足或即将耗尽时，可以动态添加卷。只需要增加一台或多台服务器，并将它们配置为一个新的卷，这样就扩大了存储系统的容量。

FastDFS中的文件标识分为两个部分：卷名和文件名，二者缺一不可。

## 上传交互过程

- client 询问 tracker 上传到的 storage，不需要附加参数
- tracker 返回一台可用的 storage
- client 直接和 storage 通讯完成文件上传

## 下载交互过程

- client 询问 tracker 下载文件的 storage，参数为文件标识（卷名和文件名）
- tracker 返回一台可用的 storage
- client 直接和 storage 通讯完成文件下载

需要说明的是，client 为使用 FastDFS 服务的调用方，client 也应该是一台服务器，它对 tracker 和 storage 的调用均为服务器间的调用。

## FastDFS 为什么要结合 Nginx

我们在使用 FastDFS 部署一个分布式文件系统的时候，通过 FastDFS 的客户端 API 来进行文件的上传、下载、删除等操作。同时通过 FastDFS 的 HTTP 服务器来提供 HTTP 服务。但是 FastDFS 的 HTTP 服务较为简单，无法提供负载均衡等高性能的服务，我们使用 FastDFS 的 Nginx 模块来弥补这一缺陷。

FastDFS 通过 Tracker 服务器,将文件放在 Storage 服务器存储,但是同组之间的服务器需要复制文件,有延迟的问题.假设 Tracker 服务器将文件上传到了 192.168.1.80,文件ID已经返回客户端,这时,后台会将这个文件复制到 192.168.1.30,如果复制没有完成,客户端就用这个 ID 在 192.168.1.30 取文件,肯定会出现错误。这个 fastdfs-nginx-module 可以重定向连接到源服务器取文件,避免客户端由于复制延迟的问题,出现错误。

## 其他资源

- 源码地址：https://github.com/happyfish100
- 下载地址：http://sourceforge.net/projects/fastdfs/files/
- 官方论坛：http://bbs.chinaunix.net/forum-240-1.html