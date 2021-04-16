#### 			Объединение  ЦОД по технологии VXLAN Multipod

####  

  выбор технологии VXLAN multipod  для объединения трех ЦОД.  Обеспечить L2 связность между ЦОД для overlay сети. 




#####  2. Схема uderlay сети.

![](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20multipod/underlay%20multipod.PNG)

#####  3. Схема overlay  сети.

![](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20multipod/overlay%20multipod.PNG)



##### 4. Адресное пространство. 

#####   DCI линк

интерфейсы p2p  между  A-B  - 10.1.1.0/24 

DataCenter A:   

AS65001

вся сеть -10.77.0.0/16,   

интерфейсы loopback  - 10.77.255.0/24, 

интерфейсы p2p  для spine1  - 10.77.1.0/24 

интерфейсы p2p  для spine2  - 10.77.2.0/24 

| Оборудование | Интерфейс | Ip адрес       | Тип сети | VFR|
| :----------: | --------- | -------------- |---|--|
|  a-nxos-sp1  | loopback1 | 10.77.255.1/32 |underay|default|
|              | e1/1      | 10.1.1.1/30  |underay|default|
|              | e1/2      |                |          |         |
|              | e1/3      | 10.77.1.1/30  |underay|default|
|              | e1/4      | 10.77.1.5/30  |underay|default|
| | e1/5 | 10.1.1.9/30 |underay|default|
| | e1/7 |                |          |         |
| |  |  |||

| Оборудование | Интерфейс | Ip адрес       | Тип сети | VFR|
| :----------: | --------- | -------------- |---|--|
|  a-nxos-sp2  | loopback1 | 10.77.255.2/32 |underay|default|
|              | e1/1      | 10.1.1.5/30  |underay|default|
|              | e1/2      |                |          |         |
|              | e1/3      | 10.77.2.1/30  |underay|default|
|              | e1/4      | 10.77.2.5/30  |underay|default|
|              | e1/5     | 10.1.1.13/30 |underay|default|
|   | e1/7 |                |          |         |
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





##### 6. Листинг  конфигурации оборудования 

[a-nxos-sp1](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20multipod/a-nxos-sp1.md)

[a-nxos-sp2](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20multipod/a-nxos-sp2.md)

[a-nxos-lf3](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20multipod/a-nxos-lf3.md)

[a-esx-01](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20multipod/a-esx-01.md)

[b-nxos-sp1](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20multipod/b-nxos-sp1.md)

[b-nxos-sp2](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20multipod/b-nxos-sp2.md)

[b-nxos-lf3](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20multipod/b-nxos-lf3.md)

[b-wf-wan11](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20multipod/b-fw-wan11.md)

[b-db-01](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20multipod/b-db-01.md)