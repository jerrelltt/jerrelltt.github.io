# vSphere with Tanzu实验-2：NSX-ALB 20.1.6安装与配置-1



## vSphere with Tanzu实验环境简介

本次实验的网络连接采用vSphere分布式虚拟交换机DVS实现，外部通过NSX-ALB实现负载均衡接入。

实验的设计拓扑图如下：

![image-20211120194055927](vSphere with Tanzu实验-2(AVI 20.1.6)-1.assets/image-20211120194055927-7408458.png)

安装中涉及到的IP地址段说明：

- nsxalb—Dswitch-Managerment(192.168.110.120-139)：NSX-ALB是软件定义的负载均衡解决方案，采用了控制平面和转发平面分离的技术。在我们的实验中部署了1个NSX-ALB controller（nsxalb-controller）作为控制平面，转发平面NSX-ALB Service Engine（nsxalb-se）是VM的形态，由控制器自动按需部署。其IP地址在IP地址段内自动分配。
- nsxalb--K8s-Frontend (192.168.220.2-127)：NSX-ALB为TKC中的容器service和VM service自动创建流量负载均衡接入服务，每个负载均衡接入服务使用的virtual server(vs)是运行在NSX-ALB Service Engine VM中的进程。virtual server IP地址在此地址段内自动分配。



实际的ESXi内部的分布式虚拟交换机连接如下图所示。

![image-20210715134335379](vSphere with Tanzu实验-2(AVI 20.1.6)-1.assets/image-20210715134335379.png)



## 前期准备

- 下载安装包：NSX-ALB（即AVI）OVA： **[here](https://my.vmware.com/group/vmware/downloads/details?downloadGroup=NSX-ALB-10&productId=1092)**
- NSX-ALB（即AVI）官方文档： **[here](https://avinetworks.com/docs/latest/)**
- 安装配置NSX-ALB for vSphere with Tanzu官方文档： **[here](https://docs.vmware.com/en/VMware-vSphere/7.0/vmware-vsphere-with-tanzu/GUID-AC9A7044-6117-46BC-9950-5367813CD5C1.html)**
- 准备好的vSphere7.0 U2环境，VC带管理员权限的用户。



## 安装NSX-ALB 20.1.6

下载VMware Controller OVA，然后在VC中部署。

进入OVA部署导航页面，选择下载的安装包。

### 部署 NSX-ALB Controller VM

![image-20210719023238528](vSphere with Tanzu实验-2(AVI 20.1.6)-1.assets/image-20210719023238528.png)

指定NSX-ALB controller的VM名字，选择安装的数据中心名称。

![image-20210719023317625](vSphere with Tanzu实验-2(AVI 20.1.6)-1.assets/image-20210719023317625.png)

选择ESXi集群。

![image-20210719023344055](vSphere with Tanzu实验-2(AVI 20.1.6)-1.assets/image-20210719023344055.png)

确认安装信息。

![image-20210719023423090](vSphere with Tanzu实验-2(AVI 20.1.6)-1.assets/image-20210719023423090.png)

选择存储模式和存储位置。

![image-20210719023452923](vSphere with Tanzu实验-2(AVI 20.1.6)-1.assets/image-20210719023452923.png)



选择连接的网络。

![image-20210719023521638](vSphere with Tanzu实验-2(AVI 20.1.6)-1.assets/image-20210719023521638.png)



输入相应的网络配置。

![image-20210719023714065](vSphere with Tanzu实验-2(AVI 20.1.6)-1.assets/image-20210719023714065.png)



和20.1.4相比，新增了很多关于NSX-T的集成配置。但是此处没有和NSX-T集成，所以不需要配置。

最后，确定所有安装信息，完成，进行安装。

![image-20210719024055450](vSphere with Tanzu实验-2(AVI 20.1.6)-1.assets/image-20210719024055450.png)





### 初始化配置NSX-ALB Controller VM

安装完成后，启动NSX-ALB controller VM。访问http://NSX-ALB_controller_IP。

创建admin密码。

![image-20210719031048799](vSphere with Tanzu实验-2(AVI 20.1.6)-1.assets/image-20210719031048799.png)

NSX-ALB controller VM第一次启动，首次访问web界面会进行入的初始化配置界面。

创建备份密码，DNS Server IP，DNS域名，NEXT。

![image-20210719031232984](vSphere with Tanzu实验-2(AVI 20.1.6)-1.assets/image-20210719031232984.png)

配置相应的Email/SMTP参数，NEXT。

![image-20210719031259338](vSphere with Tanzu实验-2(AVI 20.1.6)-1.assets/image-20210719031259338.png)

选择相应的IP Route Domain配置，勾选右下角的Setup Cloud After选项，SAVE。

![image-20210719031405935](vSphere with Tanzu实验-2(AVI 20.1.6)-1.assets/image-20210719031405935.png)



然后，会进入Setup Cloud页面。

选择Cloud Infrastructure Type=VMware，Next。

![image-20210719031521523](vSphere with Tanzu实验-2(AVI 20.1.6)-1.assets/image-20210719031521523.png)



进入Infrastructure配置页面，按照下图示例配置vCenter Server的连接信息，连接用户需要有管理员权限。Next。

![image-20210719031738108](vSphere with Tanzu实验-2(AVI 20.1.6)-1.assets/image-20210719031738108.png)

进入DataCenter配置页面，按照下图示例配置，Next。

![image-20210719032003513](vSphere with Tanzu实验-2(AVI 20.1.6)-1.assets/image-20210719032003513.png)

进入Network配置界面，配置NSX-ALB Service Engine的管理网络所连接的虚拟交换机端口组，勾选DHCP Enabled选项，Save。

![image-20210719032250593](vSphere with Tanzu实验-2(AVI 20.1.6)-1.assets/image-20210719032250593.png)



### 配置NSX-ALB controller的License

----> 接下文，原因是图片太多，使得我的编辑器响应很慢！！不得不分成两个文档。







