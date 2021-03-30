####  																									Проектная работа 

#### 			Объединение трех ЦОД по технологии VXLAN Multipod

#####   Задание.

 Выбрать технологию и реализовать модель сетевой схемы  для  объединения трех ЦОД.     Для  ЦОД имеются следующие исходные данные:  ЦОД  географически расположенные в одном населенном пункте и объединены между собой собственной оптической линией связи. Необходимо обеспечить L2 связность между ЦОД для overlay сети .  ЦОД  не в полной мере укомплектованы оборудованием для релизациия полноценной схемы CLOS.

В рамках  задания выпонить следующие пункты:
 1.	Обосновать выбор   технологии для объединения ЦОД.
 2. Изобразить схему  underlay сети
 3. Изобразить схему overlay сети
 4. Адресное пространсвтво underaly и overlay сети для каждого ЦОД
 5. Конфигурация оборудования

#### Выбор  технологии



#### Схема uderlay сети:

![](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20multipod/underlay%20multipod.PNG)

####  Схема overlay  сети:

![](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20multipod/overlay%20multipod.PNG)



#### Адресное пространство 

#####   DCI линк

интерфейсы p2p  между  A-B  - 10.1.1.0/24 

##### DataCenter A:   

AS65001

вся сеть -10.77.0.0/16,   

интерфейсы loopback  - 10.77.255.0/24, 

интерфейсы p2p  для spine1  - 10.77.1.0/24 

интерфейсы p2p  для spine2  - 10.77.2.0/24 

| Оборудование | Интерфейс | Ip адрес       | Тип сети | VFR|
| :----------: | --------- | -------------- |---|--|
|  a-nxos-sp1  | loopback1 | 10.77.255.1/32 |underay|default|
|              | e1/1      | 10.1.1.1/30  |underay|default|
|              | e1/2      | 10.1.3.1/30 |underay|default|
|              | e1/3      | 10.77.1.1/30  |underay|default|
|              | e1/4      | 10.77.1.5/30  |underay|default|
| | e1/5 | 10.1.1.9/30 |underay|default|
| | e1/7 | 10.1.3.9/30 |underlay|default|
| |  |  |||

| Оборудование | Интерфейс | Ip адрес       | Тип сети | VFR|
| :----------: | --------- | -------------- |---|--|
|  a-nxos-sp2  | loopback1 | 10.77.255.2/32 |underay|default|
|              | e1/1      | 10.1.1.5/30  |underay|default|
|              | e1/2      | 10.1.3.5/30 |underay|default|
|              | e1/3      | 10.77.2.1/30  |underay|default|
|              | e1/4      | 10.77.2.5/30  |underay|default|
|              | e1/5     | 10.1.1.13/30 |underay|default|
|   | e1/7 | 10.1.3.13/30 |underay|default|
| |  |  |||

| Оборудование | Интерфейс | Ip адрес           | Тип сети | VFR     |
| :----------: | --------- | ------------------ | -------- | ------- |
|  a-nxos-lf3  | loopback1 | 10.77.255.3/32     | underay  | default |
|              | loopback2 | 10.77.253.3/32     | underay  | default |
|              | loopback2 | 10.77.255.5/32 (s) | underay  | default |
|              | e1/1      | 10.77.1.2/30       | underay  | default |
|              | e1/2      | 10.77.2.6/30       | underay  | default |
|              | e1/3      |                    |          |         |
|              | e1/4      | 172.31.251.3/29    | underay  | vrfKEEP |
|              | vlan10    | 10.90.10.1/24      | overlay  | vrfLAN  |
|              | vlan20    | 10.90.20.1/24      | overlay  | vrfLAN  |
|              | vlan999   | forward            | overlay  | vrfLAN  |
|              |           |                    |          |         |

| Оборудование | Интерфейс      | Ip адрес       | Тип сети | VFR  |
| :----------: | -------------- | -------------- | -------- | ---- |
|   a-esx-01   | Ethernet0/0.10 | 10.90.10.71/24 |          | vrfA |
|  модель ESX  | Ethernet0/0.20 | 10.90.20.71/24 |          | vrfB |
|              | Ethernet0/0.50 | 10.90.50.71/24 |          | vrfC |

##### DataCenter B:   

AS65002

вся сеть -10.66.0.0/16,   

интерфейсы loopback  - 10.66.255.0/24, 

интерфейсы p2p  для spine1  - 10.66.1.0/24 

интерфейсы p2p  для spine2  - 10.66.2.0/24 

| Оборудование | Интерфейс | Ip адрес       | Тип сети | VFR     |
| :----------: | --------- | -------------- | -------- | ------- |
|  b-nxos-sp1  | loopback1 | 10.66.255.1/32 | underay  | default |
|              | e1/1      | 10.1.1.2/30    | underay  | default |
|              | e1/2      | 10.1.2.1/30    | underay  | default |
|              | e1/3      | 10.66.1.1/30   | underay  | default |
|              | e1/4      |                |          |         |
|              | e1/5      | 10.1.1.14/30   | underay  | default |
|              |           |                |          |         |

