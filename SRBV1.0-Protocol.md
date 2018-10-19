# SRB总线协议V1.0
`lee8871@126.com`
## 概述
### 什么是SRB
Simple Robot Bus 简单机器人总线。</br>
SRB是一种总线协议，基于485总线，速度采用2Mb/s，使用9bit模式传输信息，总线采用一主多从结构，可以支持32个物理节点，或128个逻辑节点。
### 设计初衷
我们希望SRB被用于机器人设备。</br>
当今的工业机器人具有非常高精度，但自由度较低，往往只有几个到十几个电机控制，每个电机对应一个自由度机器人上的传感器屈指可数：电机对应的编码器，陀螺仪，摄像头，激光雷达。</br>
相比而言，动物身体上有大量的肌肉和神经感受器，</br>
### 总线网络分层
SRB分为以下四层
1. 物理层（PHY）：关于总线的收发硬件，硬件可以使用多种方案，包括使用CAN总线驱动器。
1. 介质访问控制层（MAC）：控制节点访问总线的时机，进行包的收发，防止总线冲突。
1. 总线逻辑控制层（BLC）：根据收到包的类型进行进行数据收发，数据的收发通过缓冲区与应用层进行数据交互。这一层又分为多个部分：
  1. 数据通信（data0~data3）
  2. 簇信息通信（clu）
  3. 事件-命令通信（cmd）
  4. 重发命令（rpt）
1. 应用层（APL）：应用层进行周期性的任务，在任务完成后对缓冲区进行读写操作。
![](http://on-img.com/chart_image/5bc9c407e4b06fc64b24cf6f.png)



## 物理层
本协议使用485总线。</br>
1bit起始位，1bit停止位，无奇偶校验，9bit数据位。bit8=1表示bit0-7位为地址，bit8=0表示bit0-7位为数据。
## 数据链路层
本协议采用Modbus协议。主机发送一个数据包到节点，然后节点立即返回一个数据包，以完成一次访问。