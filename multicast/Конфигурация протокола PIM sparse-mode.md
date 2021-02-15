### 					  Конфигурация протокола PIM sparse mode	


#### Схема сети:

![](https://github.com/dmn111/otus1/blob/master/multicast/%D0%A1%D1%85%D0%B5%D0%BC%D0%B0%20%D1%81%D0%B5%D1%82%D0%B8%20%D0%BD%D0%B0%20%D0%BE%D1%81%D0%BD%D0%BE%D0%B2%D0%B5%20%D0%BF%D1%80%D0%BE%D1%82%D0%BE%D0%BA%D0%BE%D0%BB%D0%B0%20PIM%20sparse%20mode.png)

#### Описание сети

​	Для изучения работы протокола PIM в режиме sparse mode была выбрана конфигурация сети приведенная на схеме. В качестве маршрутиазтора RP выбран  a-nxos-sp1,  маршрутизатор FHR  - a-nxos-lf4,  маршрутизатор LHR - a-nxos-lf7, в качестве потребителя мультикаст трафика маршрутиазтор pc-01 с подпиской на мультикаст группу 239.5.5.5,  в качестве источника мулитикаста маршрутизатор srv-01. Выбор a-nxos-sp1 в качестве RP позволяет моделировать переключение трафика при  spt switchover. Проанализированы два состояния сети, маршрутизатор a-nxos-sp2 выключен и включен.

​	В результате включения потребителя  и  источника мультикаст трафика видим построение shared tree через RP

<pre><code>
 a-nxos-lf7# sh ip mroute 
IP Multicast Routing Table for VRF "default"
(*, 232.0.0.0/8), uptime: 09:13:40, pim ip 
  Incoming interface: Null, RPF nbr: 0.0.0.0
  Outgoing interface list: (count: 0)
(*, 239.5.5.5/32), uptime: 00:17:06, igmp ip pim 
  Incoming interface: Ethernet1/4, RPF nbr: 10.77.1.9
  Outgoing interface list: (count: 1)
    Ethernet1/2, uptime: 00:17:06, igmp
(10.6.6.6/32, 239.5.5.5/32), uptime: 00:09:42, ip mrib pim 
  Incoming interface: Ethernet1/4, RPF nbr: 10.77.1.9
  Outgoing interface list: (count: 1)
    Ethernet1/2, uptime: 00:09:42, mrib
</code></pre>

<pre><code>
a-nxos-sp1# sh ip mroute 
IP Multicast Routing Table for VRF "default"
(*, 232.0.0.0/8), uptime: 09:14:48, pim ip 
  Incoming interface: Null, RPF nbr: 0.0.0.0
  Outgoing interface list: (count: 0)
(*, 239.5.5.5/32), uptime: 00:17:56, pim ip 
  Incoming interface: loopback1, RPF nbr: 10.77.255.1
  Outgoing interface list: (count: 1)
    Ethernet1/4, uptime: 00:17:56, pim
(10.6.6.6/32, 239.5.5.5/32), uptime: 00:21:07, pim ip 
  Incoming interface: Ethernet1/3, RPF nbr: 10.77.1.6, internal
  Outgoing interface list: (count: 1)
    Ethernet1/4, uptime: 00:17:56, pim
</code></pre>

<pre><code>
a-nxos-lf4# sh ip mroute 
IP Multicast Routing Table for VRF "default"
(*, 232.0.0.0/8), uptime: 09:15:54, pim ip 
  Incoming interface: Null, RPF nbr: 0.0.0.0
  Outgoing interface list: (count: 0)
(10.6.6.6/32, 239.5.5.5/32), uptime: 00:50:30, ip pim 
  Incoming interface: Ethernet1/5, RPF nbr: 10.6.6.6
  Outgoing interface list: (count: 1)
    Ethernet1/3, uptime: 00:20:13, pim
</code></pre>

При включении маршрутизатора a-nxos-sp2,  трафик от источника идет не через RP (a-nxos-sp1), а через a-nxos-sp2

<pre><code>
a-nxos-lf7# sh ip mroute
IP Multicast Routing Table for VRF "default"
(*, 232.0.0.0/8), uptime: 00:31:34, pim ip
  Incoming interface: Null, RPF nbr: 0.0.0.0
  Outgoing interface list: (count: 0)
(*, 239.5.5.5/32), uptime: 00:23:39, igmp ip pim
  Incoming interface: Ethernet1/4, RPF nbr: 10.77.1.9
  Outgoing interface list: (count: 1)
    Ethernet1/2, uptime: 00:23:39, igmp
(10.6.6.6/32, 239.5.5.5/32), uptime: 00:05:03, ip mrib pim
  Incoming interface: Ethernet1/5, RPF nbr: 10.77.2.9
  Outgoing interface list: (count: 1)
    Ethernet1/2, uptime: 00:05:02, mrib
</code></pre>

На  a-nxos-sp1  OIL пустой

<pre><code>
a-nxos-sp1# sh ip mroute
IP Multicast Routing Table for VRF "default"
(*, 232.0.0.0/8), uptime: 00:33:52, pim ip
Incoming interface: Null, RPF nbr: 0.0.0.0
  Outgoing interface list: (count: 0)  
(*, 239.5.5.5/32), uptime: 00:25:24, pim ip
  Incoming interface: loopback1, RPF nbr: 10.77.255.1
  Outgoing interface list: (count: 1)
    Ethernet1/4, uptime: 00:25:24, pim
(10.6.6.6/32, 239.5.5.5/32), uptime: 00:06:48, pim mrib ip
  Incoming interface: Ethernet1/3, RPF nbr: 10.77.1.6, internal
  Outgoing interface list: (count: 0)
</code></pre>

<pre><code>
a-nxos-sp2# sh ip mroute
IP Multicast Routing Table for VRF "default"
(*, 232.0.0.0/8), uptime: 00:14:36, pim ip
  Incoming interface: Null, RPF nbr: 0.0.0.0
  Outgoing interface list: (count: 0)
(10.6.6.6/32, 239.5.5.5/32), uptime: 00:07:25, pim ip
  Incoming interface: Ethernet1/2, RPF nbr: 10.77.2.2
  Outgoing interface list: (count: 1)
    Ethernet1/5, uptime: 00:07:25, pim
</code></pre>

<pre><code>
a-nxos-lf4# sh ip mroute
IP Multicast Routing Table for VRF "default"
(*, 232.0.0.0/8), uptime: 00:32:22, pim ip
  Incoming interface: Null, RPF nbr: 0.0.0.0
  Outgoing interface list: (count: 0)
(10.6.6.6/32, 239.5.5.5/32), uptime: 00:06:26, ip pim
  Incoming interface: Ethernet1/5, RPF nbr: 10.6.6.6
  Outgoing interface list: (count: 1)
    Ethernet1/2, uptime: 00:06:25, pim
</code></pre>

#### Адресное пространство

##### DataCenter A:   

вся сеть -10.77.0.0/16,   

интерфейсы loopback  - 10.77.255.0/24, 

интерфейсы p2p  для spine1  - 10.77.1.0/24 

интерфейсы p2p  для spine2  - 10.77.2.0/24 

| Оборудование | Интерфейс | Ip адрес       |
| :----------: | --------- | -------------- |
|  a-nxos-sp1  | loopback1 | 10.77.255.1/32 |
|              | e1/1      | 10.77.1.13/30  |
|              | e1/2      | 10.77.1.1/30   |
|              | e1/3      | 10.77.1.5/30   |
|              | e1/4      | 10.77.1.9/30   |


| Оборудование | Интерфейс | Ip адрес       |
| :----------: | --------- | -------------- |
|  a-nxos-sp2  | loopback1 | 10.77.255.2/32 |
|              | e1/1      | 10.77.2.13/30  |
|              | e1/2      | 10.77.2.1/30   |
|              | e1/3      | 10.77.2.5/30   |
|              | e1/4      | 10.77.2.9/30   |

| Оборудование | Интерфейс | Ip адрес       |
| :----------: | --------- | -------------- |
|  a-nxos-lf3  | loopback1 | 10.77.255.3/32 |
|              | e1/1      |                |
|              | e1/2      | 10.77.1.2/30   |
|              | e1/3      | 10.77.2.6/30   |
|              | e1/4      |                |

| Оборудование | Интерфейс | Ip адрес       |
| :----------: | --------- | -------------- |
|  a-nxos-lf4  | loopback1 | 10.77.255.4/32 |
|              | e1/1      |                |
|              | e1/2      | 10.77.2.2/30   |
|              | e1/3      | 10.77.1.6/30   |
|              | e1/4      |                |

| Оборудование | Интерфейс | Ip адрес       |
| :----------: | --------- | -------------- |
|  a-nxos-lf7  | loopback1 | 10.77.255.7/32 |
|              | e1/1      |                |
|              | e1/4      | 10.77.1.10/30  |
|              | e1/5      | 10.77.2.10/30  |

##### 

| Оборудование | Интерфейс | Ip адрес    |
| :----------: | --------- | ----------- |
|    pc-01     | e0/0      | 10.5.5.5/24 |
|              |           |             |

| Оборудование | Интерфейс | Ip адрес    |
| :----------: | --------- | ----------- |
|    srv-01    | e0/0      | 10.6.6.6/24 |
|              |           |             |
|              |           |             |



##### Конфигурация оборудования :

[a-nxos-sp1](https://github.com/dmn111/otus1/blob/master/multicast/a-nxos-sp1.conf.md)

[a-nxos-sp2](https://github.com/dmn111/otus1/blob/master/multicast/a-nxos-sp2.conf.md)

[a-nxos-lf4](https://github.com/dmn111/otus1/blob/master/multicast/a-nxos-lf4.conf.md)

[a-nxos-lf7](https://github.com/dmn111/otus1/blob/master/multicast/a-nxos-lf7.conf.md)

[pc-01](https://github.com/dmn111/otus1/blob/master/multicast/pc-01.conf.md)

[srv-01](https://github.com/dmn111/otus1/blob/master/multicast/srv-01.conf.md)

