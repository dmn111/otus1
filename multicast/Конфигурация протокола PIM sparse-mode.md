### 					  						Конфигурация протокола BGP для underlay сети СLOS	



#### Схема сети:

![](https://github.com/dmn111/otus1/blob/master/underlay%20bgp/%D0%A1%D1%85%D0%B5%D0%BC%D0%B0%20CLOS%20%D0%BD%D0%B0%20%D0%BE%D1%81%D0%BD%D0%BE%D0%B2%D0%B5%20%D0%BF%D1%80%D1%82%D0%BE%D0%BA%D0%BE%D0%BB%D0%B0%20BGP%20%D0%B2%20underlay%20%D1%81%D0%B5%D1%82%D0%B8.PNG)

#### Описание сети

Для изучения работы протокола PIM в режиме sparse mode  была выбрана конфигурация сети приведенная на схеме. В качестве маршрутиазтора RP выбран  a-nxos-sp1,  маршрутизатор FHR  - a-nxos-lf4,  маршрутизатор LHR - a-nxos-lf7, в качестве потребителя мультикастамаршрутиазтор pc-01 с подпиской на мультикаст группу 239.5.5.5, с качестве источника мулитикаст маршрутизатор srv-01. Выбор a-nxos-sp1 в качестве RP позволяет промоделировать spf switching.





Пример:

<pre><code>
router bgp 65003
  router-id 10.77.255.3
  address-family ipv4 unicast
    network 10.77.255.3/32
    maximum-paths 5
  neighbor 10.77.255.1
    remote-as 65000
    update-source loopback1
    ebgp-multihop 2
    timers 3 9
    address-family ipv4 unicast
</code></pre>

В результате построения сети получены целевые показатели:

- наличие  двух маршрутов в таблице маршрутизации до loopback интрерфейсов соседних leaа, а также, два default маршрута до wan маршрутизатора
<pre><code>
a-nxos-lf3# sh ip ro
IP Route Table for VRF "default"
'*' denotes best ucast next-hop
'**' denotes best mcast next-hop
'[x/y]' denotes [preference/metric]
'%<string>' in via output denotes VRF <string>
0.0.0.0/0, ubest/mbest: 2/0
    *via 10.77.255.1, [20/0], 00:31:59, bgp-65003, external, tag 65000
    *via 10.77.255.2, [20/0], 00:32:12, bgp-65003, external, tag 65000
10.77.1.0/30, ubest/mbest: 1/0, attached
    *via 10.77.1.2, Eth1/2, [0/0], 04:55:04, direct
10.77.1.2/32, ubest/mbest: 1/0, attached
    *via 10.77.1.2, Eth1/2, [0/0], 04:55:04, local
10.77.2.4/30, ubest/mbest: 1/0, attached
    *via 10.77.2.6, Eth1/3, [0/0], 04:55:04, direct
10.77.2.6/32, ubest/mbest: 1/0, attached
    *via 10.77.2.6, Eth1/3, [0/0], 04:55:04, local
10.77.255.1/32, ubest/mbest: 1/0
    *via 10.77.1.1, [1/0], 04:39:01, static
10.77.255.2/32, ubest/mbest: 1/0
    *via 10.77.2.5, [1/0], 02:03:20, static
10.77.255.3/32, ubest/mbest: 2/0, attached
    *via 10.77.255.3, Lo1, [0/0], 04:56:39, local
    *via 10.77.255.3, Lo1, [0/0], 04:56:39, direct
10.77.255.7/32, ubest/mbest: 2/0
    *via 10.77.255.1, [20/0], 01:06:20, bgp-65003, external, tag 65000
    *via 10.77.255.2, [20/0], 01:06:20, bgp-65003, external, tag 65000
10.77.255.15/32, ubest/mbest: 2/0
    *via 10.77.255.1, [20/0], 00:58:46, bgp-65003, external, tag 65000
    *via 10.77.255.2, [20/0], 00:46:38, bgp-65003, external, tag 65000
</code></pre>

- включение multipath в  для bgp ( символ "|")
<pre><code>
   Network            Next Hop            Metric     LocPrf     Weight Path
*|e0.0.0.0/0          10.77.255.1                                    0 65000 65300 i
*>e                   10.77.255.2                                    0 65000 65300 i
*>l10.77.255.3/32     0.0.0.0                           100      32768 i
*|e10.77.255.7/32     10.77.255.2                                    0 65000 65007 i
*>e                   10.77.255.1                                    0 65000 65007 i
*|e10.77.255.15/32    10.77.255.2                                    0 65000 65300 i
*>e                   10.77.255.1                                    0 65000 65300 i
</code></pre>

- bgp соседство
<pre><code>
a-nxos-lf3# sh ip bgp sum
BGP summary information for VRF default, address family IPv4 Unicast
BGP router identifier 10.77.255.3, local AS number 65003
BGP table version is 15, IPv4 Unicast config peers 2, capable peers 2
4 network entries and 7 paths using 1252 bytes of memory
BGP attribute entries [3/492], BGP AS path entries [2/20]
BGP community entries [0/0], BGP clusterlist entries [0/0]

Neighbor        V    AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
10.77.255.1     4 65000    5287    5282       15    0    0 04:25:01 3         
10.77.255.2     4 65000    2465    2460       15    0    0 02:03:13 3         
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

##### wan: 

| Оборудование | Интерфейс | Ip адрес        |
| :----------: | --------- | --------------- |
|   WAN-R15    | loopback1 | 10.77.255.15/32 |
|              | e0/0      | 10.77.1.14/30   |
|              | e0/1      | 10.77.2.14/30   |
|              | e0/2      | 10.66.1.2/30    |
|              |           |                 |

##### DataCenter  B:   

вся сеть -10.66.0.0/16, 

интерфейсы loopback  - 10.66.255.0/24, 

интерфейсы p2p  для spine1 - 10.66.1.0/24 

| Оборудование | Интерфейс | Ip адрес       |
| :----------: | --------- | -------------- |
|  b-nxos-sp1  | loopback1 | 10.66.255.1/32 |
|              | e1/1      | 10.66.1.1/30   |
|              | e1/2      | 10.66.1.10/30  |

| Оборудование | Интерфейс | Ip адрес       |
| :----------: | --------- | -------------- |
|  b-nxos-lf1  | loopback1 | 10.66.255.2/32 |
|              | e1/1      | 10.66.1.11/30  |
|              | e1/2      | switchport     |



##### Конфигурация оборудования :

[a-nxos-sp1](https://github.com/dmn111/otus1/blob/master/underlay%20bgp/a-nxos-sp1.conf.md)

[a-nxos-sp2](https://github.com/dmn111/otus1/blob/master/underlay%20bgp/a-nxos-sp2.conf.md)

[a-nxos-lf3](https://github.com/dmn111/otus1/blob/master/underlay%20bgp/a-nxos-lf3.conf.md)

[a-nxos-lf4](https://github.com/dmn111/otus1/blob/master/underlay%20bgp/a-nxos-lf4.conf.md)

[a-nxos-lf7](https://github.com/dmn111/otus1/blob/master/underlay%20bgp/a-nxos-lf7.conf.md)

[wan-r15](https://github.com/dmn111/otus1/blob/master/underlay%20bgp/wan-r15.conf.md)