| Оборудование | Ip адрес       | Тип сети | VFR     |
| :----------: | -------------- | -------- | ------- |
|  b-nxos-sp2  | 10.66.255.2/32 | underay  | default |
|              | 10.1.1.6/30    | underay  | default |
|              | 10.1.2.5/30    | underay  | default |
|              |                |          |         |
|              | 10.66.2.5/30   | underay  | default |
|              | 10.1.1.10/30   | underay  | default |
|              |                |          |         |
|              |                |          |         |

| Оборудование | Интерфейс | Ip адрес        | Тип сети | VFR     |
| :----------: | --------- | --------------- | -------- | ------- |
|  b-nxos-lf3  | loopback1 | 10.66.255.3/32  | underay  | default |
|              | loopback2 | 10.66.253.3/32  | underay  | default |
|              | e1/1      | 10.66.1.2/30    | underay  | default |
|              | e1/2      | 10.66.2.6/30    | underay  | default |
|              | vlan50    | 10.90.50.254/24 | overlay  | vrfDMZ  |
|              | vlan111   | 10.90.255.2/24  | overlay  | vrfLAN  |
|              |           |                 |          |         |

|   Оборудование   | Интерфейс       | Ip адрес       | Тип сети | VFR     |
| :--------------: | --------------- | -------------- | -------- | ------- |
|    b-fw-wan11    | Ethernet0/0.50  | 10.90.50.1/24  |          | default |
| моделирование FW | Ethernet0/0.111 | 10.90.255.1/24 |          | default |
|                  |                 |                |          |         |

##### DataCenter С:   

AS65003

вся сеть -10.55.0.0/16,   

интерфейсы loopback  - 10.55.255.0/24, 

интерфейсы p2p  для spine1  - 10.55.1.0/24 

интерфейсы p2p  для spine2  - 10.55.2.0/24 

| Оборудование | Интерфейс | Ip адрес           | Тип сети | VFR     |
| :----------: | --------- | ------------------ | -------- | ------- |
|  с-nxos-sp1  | loopback1 | 10.55.255.1/32     | underay  | default |
|              | loopback2 | 10.55.253.3/32     | underay  | default |
|              | loopback2 | 10.55.253.5/32 (s) | underay  | default |
|              | e1/1      | 10.1.3.2/30        | underay  | default |
|              | e1/2      | 10.1.2.2/30        | underay  | default |
|              | e1/3      |                    |          |         |
|              | e1/4      | 172.31.255.1/30    | underay  | vrfKEEP |
|              | e1/7      | 10.1.3.14/30       | underay  | default |
|              | vlan5     | 10.55.255.253/30   | underay  | default |
|              | vlan10    | 10.90.10.1/24      | overlay  | vrfLAN  |
|              | vlan20    |                    |          |         |
|              | vlan999   | forward            | overlay  | vrfLAN  |
|              |           |                    |          |         |

| Оборудование | Интерфейс | Ip адрес           | Тип сети | VFR     |
| :----------: | --------- | ------------------ | -------- | ------- |
|  с-nxos-sp2  | loopback1 | 10.55.255.2/32     | underay  | default |
|              | loopback2 | 10.55.253.4/32     | underay  | default |
|              | loopback2 | 10.55.253.5/32 (s) | underay  | default |
|              | e1/1      | 10.1.3.6/30        | underay  | default |
|              | e1/2      | 10.1.2.6/30        | underay  | default |
|              | e1/3      |                    |          |         |
|              | e1/4      | 172.31.255.2/30    | underay  | vrfKEEP |
|              | e1/7      | 10.1.3.10/30       | underay  | default |
|              | vlan5     | 10.55.255.254/30   | underay  | default |
|              | vlan10    | 10.90.10.1/24      | overlay  | vrfLAN  |
|              | vlan20    |                    |          |         |
|              | vlan999   | forward            | overlay  | vrfLAN  |
|              |           |                    |          |         |

|    Оборудование     | Интерфейс   | Ip адрес       | Тип сети | VFR  |
| :-----------------: | ----------- | -------------- | -------- | ---- |
|       c-db-01       | Ethernet0/0 | 10.90.10.51/24 |          |      |
| модель физ. сервера |             |                |          |      |
|                     |             |                |          |      |

|    Оборудование    | Интерфейс   | Ip адрес       | Тип сети | VFR  |
| :----------------: | ----------- | -------------- | -------- | ---- |
|      c-lb-01       | Ethernet0/0 | 10.90.10.71/24 |          | vrfA |
| модель физ.сервера |             |                |          |      |

| Оборудование | Интерфейс  | Ip адрес | Тип сети | VFR  |
| :----------: | ---------- | -------- | -------- | ---- |
|    c-sw5     | switchport |          |          |      |



Конфигурация оборудования :

[a-nxos-sp1](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20type-5/a-nxos-sp1.conf.md)

[a-nxos-sp2](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20type-5/a-nxos-sp2.conf.md)

[a-nxos-lf3](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20type-5/a-nxos-lf3.conf.md)

[a-nxos-lf4](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20type-5/a-nxos-lf4.conf.md)

[a-nxos-lf7](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20type-5/a-nxos-lf7.conf.md)

[R7](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20type-5/R7.conf.md)

[SW10](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20type-5/SW10.conf.md)