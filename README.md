# 基于类bitTorrent协议的p2p文件共享系统
# 项目简介：
P2P，即点对点网络（Peer-to-Peer Network），是指一种基于去中心化的计算机网络模型，它不依赖于传统的服务器或中心节点，而是将节点之间的计算能力、存储空间、带宽等资源进行共享和协作，实现分布式计算、数据传输和共享等功能。
众所周知，传统的C/S模式的文件传输中，任意一个客户端想要请求一个文件都需要向服务端发起请求然后由服务器返回文件资源。服务器频繁大量地向大量客户端接收和发送数据时，服务器负载就会提高，而文件传输又恰好是一个需要大量带宽资源的服务，这就使服务器的上载带宽成为整个系统的主要瓶颈，想要解决这个瓶颈就需要增加网络设备的性能。这必将对服务提供者带来相应的成本提高，类似的场景有许多像CDN和一些大型的视频网站也都使用了p2p的技术进行加速。
本系统利用了P2P技术，将传统的客户端分布式的文件资源能够实现相互传输、相互补全，相应的流量也转移了用户与用户之间。从而解决了传统的C/S模式中服务端带宽受限的瓶颈。
考虑到系统网络的节点个数局限性，本系统采用了中心化的P2P类型，即由一台或多台服务器节点设备（我们称之为tracker）对P2P网络文件资源进行调配，虽然采用了服务端，但是由于服务器只接受简单且数据量少的文件请求信息，大流量的文件传输仍由各个P2P节点之间进行从而减少服务器负担。
首先介绍一下该系统中的名词
1.tracker：p2p网络中的服务节点，对P2P网络文件资源进行调配。
2.torrent文件（种子文件）：种子文件中存有分享文件的元信息，包括大小，名称，和唯一的哈希值。
3.磁力链接：包含了tracker的地址和tracker中相应的种子文件的哈希值。
4.peer：有共同torrent文件的节点。


//为提高性能，节点的文件将被分割成块，相应的块使用多线程进行传输，并且不同节点之间会尽量保证优先下载不同的分块以此来保证性能。//(不一定有)

# 系统共享的原理如下
1.先有分享者发布torrent文件到tracker服务器
2.tracker服务器维护一个torrent文件和磁力链接的映射
3.由下载者向tracker服务器发送磁力链接，返回peer节点信息和torrent文件
4.对peer节点建立链接，根据torrent文件进行下载