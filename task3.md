# **task3**
## 使用docker-compose部署核心网
1. 拉取Docker Hub镜像到本地，创建bridge网络，指定IPAM。创建并启动容器mongodb,nrf,amf,smf,upf,ausf,udm,udr,pcf,nssf,chf,nef,n3iwf,tngf,webui,n3iwue,ueransim.
> 使用docker compose部署核心网以及过程中定义了哪些容器。
2. 所有容器接在同一个 bridge 网络上，宿主机网桥br-free5gc。每个容器得到一对veth，一端在容器内是eth0，另一端挂在网桥上；Docker自动下发路由和 ARP。而amf和n3iwf固定IP，tngf接管宿主机网络栈。
> 容器之间的网络连接方式
3. 数据卷的两类挂载，dbdata具名卷，使用卷是为了让Docker管理路径和权限，容器删了数据需要保存，否则每次重建都会丢失签约数据。bind mount配置文件，将配置从镜像里面抽出来，同一个镜像，可以通过改变挂载的YAML改变行为。
> 数据卷为什么这么写
4. depends_on的取值db最先因为UDR等启动后会立刻查MongoDB，库没启动会直接崩溃。nrf第二因为所有NF启动第一件事是NFRegister。smf依赖【nrf，upf】因为SMF要给UPF发PFCP，UPF必须先监听8805。n3iwf/tngf/ueransim依赖【amf，smf，upf】：接入网网元要能同时接上控制面和用户面。
> 依赖顺序为什么这样写
5. 只有三个地方需要特权，cap_add:【NET_ADMIN】：需要改网络。devices:【"/dev/net/tun"】:模拟UE需要建uesimtunTUN接口，TUN是字符设备，必须显式把设备节点暴露进容器。network_mode:host:要独占宿主机网络栈做IPsec。
> 特权配置为什么这么写
## 手写 dockerfile
1. 手写命令跑通对命令顺序要求很高，原本compose中的环境声明全都需要编码在敲命令的顺序里面，手动模式下需要把9个主机名全部编码成IP，由于原本做task2时使用的是v3.3.0版本，需要跟v4.2.3匹配，compose使用${FREE5GC_IMAGE_TAG}控制所有镜像版本，手动模式没有机制保证镜像和配置的版本一致。