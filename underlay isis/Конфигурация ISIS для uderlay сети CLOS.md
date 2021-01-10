### 					  						Конфигурация Uderlay сеть СLOS  (конфигурация ISIS)	



#### Схема сети:

![](https://github.com/dmn111/otus1/blob/master/underlay%20isis/%D0%A1%D1%85%D0%B5%D0%BC%D0%B0%20ISIS%20%D0%B4%D0%BB%D1%8F%20underlay%20%D1%81%D0%B5%D1%82%D0%B8%20CLOS.PNG)

#### Описание сети

При построении сети выбрана следующая топология  разбиения на зоны и уровни  L2-L1 протокола ISIS. Сделано предположение, что маршрутизатор  a-core1 является ядром сети.  A-core1 обеспечивает  связность по уровню L2  для двух spine и, таким образом,  с двумя spine формирует backbone.  Маршрутизатор a-core1 выделен в отдельную зону area5, чтобы обеспечить формирование attached bit на маршрутизаторах L2/L1  в сторону маршурутизаторов L1 (areа1) и тем самым, обеспечить формирование маршрутиа по умолчанию на маршрутиазторах L1. Все spine  являются маршрутизаторами уровня L2/L1, таким образом обеспечивают связь по L1 со всеми  leaf и по L2 cо всеми spine (через a-core1).

По моему наблюдению на  NX-OS  ( в отличии от IOS) маршруты L1 по умолчанию не  передаются в уровень L2  на маршрутизаторах L2-L1, в лабораторной работе это сделано  командой явно:

   address-family ipv4 unicast

​    distribute level-1 into level-2 all

Работоспособность сети проверена .

<pre><code>
Таблица маршрутитизации isis на сore1
a-core1#sh ip route isis 
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area 
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route

       + - replicated route, % - next hop override

Gateway of last resort is not set

      10.0.0.0/8 is variably subnetted, 15 subnets, 2 masks
i L2     10.77.1.0/30 [115/50] via 10.77.1.13, 00:17:13, Ethernet0/0
i L2     10.77.1.4/30 [115/50] via 10.77.1.13, 00:17:13, Ethernet0/0
i L2     10.77.1.8/30 [115/50] via 10.77.1.13, 00:17:13, Ethernet0/0
i L2     10.77.2.0/30 [115/50] via 10.77.2.13, 00:57:05, Ethernet0/1
i L2     10.77.2.4/30 [115/50] via 10.77.2.13, 00:53:55, Ethernet0/1
i L2     10.77.2.8/30 [115/50] via 10.77.2.13, 00:53:35, Ethernet0/1
i L2     10.77.255.1/32 [115/11] via 10.77.1.13, 00:17:13, Ethernet0/0
i L2     10.77.255.2/32 [115/11] via 10.77.2.13, 00:57:05, Ethernet0/1
i L2     10.77.255.4/32 [115/51] via 10.77.2.13, 00:17:13, Ethernet0/1
                        [115/51] via 10.77.1.13, 00:17:13, Ethernet0/0
i L2     10.77.255.7/32 [115/51] via 10.77.2.13, 00:17:07, Ethernet0/1
                        [115/51] via 10.77.1.13, 00:17:07, Ethernet0/0



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

[a-nxos-sp1](https://github.com/dmn111/otus1/blob/master/underlay%20isis/a-nxos-sp1.conf.md)

[a-nxos-sp2](https://github.com/dmn111/otus1/blob/master/underlay%20isis/a-nxos-sp2.conf.md)

[a-nxos-lf3](https://github.com/dmn111/otus1/blob/master/underlay%20isis/a-nxos-lf3.conf.md)

[a-nxos-lf4](https://github.com/dmn111/otus1/blob/master/underlay%20isis/a-nxos-lf4.conf.md)

[a-nxos-lf7](https://github.com/dmn111/otus1/blob/master/underlay%20isis/a-nxos-lf7.conf.md)

[a-core1](https://github.com/dmn111/otus1/blob/master/underlay%20isis/a-core1.conf.md)