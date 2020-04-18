### Pod网络

*   一个pod一个ip
    *   pod内所有容器共享网络namespace
    *   容器间通信不需NAT，直接localhost
    *   Node和容器不需NAT
*   集群内部访问通过Service，集群外访问走Ingress
*   CNI（container network interface）配置Pod网路

