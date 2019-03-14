
Kubernetes
https://kubernetes.io/
一个容器资源编排解决方案，类似的还有Apache Mesos、Docker Swarm等。简称K8s，是用8代替8个字符“ubernete”而成的缩写。
Google开源的一个容器编排引擎，它支持自动化部署、大规模可伸缩、应用容器化管理。在生产环境中部署一个应用程序时，通常要部署该应用的多个实例以便对应用请求进行负载均衡。

计算资源管理 cpu和mem资源如何共享且避免争抢-> Federation统一管理多个Kubernetes集群、Namespace\Pod（可理解为实例块？）\Container ->实体Node
            Replication Controller确保任意时间都有指定数量的Pod“副本”在运行
            Pod 包含一组容器和卷。同一个Pod里的容器共享同一个网络命名空间，可以使用localhost互相通信。Pod是短暂的，不是持续性实体。
            Label 一个Label是attach到Pod的一对键/值对，用来传递用户定义的属性。
            Node 节点是物理或者虚拟机器，作为Kubernetes worker，通常称为Minion
            Service 是定义一系列Pod以及访问这些Pod的策略的一层抽象。Service通过Label找到Pod组。
            Kubernetes Master提供集群的独特视角，并且拥有一系列组件，比如Kubernetes API Server
            kubelet 每个Node节点都会启动kubelet进程，用来处理Master节点下发到本节点的任务，管理Pod和其中的容器。kubelet会在API Server上注册节点             信息，定期向Master汇报节点资源使用情况，并通过cAdvisor监控容器和节点资源。可以把kubelet理解成【Server-Agent】架构中的agent，是Node上 
            的pod管家。
            kubectl 是用于运行Kubernetes集群命令的管理工具
            kubeadm 是Kubernetes官方提供的用于快速安装Kubernetes集群的工具:
            
            kubeadm init 启动一个master节点；
            kubeadm join 启动一个node节点，加入master；
            kubeadm upgrade 更新集群版本；
            kubeadm config 从1.8.0版本开始已经用处不大，可以用来view一下配置；
            kubeadm token 管理kubeadm join的token；
            kubeadm reset 把kubeadm init或kubeadm join做的更改恢复原状；
            kubeadm version打印版本信息；
            kubeadm alpha预览一些alpha特性的命令。
            
