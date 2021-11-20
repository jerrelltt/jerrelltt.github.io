# vSphere with Tanzu实验-2.1：NSX-ALB 20.1.4安装与配置截图



## vSphere with Tanzu实验环境简介

本次实验的网络连接采用vSphere分布式虚拟交换机DVS实现，外部通过NSX-ALB实现负载均衡接入。

实验的设计拓扑图如下：

![image-20211120194055927](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20211120194055927-7408458.png)

安装中涉及到的IP地址段说明：

- nsxalb—Dswitch-Managerment(192.168.110.120-139)：NSX-ALB是软件定义的负载均衡解决方案，采用了控制平面和转发平面分离的技术。在我们的实验中部署了1个NSX-ALB controller（nsxalb-controller）作为控制平面，转发平面NSX-ALB Service Engine（nsxalb-se）是VM的形态，由控制器自动按需部署。其IP地址在IP地址段内自动分配。
- nsxalb--K8s-Frontend (192.168.220.2-127)：NSX-ALB为TKC中的容器service和VM service自动创建流量负载均衡接入服务，每个负载均衡接入服务使用的virtual server(vs)是运行在NSX-ALB Service Engine VM中的进程。virtual server IP地址在此地址段内自动分配。



实际的ESXi内部的分布式虚拟交换机连接如下图所示。

![image-20210715134335379](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210715134335379.png)



## 前期准备

- 下载安装包：NSX-ALB（即AVI）OVA： **[here](https://my.vmware.com/group/vmware/downloads/details?downloadGroup=NSX-ALB-10&productId=1092)**
- NSX-ALB（即AVI）官方文档： **[here](https://avinetworks.com/docs/latest/)**
- 安装配置NSX-ALB for vSphere with Tanzu官方文档： **[here](https://docs.vmware.com/en/VMware-vSphere/7.0/vmware-vsphere-with-tanzu/GUID-AC9A7044-6117-46BC-9950-5367813CD5C1.html)**
- 准备好的vSphere7.0 U2环境，VC带管理员权限的用户。



## 安装AVI controller 20.1.4

### Deploy the Controller VM



![image-20210718233056971](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210718233056971.png)



![image-20210718233228408](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210718233228408.png)





![image-20210718233247967](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210718233247967.png)



![image-20210718233336619](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210718233336619.png)



![image-20210718233400062](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210718233400062.png)





![image-20210718233433842](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210718233433842.png)





![image-20210718233511249](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210718233511249.png)



![image-20210718233535370](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210718233535370.png)



### Configure the Controller VM

https://nasxalb-controller-01.corp.tanzu



![image-20210718235500811](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210718235500811.png)



![image-20210718235625086](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210718235625086.png)



![image-20210718235704676](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210718235704676.png)



![image-20210718235724411](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210718235724411.png)



![image-20210718235742976](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210718235742976.png)





![image-20210718235823511](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210718235823511.png)



![image-20210718235912204](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210718235912204.png)



![image-20210719000056433](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210719000056433.png)



![image-20210719000116251](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210719000116251.png)



### License the Controller



![image-20210719000432528](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210719000432528.png)





### Assign a Certificate to the Controller



![image-20210719000358506](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210719000358506.png)



![image-20210719000511649](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210719000511649.png)





![image-20210719000757397](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210719000757397.png)



![image-20210719000843014](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210719000843014.png)



![image-20210719000910901](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210719000910901.png)



### Configure a Service Engine Group

![image-20210719001136909](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210719001136909.png)



![image-20210719001250273](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210719001250273.png)



![image-20210719003752475](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210719003752475.png)







### Configure SE管理IP Network和LB Virtual IP Network

![image-20210719001420506](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210719001420506.png)





![image-20210719001516325](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210719001516325.png)



![image-20210719001849070](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210719001849070.png)





![image-20210719001801218](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210719001801218.png)



![image-20210719001929862](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210719001929862.png)



![image-20210719002001817](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210719002001817.png)



![image-20210719002118289](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210719002118289.png)



![image-20210719002143954](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210719002143954.png)



![image-20210719002248080](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210719002248080.png)



### Configure Static Routes



![image-20210719002454599](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210719002454599.png)





![image-20210719002621039](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210719002621039-6625581-6625582.png)



![image-20210719002703829](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210719002703829.png)



![image-20210719002724936](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210719002724936.png)



### Configure the IPAM and DNS Profiles



![image-20210719002816149](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210719002816149.png)



![image-20210719002934134](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210719002934134.png)





![image-20210719003029070](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210719003029070-6625829.png)





![image-20210719003103530](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210719003103530.png)



### Assign these profiles to the Default Cloud 



![image-20210719003138282](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210719003138282.png)



![image-20210719003250811](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210719003250811.png)



![image-20210719003358108](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210719003358108-6626038.png)



![image-20210719003536723](vSphere with Tanzu实验-2.1(AVI 20.1.4).assets/image-20210719003536723.png)











