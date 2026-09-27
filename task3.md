# **task3**

1. 拉取Docker Hub镜像到本地，创建bridge网络，指定IPAM。创建并启动容器mongodb,nrf,amf,smf,upf,ausf,udm,udr,pcf,nssf,chf,nef,n3iwf,tngf,webui,n3iwue,ueransim.
> 使用docker compose部署核心网以及过程中定义了哪些容器。
2. 所有容器接在同一个 bridge 网络上，宿主机网桥br-free5gc。每个容器得到一对veth，一端在容器内是eth0，另一端挂在网桥上；Docker自动下发路由和 ARP。而amf和n3iwf固定IP，thgf接管宿主机网络栈。
