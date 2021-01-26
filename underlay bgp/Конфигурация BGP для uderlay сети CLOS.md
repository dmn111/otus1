### 					  						Конфигурация Uderlay сеть СLOS  (протокол BGP)	



#### Схема сети:

![](https://github.com/dmn111/otus1/blob/master/underlay%20bgp/%D0%A1%D1%85%D0%B5%D0%BC%D0%B0%20BGP%20%D0%B4%D0%BB%D1%8F%20underlay%20%D1%81%D0%B5%D1%82%D0%B8%20CLOS.PNG)

#### Описание сети

При построении сети выбрана следующая топология  разбиения на зоны и уровни  L2-L1 протокола ISIS. Сделано предположение, что маршрутизатор  a-core1 является ядром сети.  A-core1 обеспечивает  связность по уровню L2  для двух spine и, таким образом,  с двумя spine формирует backbone.  Маршрутизатор a-core1 выделен в отдельную зону area5, чтобы обеспечить формирование attached bit на маршрутизаторах L2/L1  в сторону маршурутизаторов L1 (areа1) и тем самым, обеспечить формирование маршрутиа по умолчанию на маршрутиазторах L1. Все spine  являются маршрутизаторами уровня L2/L1, таким образом обеспечивают связь по L1 со всеми  leaf и по L2 cо всеми spine (через a-core1).

По моему наблюдению на  NX-OS  ( в отличии от IOS) маршруты L1 по умолчанию не  передаются в базу L2  на маршрутизаторах L2-L1, в лабораторной работе это сделано  командой явно:

   address-family ipv4 unicast

​    distribute level-1 into level-2 all

Работоспособность сети проверена .

Таблица маршрутизации isis на сore1.  Доступны  оба leaf за счет дистрибьюции маршрутов L1 в базу L2 ( на spine)
<pre><code>
a-core1#sh ip route isis
...
Gateway of last resort is not set

      10.0.0.0/8 is variably subnetted, 15 subnets, 2 masks
i L2     10.77.1.0/30 [115/50] via 10.77.1.13, 02:48:48, Ethernet0/0
i L2     10.77.1.4/30 [115/50] via 10.77.1.13, 02:48:48, Ethernet0/0
i L2     10.77.1.8/30 [115/50] via 10.77.1.13, 02:48:48, Ethernet0/0
i L2     10.77.2.0/30 [115/50] via 10.77.2.13, 02:09:05, Ethernet0/1
i L2     10.77.2.4/30 [115/50] via 10.77.2.13, 02:09:05, Ethernet0/1
i L2     10.77.2.8/30 [115/50] via 10.77.2.13, 02:09:05, Ethernet0/1
i L2     10.77.255.1/32 [115/11] via 10.77.1.13, 02:48:48, Ethernet0/0
i L2     10.77.255.2/32 [115/11] via 10.77.2.13, 02:09:05, Ethernet0/1
i L2     10.77.255.3/32 [115/51] via 10.77.2.13, 01:12:37, Ethernet0/1
                        [115/51] via 10.77.1.13, 01:12:37, Ethernet0/0
i L2     10.77.255.7/32 [115/51] via 10.77.2.13, 01:41:40, Ethernet0/1
                        [115/51] via 10.77.1.13, 01:41:40, Ethernet0/0
</code></pre>

Таблица маршрутизации на  a-nxos-lf7.  Видим loopback  a-nxos-lf3  через оба spine

<pre><code>
a-nxos-lf7# sh ip route isis-1
IP Route Table for VRF "default"
'*' denotes best ucast next-hop
'**' denotes best mcast next-hop
'[x/y]' denotes [preference/metric]
'%<string>' in via output denotes VRF <string>

0.0.0.0/0, ubest/mbest: 2/0
    *via 10.77.1.9, Eth1/4, [115/40], 01:42:48, isis-1, L1
    *via 10.77.2.9, Eth1/5, [115/40], 01:42:46, isis-1, L1
10.77.1.0/30, ubest/mbest: 1/0
    *via 10.77.1.9, Eth1/4, [115/80], 01:42:48, isis-1, L1
10.77.1.4/30, ubest/mbest: 1/0
    *via 10.77.1.9, Eth1/4, [115/80], 01:42:48, isis-1, L1
10.77.1.12/30, ubest/mbest: 1/0
    *via 10.77.1.9, Eth1/4, [115/80], 01:42:48, isis-1, L1
10.77.2.0/30, ubest/mbest: 1/0
    *via 10.77.2.9, Eth1/5, [115/80], 01:42:46, isis-1, L1
10.77.2.4/30, ubest/mbest: 1/0
    *via 10.77.2.9, Eth1/5, [115/80], 01:42:46, isis-1, L1
10.77.2.12/30, ubest/mbest: 1/0
    *via 10.77.2.9, Eth1/5, [115/80], 01:42:46, isis-1, L1
10.77.255.1/32, ubest/mbest: 1/0
    *via 10.77.1.9, Eth1/4, [115/41], 01:42:48, isis-1, L1
10.77.255.2/32, ubest/mbest: 1/0
    *via 10.77.2.9, Eth1/5, [115/41], 01:42:46, isis-1, L1
10.77.255.3/32, ubest/mbest: 2/0
    *via 10.77.1.9, Eth1/4, [115/81], 01:13:53, isis-1, L1
    *via 10.77.2.9, Eth1/5, [115/81], 01:13:45, isis-1, L1

</code></pre>

Сеседство по L2  и по L1 на spine

<pre><code>
a-nxos-sp1# sh isis interface br
IS-IS process: 1 VRF: default
Interface    Type  Idx State        Circuit   MTU  Metric  Priority  Adjs/AdjsUp

######                                                    L1  L2  L1  L2    L1    L2
Topology: TopoID: 0
loopback1    Loop  1     Up/Ready   0x01/L1-2 1500 1   1   64  64    0/0   0/0
Topology: TopoID: 0
Ethernet1/1  P2P   5     Up/Ready   0x01/L2   1500 40  40  64  64    0/0   1/1
Topology: TopoID: 0
Ethernet1/2  P2P   4     Up/Ready   0x01/L1   1500 40  40  64  64    1/1   0/0
Topology: TopoID: 0
Ethernet1/3  P2P   3     Up/Ready   0x01/L1   1500 40  40  64  64    0/0   0/0
Topology: TopoID: 0
Ethernet1/4  P2P   2     Up/Ready   0x01/L1   1500 40  40  64  64    1/1   0/0

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

##### core: 

| Оборудование | Интерфейс | Ip адрес        |
| :----------: | --------- | --------------- |
|   a-core1    | loopback1 | 10.77.255.15/32 |
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

[a-core1](https://github.com/dmn111/otus1/blob/master/underlay%20bgp/a-core1.conf.md)
