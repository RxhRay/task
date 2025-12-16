# **task1**

1. 下载wireshark，将用户组包含到wireshark组中。

2. 切换网卡混杂模式，启动wireshark，监听ens33.

3. 终端输入ping baidu.com，筛选DNS和ICMP类型的报文。

4. 观察到baidu的DNS报文后观察ICMP报文。打开ICMP报文后，查看Internet Control Message Protocol。展开 “Type” 确认是请求（8）还是响应（0），核对 “Identifier” 和 “Sequence number”，匹配请求与响应，对比请求包与响应包的 “Data” 部分，确认数据一致。

5. 例如：检查分组9和分组10，前者type为8 后者type为10.Identifier均为6892（BE）和60442（LE）。Sequence number均为2（BE）和512（LE）。两者的data部分也均一致。意味着两者是互相对应的一对请求和响应。

6. ![截图](task1.png)

7. 创建第二个虚拟机B，找到A的IP，确保B终端ping通。关闭A和B的防火墙。在A上启动nc监听，将A作为TCP服务端并启动wireshark。收到三个TCP包对应三次握手。

8. 步骤一：客户端发起连接请求。 标志：SYN=1 ACK=0，Seq = 0 Len = 0

   步骤二：服务端响应并同步。 标志：SYN=1，ACK=1 Seq = 1 

   步骤三：客户端确认。 标志：ACK=1，Seq=1，Ack=2 

   我客户端初始序列号为0，服务端初始序列号为1