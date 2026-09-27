# **task2**

1. 完成free5gc虚拟机的创建和配置后，进行free5gc的安装。
在安装过程中遇到了内核版本与指导内容不匹配的问题，完成了内核版本的降级。
2. 进行ueransim的编译，注意到make版本过低，先升级make版本后，完成了编译。开启核心网时检测到gtp5g版本过高，更新后继续操作。
3. 在核心网 WebConsole 中添加测试用户时，登入web页面出现404，查询后发现需要单独构建并复制前端资源。
4. 开始修改UERANSIM配置，完成UERANSIM的配置，ping通8.8.8.8，基础数据面全部正常，准备开始ULCL分流配置。
5. UE注册经过的网元如下：gNB ──> AMF ──> AUSF ──> UDM ──> UDR ──> MongoDB
6. 修改free5gc的yaml文件，关联两个UPF的PFCP。确认8个NF和2个UPF全部运行，两个PFCP全部关联成功。重启gNB和UE。检查SMF日志看到两条Selected UPF。两个ping通但走的路径不同。
7. 分流指的是UE的PDU会话中的上行流量到达UPF后，UPF决定哪些包走路径A，哪些包走路径B。分流规则在SMF决定，在UPF执行，UPF收到SMF翻译后的决策PDR之后加载到gtp5g内核模块中。PDR中根据UEAddr字段包含UE的IP，FTEID含义为GTP-U隧道ID+端点，SrcIntf	含义为来源接口等。
8. 完成后看gtp5g规则；抓包对比两条路径，应该只有8.8.8.8N9有包；观察N6隧道出口，只有8.8.8.8的流量可以从AnchorUPF1的N6出去