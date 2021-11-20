# vSphere with Tanzu实验-2：NSX-ALB 20.1.6安装和配置-2



### 配置NSX-ALB controller的License

![image-20210719032343200](vSphere with Tanzu实验-2(AVI 20.1.6)-2.assets/image-20210719032343200.png)



### 配置NSX-ALB controller的证书

![image-20210719032532409](vSphere with Tanzu实验-2(AVI 20.1.6)-2.assets/image-20210719032532409.png)

点击Access Settings页面右上角的笔型图标。

![image-20210719032619400](vSphere with Tanzu实验-2(AVI 20.1.6)-2.assets/image-20210719032619400.png)

选择SSL/TLS Certificate选项的下拉菜单，选择Create Certificate。

![image-20210719032742138](vSphere with Tanzu实验-2(AVI 20.1.6)-2.assets/image-20210719032742138.png)

按照上图示例新建1个NSX-ALB controller的证书。

注意Subject Alternate Name（SAN）必须配置，也要与Common Name一致，建议采用NSX-ALB controller的FQDN。

建议Algorithm选择EC，Key Size选择SECP256R1。

Save。

![image-20210719032829059](vSphere with Tanzu实验-2(AVI 20.1.6)-2.assets/image-20210719032829059.png)

SSL/TLS Certificate选择新建的证书。其他配置参考上图配置。Save。

![image-20210719032904915](vSphere with Tanzu实验-2(AVI 20.1.6)-2.assets/image-20210719032904915.png)

完成配置，检查各项配置信息。



### 配置DNS/NTP

![image-20210719033154340](vSphere with Tanzu实验-2(AVI 20.1.6)-2.assets/image-20210719033154340.png)



![image-20210719033236331](vSphere with Tanzu实验-2(AVI 20.1.6)-2.assets/image-20210719033236331.png)



### 配置1个Service Engine Group

![image-20210719033334646](vSphere with Tanzu实验-2(AVI 20.1.6)-2.assets/image-20210719033334646.png)

选择Default-Group条目最右边的笔型图标。

按照下图示例配置NSX-ALB Service Engine Group的基本配置。

具体的配置含义需要参考前期准备中列出的文档。

Save。

![image-20210719033417991](vSphere with Tanzu实验-2(AVI 20.1.6)-2.assets/image-20210719033417991.png)

选择Advanced页面，进入高级配置。

需要选择此Service Engine Group可以运行的ESXi Cluster。其他可以选择默认配置。

Save。

![image-20210719033454418](vSphere with Tanzu实验-2(AVI 20.1.6)-2.assets/image-20210719033454418.png)



### 配置Virtual IP Network

![image-20210719033536192](vSphere with Tanzu实验-2(AVI 20.1.6)-2.assets/image-20210719033536192.png)

选择DSwitch-Management条目最右侧的笔型图标。

![image-20210719033653312](vSphere with Tanzu实验-2(AVI 20.1.6)-2.assets/image-20210719033653312.png)

取消DHCP Enabled选项。

选择192.168.110.0/24条目最右侧的笔形图表。

![image-20210719033945089](vSphere with Tanzu实验-2(AVI 20.1.6)-2.assets/image-20210719033945089.png)

选择Use Static IP Address for VIPs and SE，点击Add Static IP Address Pool按钮，

![image-20210719033904399](vSphere with Tanzu实验-2(AVI 20.1.6)-2.assets/image-20210719033904399.png)

按照本章开始的规划创建NSX-ALB Service Engine Group的管理IP地址池。

Save。

![image-20210719034134959](vSphere with Tanzu实验-2(AVI 20.1.6)-2.assets/image-20210719034134959.png)

选择K8s-Frontend条目最右侧的笔型图标。

![image-20210719034203348](vSphere with Tanzu实验-2(AVI 20.1.6)-2.assets/image-20210719034203348.png)

取消DHCP Enabled选项。

选择Add Subnet按钮。

![image-20210719034307726](vSphere with Tanzu实验-2(AVI 20.1.6)-2.assets/image-20210719034307726.png)

IP Subnet=192.168.220.0/23

勾选Use Static IP Address for VIPs and SE。

像上一个步骤一样，Add Static IP Address Pool，按照按照本章开始的规划创建NSX-ALB Virtual Server池的VIP地址池。

![image-20210719034350629](vSphere with Tanzu实验-2(AVI 20.1.6)-2.assets/image-20210719034350629.png)



### 配置Static Routes

![image-20210719034501706](vSphere with Tanzu实验-2(AVI 20.1.6)-2.assets/image-20210719034501706.png)

点击CREATE按钮。

配置如下两条静态路由。

![image-20210719034549722](vSphere with Tanzu实验-2(AVI 20.1.6)-2.assets/image-20210719034549722.png)



![image-20210719034631771](vSphere with Tanzu实验-2(AVI 20.1.6)-2.assets/image-20210719034631771.png)



![image-20210719034706910](vSphere with Tanzu实验-2(AVI 20.1.6)-2.assets/image-20210719034706910.png)



### 配置IPAM Profile和DNS Profile

![image-20210719034804729](vSphere with Tanzu实验-2(AVI 20.1.6)-2.assets/image-20210719034804729.png)

点击CREATE按钮。

配置如下图示例的IPAM Profile。

Save。

![image-20210719034852793](vSphere with Tanzu实验-2(AVI 20.1.6)-2.assets/image-20210719034852793.png)

配置如下图示例的DNS Profile。

Save。

![image-20210719034936776](vSphere with Tanzu实验-2(AVI 20.1.6)-2.assets/image-20210719034936776.png)



![image-20210719035001873](vSphere with Tanzu实验-2(AVI 20.1.6)-2.assets/image-20210719035001873.png)



### 为Default-Cloud配置IPAM profile和DNS profile

![image-20210719035051090](vSphere with Tanzu实验-2(AVI 20.1.6)-2.assets/image-20210719035051090.png)

选择Default-Cloud条目最右侧的笔型图标。

![image-20210719035237079](vSphere with Tanzu实验-2(AVI 20.1.6)-2.assets/image-20210719035237079.png)

在IPAM/DNS配置部分，选择IPAM Profile配置的下拉菜单中选择前面配置的IPAM profile，选择DNS Profile配置的下拉菜单中选择前面配置的DNS profile。

Save。

![image-20210719035320929](vSphere with Tanzu实验-2(AVI 20.1.6)-2.assets/image-20210719035320929.png)



### 确认配置

![image-20210719035358265](vSphere with Tanzu实验-2(AVI 20.1.6)-2.assets/image-20210719035358265.png)

选择页面中刷新图标，确认刷新后的Status为绿色。

这样NSX-ALB的配置完成。VC中会自动按照上述配置创建2个NSX-ALB Service Engine VM。







