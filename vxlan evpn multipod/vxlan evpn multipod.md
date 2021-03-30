####  																									Проектная работа 

#### 			Объединение трех ЦОД по технологии VXLAN Multipod

####  Задание.

   Обосновать выбор технологии VXLAN multipod  для объединения трех ЦОД.  Реализовать сетевую схему взаимодействия. Обеспечить L2и L3 связность между ЦОД для overlay сети. 

​		Для ЦОД  имеются следующие исходные данные:

- ЦОД  географически расположенные в одном населенном пункте и объединены между собой собственной оптической линией связи. 
- ЦОД  не в полной мере укомплектованы оборудованием для релизациия полноценной схемы CLOS.

##### План работ: 
 1.	Обосновать выбор технологии объединения ЦОД.
 2. Составить схему  underlay сети.
 3. Составить схему overlay сети.
 4. Выделить адресное пространсвтво underaly и overlay сети для каждого ЦОД.
 5. Выполнить конфигурацию оборудования на стенде.
 6.	Приложить конфигурацию оборудования стенда.

##### 1. Обоснование  выбора  технологии объединения ЦОД.

   При выборе  сетевой технологии по объединению ЦОД  рассматривались  два варианта изучаемых на курсе VXLAN Multipod  и VXLAN Multisite.  В пользу выбора VXLAN Multipod повлияли следующие факторы: 
  - наличие одного админитсртаивного домена, что позволяет использовать один интсанс IGP протокола в трех ЦОД;
  - возможность использовать собственные оптические линии связи, что предполагает достаточную полосу пропускания между POD;
  - структурирование конфигурации с помощью отдельных AS, что позволяет увеличивать число spine в одном ЦОД без необходимости внесения изменений в конфигурацию в другом. 


#####  2. Схема uderlay сети.

![](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20multipod/underlay%20multipod.PNG)

#####  3. Схема overlay  сети.

![](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20multipod/overlay%20multipod.PNG)



##### 4. Адресное пространство. 

#####   DCI линк

интерфейсы p2p  между  A-B  - 10.1.1.0/24 
интерфейсы p2p  между  B-C  - 10.1.3.0/24 
интерфейсы p2p  между  C-A  - 10.1.2.0/24 

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

##### 5. Конфигурация оборудования  на стенде

Конифигурация выполнена в на стенде http://10.120.0.13/#/login. 

##### 6. Листинг  конфигурации оборудования 

[a-nxos-sp1](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20multipod/a-nxos-sp1.md)

[a-nxos-sp2](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20multipod/a-nxos-sp2.md)

[a-nxos-lf3](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20multipod/a-nxos-lf3.md)

[a-esx-01](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20multipod/a-esx-01.md)

[b-nxos-sp1](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20multipod/b-nxos-sp1.md)

[b-nxos-sp2](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20multipod/b-nxos-sp2.md)

[b-nxos-lf3](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20multipod/b-nxos-lf3.md)

[b-wf-wan11](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20multipod/b-fw-wan11.md)

[c-nxos-sp1](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20multipod/c-nxos-sp1.md)

[c-nxos-sp2](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20multipod/c-nxos-sp2.md)

[c-db-01](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20multipod/c-db-01.md)

[c-lb-01](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20multipod/c-lb-01.md)

[c-sw5](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20multipod/c-sw5.md)
