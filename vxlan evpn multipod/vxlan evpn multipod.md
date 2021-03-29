####  																									Проектная работа 

#### 			Объединение трех ЦОД по технологии VXLAN Multipod

#####   Задание.

	Выбрать технологию и реализовать модель сетевой схемы  для  объединения трех ЦОД.     Для  ЦОД имеются следующие исходные данные;  ЦОД  географически расположенные в одном населенном пункте и объединенные между собой собственной оптической линией связи. Необходимо обеспечить L2 связность между ЦОД. ЦОД  не в полной мере укомплектованы оборудованием для релизациия полноценной схемы CLOS.
	 В рамках  задания выпонить следующие пункты:

 1.	Обосновать выбор  overlay технологии для объединения ЦОД.
 2. Изобразить схему  underlay сети
 3. Изобразить схему overlay сети
 4. Адресное пространсвтво underaly и overlay сети для каждого ЦОД
 5. Конфигурация оборудования

#### Выбор  технологии

#### Схема uderlay сети:

![](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20multipod/uderlay%20multipod.PNG)

####  Схема overlay  сети:

![](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20multipod/overlay%20multipod.PNG)



#### Адресное пространство 

##### DataCenter A:   

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
|   | e1/5 | 10.1.3.13/30 |underay|default|
| |  |  |||



#####   Граничные утройтсва подключенные к CLOS





| Оборудование | Интерфейс   | Ip адрес               |
| :----------: | ----------- | ---------------------- |
|      R7      | e0/0.20     | 192.168.20.7/24 vrfA   |
|              | e0/0.30     | 192.168.30.7/24 vrfB   |
|              | e0/0.50     | 192.168.50.7/24 vrfC   |
|              | loopback150 | 192.168.150.7/32  vrfC |
|              |             |                        |

| Оборудование | Интерфейс | Ip адрес         |
| :----------: | --------- | ---------------- |
|     sw10     | vlan10    | 192.168.10.10/24 |



##### Конфигурация оборудования :

[a-nxos-sp1](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20type-5/a-nxos-sp1.conf.md)

[a-nxos-sp2](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20type-5/a-nxos-sp2.conf.md)

[a-nxos-lf3](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20type-5/a-nxos-lf3.conf.md)

[a-nxos-lf4](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20type-5/a-nxos-lf4.conf.md)

[a-nxos-lf7](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20type-5/a-nxos-lf7.conf.md)

[R7](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20type-5/R7.conf.md)

[SW10](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20type-5/SW10.conf.md)
