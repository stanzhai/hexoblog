title: SDN和OpenFlow学习
date: 2013-05-06 21:25:15
categories: 学习
tags:
---

## SDN

Software Development Network。 

Software-Defined Network软件定义网络。

<!--more-->

由于传统的网络设备（交换机、路由器）的固件是由设备制造商锁定和控制，所以 SDN 希望将网络控制与物理网络拓扑分离，从而摆脱硬件对网络架构的限制。这样企业便可以像升级、安装软件一样对网络架构进行修改，满足企业对整个网站架构进行调整、扩容或升级。而底层的交换机、路由器等硬件则无需替换，节省大量的成本的同时，网络架构迭代周期将大大缩短。

限于中国的特殊环境和大企业的保守，中国在普及 SDN 技术上还有一定的困难，包括运营商、网络服务商等都处在一种观望的状态，所以 SDN 普及还需时日。

## OpenFlow一种新型网络交换模型，众多SDN中的一种

由于现在的网络暴露出了越来越多的弊病以及人们对网络性能需求的提高，于是研究人员不得不把很多复杂功能加入到路由器的体系结构当中，例如OSPF，BGP，组播，区分服务，流量工程，NAT，防火墙，MPLS等等。这就使得路由器等交换设备越来越臃肿而且性能提升的空间越来越小。

然而与网络领域的困境截然不同的是，计算机领域实现了日新月异的发展。仔细回顾计算机领域的发展，不难发现其关键在于计算机领域找到了一个简单可用的硬件底层(x86指令集)。由于有了这样一个公用的硬件底层，所以在软件方面，不论是应用程序还是操作系统都取得了飞速的发展。现在很多主张重新设计计算机网络体系结构的人士认为：网络可以复制计算机领域的成功来解决现在网络所遇到的所有问题。在这种思想的指导下，将来的网络必将是这样的：底层的数据通路（交换机、路由器）是“哑的、简单的、最小的”，并定义一个对外开放的关于流表的公用的API，同时采用控制器来控制整个网络。未来的研究人员就可以在控制器上自由的调用底层的API来编程，从而实现网络的创新。

OpenFlow正是这种网络创新思想的强有力的推动者。

## OpenFlow创新原理

OpenFlow交换机将原来完全由交换机/路由器控制的报文转发过程转化为由OpenFlow交换机（OpenFlow Switch）和控制服务器（Controller）来共同完成，从而实现了数据转发和路由控制的分离。控制器可以通过事先规定好的接口操作来控制OpenFlow交换机中的流表，从而达到控制数据转发的目的。

流表由很多个流表项组成，每个流表项就是一个转发规则。进入交换机的数据包通过查询流表来获得转发的目的端口。流表项由头域、计数器和操作组成；其中头域是个十元组，是流表项的标识；计数器用来计数流表项的统计数据；操作标明了与该流表项匹配的数据包应该执行的操作。

## OpenFlow应用

随着OpenFlow/SDN概念的发展和推广，其研究和应用领域也得到了不断拓展。目前，关于OpenFlow/SDN的研究领域主要包括网络虚拟化、安全和访问控制、负载均衡、聚合网络和绿色节能等方面。另外，还有关于OpenFlow和传统网络设备交互和整合等方面的研究。
