title: Socket TCP和UDP收发包数据包的区别
date: 2013-06-06 09:02:30
categories: 学习
tags: socket
---

一个包没有固定长度，以太网限制在46－1500字节，1500就是以太网的MTU，超过这个量，TCP会为IP数据报设置偏移量进行分片传输，现在一般可允许应用层设置8k（NTFS系统）的缓冲区，8k的数据由底层分片，而应用层看来只是一次发送。

<!--more-->

windows的缓冲区经验值是4k。

Socket本身分为两种，流(TCP)和数据报(UDP)，你的问题针对这两种不同使用而结论不一样。甚至还和你是用阻塞、还是非阻塞Socket来编程有关。

1. 通信长度，这个是你自己决定的，没有系统强迫你要发多大的包，实际应该根据需求和网络状况来决定。对于TCP，这个长度可以大点，但要知道，Socket内部默认的收发缓冲区大小大概是8K，你可以用SetSockOpt来改变。但对于UDP，就不要太大，一般在1024至10K。注意一点，你无论发多大的包，IP层和链路层都会把你的包进行分片发送，一般局域网就是1500左右，广域网就只有几十字节。分片后的包将经过不同的路由到达接收方，对于UDP而言，要是其中一个分片丢失，那么接收方的IP层将把整个发送包丢弃，这就形成丢包。显然，要是一个UDP发包佷大，它被分片后，链路层丢失分片的几率就佷大，你这个UDP包，就佷容易丢失，但是太小又影响效率。最好可以配置这个值，以根据不同的环境来调整到最佳状态。

    send()函数返回了实际发送的长度，在网络不断的情况下，它绝不会返回(发送失败的)错误，最多就是返回0。对于TCP你可以写一个循环发送。当send函数返回SOCKET_ERROR时，才标志着有错误。但对于UDP，你不要写循环发送，否则将给你的接收带来极大的麻烦。所以UDP需要用SetSockOpt来改变Socket内部Buffer的大小，以能容纳你的发包。明确一点，TCP作为流，发包是不会整包到达的，而是源源不断的到，那接收方就必须组包。而UDP作为消息或数据报，它一定是整包到达接收方。

2. 关于接收，一般的发包都有包边界，首要的就是你这个包的长度要让接收方知道，于是就有个包头信息，对于TCP，接收方先收这个包头信息，然后再收包数据。一次收齐整个包也可以，可要对结果是否收齐进行验证。这也就完成了组包过程。UDP，那你只能整包接收了。要是你提供的接收Buffer过小，TCP将返回实际接收的长度，余下的还可以收，而UDP不同的是，余下的数据被丢弃并返回WSAEMSGSIZE错误。注意TCP，要是你提供的Buffer佷大，那么可能收到的就是多个发包，你必须分离它们，还有就是当Buffer太小，而一次收不完Socket内部的数据，那么Socket接收事件(OnReceive)，可能不会再触发，使用事件方式进行接收时，密切注意这点。这些特性就是体现了流和数据包的区别。

    补充一点，接收BuffSize >= 发送BuffSize >= 实际发送Size，对于内外部的Buffer都适用，上面讲的主要是Socket内部的Buffer大小关系。

3. TCP是有多少就收多少，如果没有当然阻塞Socket的recv就会等，直到有数据，非阻塞Socket不好等，而是返回WSAEWOULDBLOCK。UDP，如果没有数据，阻塞Socket就会等，非阻塞Socket也返回WSAEWOULDBLOCK。如果有数据，它是会等整个发包到齐，并接收到整个发包，才返回。