网络资源管理 跨主机容器网络互通；多租户多服务之间网络隔离；集群边界路由器管理Nginx\HAProxy\Traefik、集群DNS域名服务管理coreDNS  
            etcd
            “A highly-available key value store for shared configuration and service discovery.”
            是Kubernetes集群中的一个十分重要的组件，用于保存集群所有的网络配置和对象的状态信息
            项目地址 https://github.com/etcd-io/etcd
            官方文档 https://coreos.com/etcd/docs/latest/
            1.简介
            一个高可用的分布式键值数据库，可用于服务发现。ETCD 采用raft一致性算法，基于 Go 语言实现。
            CoreOS公司发起的一个开源项目，授权协议为Apache。
            2. ETCD vs ZK
            一致性协议： ETCD使用[Raft]协议， ZK使用ZAB（类PAXOS协议），前者容易理解，方便工程实现；
            运维方面：ETCD方便运维，ZK难以运维；
            项目活跃度：ETCD社区与开发活跃，ZK已经快死了；
            API：ETCD提供HTTP+JSON, gRPC接口，跨平台跨语言，ZK需要使用其客户端；
            访问安全方面：ETCD支持HTTPS访问，ZK在这方面缺失；
            3. ETCD的使用场景
            配置管理
            服务注册于发现
            选主
            应用调度
            分布式队列
            分布式锁
            4. ETCD读写性能
            按照官网给出的[Benchmark], 在2CPU，1.8G内存，SSD磁盘这样的配置下，单节点的写性能可以达到16K QPS, 而先写后读也能达到12K QPS。这个性能还是相当可观的。
            5. ETCD工作原理
            ETCD使用Raft协议来维护集群内各个节点状态的一致性。简单说，ETCD集群是一个分布式系统，由多个节点相互通信构成整体对外服务，每个节点都存储了完整的数据，并且通过Raft协议保证每个节点维护的数据是一致的。
            5.1 选主
            Raft协议是用于维护一组服务节点数据一致性的协议。这一组服务节点构成一个集群，并且有一个主节点来对外提供服务。当集群初始化，或者主节点挂掉后，面临一个选主问题。集群中每个节点，任意时刻处于Leader, Follower, Candidate这三个角色之一
            5.2 日志复制
            所谓日志复制，是指主节点将每次操作形成日志条目，并持久化到本地磁盘，然后通过网络IO发送给其他节点。其他节点根据日志的逻辑时钟(TERM)和日志编号(INDEX)来判断是否将该日志记录持久化到本地。当主节点收到包括自己在内超过半数节点成功返回，那么认为该日志是可提交的(committed），并将日志输入到状态机，将结果返回给客户端。
            
存储资源管理 应用配置文件、密钥管理；应用数据持久化存储；不同的应用之间共享数据存储 Volume\CSI
            Volume类：临时目录（同pod生命周期）、配置类（将配置挂载到pod内）、本地存储类、共享存储类
            CSI（Container Storage Interface）Kubernetes从v1.9版本引入，旨在容器和共享存储之间建立一套标准的存储访问接口。-将存储供应商的代码与Kubernetes的代码完全解耦，Kubernetes提供Sidecar容器。

镜像资源管理 镜像生命周期管理、权限管理、复制管理以及操作审计管理 开源镜像库Docker Registry\VMware Harbor

应用管理
    1、应用创建
    2、配置管理
    3、部署管理
    4、应用的弹性伸缩管理
    5、日志管理 Fluentd日志采集+Elaticsearch日志分析+Kibana可视化展示
       基于大数据的统一日志查询和分析Heapster+Fluentd+Flume+Kafka+Hadoop+Hive+Spark+Zeppelin
       流式日志实时查询分析方案Heapster+Fluentd+Kafka
    6、监控管理 Nigios、zabbix->Heapster+InfluxDB+Grafana、prometheus
    7、安全管理
    8、备份管理
    
 微服务管理体系
    1、单体应用
       缺点：
       1）系统膨胀臃肿，可持续开发和运维困难；
       2）很难通过水平扩展、多机部署的方式来提升系统吞吐量，即系统难以承载迅速增长的互联网用户引发的请求。
       水平扩展：增加服务器数量
       垂直扩展：给系统某些业务分配更好的机器、提供更多的资源，从而提升业务进程的负载能力和吞吐量
    2、分布式架构
       核心技术问题：如何实现两个彼此独立的进程之间的远程通信？
       技术方案：RPC（RemoteProcedureCall）技术--性能高(通信过程中采用了高效的二进制编码)
       早期方案：CORBA-基于RPC技术的分布式平台：梦想是实现所有语言、所有平台、所有厂商的各种IT系统的分布式互联互通
       发展：第一代裸机微服务架构ZeroC ICE 3.0首次实现了IceGrid--第一个公开发行的支持多语言的功能完备的微服务架构基础平台，核心特性：服务编排、服务托管、服务注册、服务路由和负载均衡、平台运维。
       之后：第二代Java生态圈微服务架构Dubbo、Spring Cloud--Java语言体系内的微服务框架，并不支持其他语言，完备性达不到IceGrid那种平台的高度，只能称为框架。
       再后：第一代容器技术微服务架构Kubernetes
       最后：第二代容器技术微服务架构Service Mesh
       微服务架构
       优点：1）解决了复杂度的问题；2）开发者自由选择开发技术甚至开发语言，更好的实现目标；3）加快部署
       缺点：增加了开发工作量并带来了固有的复杂性（微服务调用方式复杂）---Service Mesh采用代理模式的思想解决代码入侵及重复编排的问题。
       Service Mesh：一个用于处理服务与服务之间通信（调用）的复杂的基础架构设施。它通常是一组轻量级的网络代理程序，这些网络代理程序就部署在用户的应用程序旁边，而应用的代码感知不到它们的存在。
       第一代Service Mesh项目：Envoy(基于C++语言开发)\Linkerd(基于Scala语言开发)
       第二代Service Mesh项目：Istio开源平台
       网络代理组件Sidecar



Alpha：是内部测试版,一般不向外部发布,会有很多Bug.一般只有测试人员使用。
Beta：也是测试版，这个阶段的版本会一直加入新的功能。在Alpha版之后推出。
RC：(Release　Candidate) 顾名思义么 ! 用在软件上就是候选版本。系统平台上就是发行候选版本。RC版不会再加入新的功能了，主要着重于除错。
GA:General Availability,正式发布的版本，在国外都是用GA来说明release版本的。
