#### 			Объединение ЦОД по технологии VXLAN Multipod


#####   Схема uderlay сети.

![](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20multipod%20v2/underlay%20multipod.PNG)

#####   Схема overlay  сети.

![](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20multipod%20v2/overlay%20multipod.PNG)

#####  Описание.

В работе  рассматривается вариант объединения двух CLOS сетей по технологий VxLAN  Multipod. Каждый POD, с точки зрения протокола BGP адресного прострастава  l2vpn, находится в отдельной автономной системе: ЦОД "A" в AS65001  и ЦОД  "B" в AS65002.   IP свзяность  между  всеми узлами обеспечивается за счет протокола  OSPF (внутри POD и между POD)  рамках одного instance. Для обеспечения  корректного импорта/экспрота маршрутов  адресного протсрантства  l2vpn  находящихся в разных автономных ситстемах  используется ручное конфигурирование  route target.

Целевые показатели достигнуты :

- маршруты  route-type 2 и route-type 5 между  Leaf  в разных POD:

<pre><code>
b-nxos-lf3# sh bgp l2vpn evpn
BGP routing table information for VRF default, address family L2VPN EVPN
BGP table version is 91, Local Router ID is 10.66.255.3
Status: s-suppressed, x-deleted, S-stale, d-dampened, h-history, *-valid, >-best
Path type: i-internal, e-external, c-confed, l-local, a-aggregate, r-redist, I-injected
Origin codes: i - IGP, e - EGP, ? - incomplete, | - multipath, & - backup, 2 - best2
   Network            Next Hop            Metric     LocPrf     Weight Path
Route Distinguisher: 10.66.255.3:32777    (L2VNI 10010)
*>i[2]:[0]:[0]:[48]:[aabb.cc00.b000]:[0]:[0.0.0.0]/216
                      10.77.253.3                       100          0 65001 i
*>l[2]:[0]:[0]:[48]:[aabb.cc00.a000]:[32]:[10.90.10.61]/272
                      10.66.253.3                       100      32768 i
*>i[2]:[0]:[0]:[48]:[aabb.cc00.b000]:[32]:[10.90.10.71]/272
                      10.77.253.3                       100          0 65001 i
*>l[3]:[0]:[32]:[10.66.253.3]/88
                      10.66.253.3                       100      32768 i
*>i[3]:[0]:[32]:[10.77.253.3]/88
                      10.77.253.3                       100          0 65001 i
Route Distinguisher: 10.66.255.3:32817    (L2VNI 10050)
*>i[2]:[0]:[0]:[48]:[aabb.cc00.b000]:[0]:[0.0.0.0]/216
                      10.77.253.3                       100          0 65001 i
*>l[2]:[0]:[0]:[48]:[aabb.cc00.d000]:[0]:[0.0.0.0]/216
                      10.66.253.3                       100      32768 i
*>l[3]:[0]:[32]:[10.66.253.3]/88
                      10.66.253.3                       100      32768 i
*>i[3]:[0]:[32]:[10.77.253.3]/88
                      10.77.253.3                       100          0 65001 i
Route Distinguisher: 10.77.255.3:32777
*>i[2]:[0]:[0]:[48]:[aabb.cc00.b000]:[0]:[0.0.0.0]/216
                      10.77.253.3                       100          0 65001 i
*>i[2]:[0]:[0]:[48]:[aabb.cc00.b000]:[32]:[10.90.10.71]/272
                      10.77.253.3                       100          0 65001 i
*>i[3]:[0]:[32]:[10.77.253.3]/88
                      10.77.253.3                       100          0 65001 i
Route Distinguisher: 10.77.255.3:32787
*>i[2]:[0]:[0]:[48]:[aabb.cc00.b000]:[0]:[0.0.0.0]/216
                      10.77.253.3                       100          0 65001 i
*>i[2]:[0]:[0]:[48]:[aabb.cc00.b000]:[32]:[10.90.20.71]/272
                      10.77.253.3                       100          0 65001 i
*>i[3]:[0]:[32]:[10.77.253.3]/88
                      10.77.253.3                       100          0 65001 i
Route Distinguisher: 10.77.255.3:32817
*>i[2]:[0]:[0]:[48]:[aabb.cc00.b000]:[0]:[0.0.0.0]/216
                      10.77.253.3                       100          0 65001 i
*>i[3]:[0]:[32]:[10.77.253.3]/88
                      10.77.253.3                       100          0 65001 i
Route Distinguisher: 10.66.255.3:4    (L3VNI 10999)
*>l[5]:[0]:[0]:[24]:[10.90.50.0]/224
                      10.66.253.3             50        100      32768 ?

</code></pre>

<pre><code>
a-nxos-lf3# sh bgp l2vpn evpn
BGP routing table information for VRF default, address family L2VPN EVPN
BGP table version is 112, Local Router ID is 10.77.255.3
Status: s-suppressed, x-deleted, S-stale, d-dampened, h-history, *-valid, >-best
Path type: i-internal, e-external, c-confed, l-local, a-aggregate, r-redist, I-injected
Origin codes: i - IGP, e - EGP, ? - incomplete, | - multipath, & - backup, 2 - best2
   Network            Next Hop            Metric     LocPrf     Weight Path
Route Distinguisher: 10.66.255.3:4
*>i[5]:[0]:[0]:[24]:[10.90.50.0]/224
                      10.66.253.3                       100          0 65002 ?
Route Distinguisher: 10.66.255.3:32777
*>i[2]:[0]:[0]:[48]:[aabb.cc00.a000]:[32]:[10.90.10.61]/272
                      10.66.253.3                       100          0 65002 i
*>i[3]:[0]:[32]:[10.66.253.3]/88
                      10.66.253.3                       100          0 65002 i
Route Distinguisher: 10.66.255.3:32817
*>i[2]:[0]:[0]:[48]:[aabb.cc00.d000]:[0]:[0.0.0.0]/216
                      10.66.253.3                       100          0 65002 i
*>i[3]:[0]:[32]:[10.66.253.3]/88
                      10.66.253.3                       100          0 65002 i
Route Distinguisher: 10.77.255.3:32777    (L2VNI 10010)
*>l[2]:[0]:[0]:[48]:[aabb.cc00.b000]:[0]:[0.0.0.0]/216
                      10.77.253.3                       100      32768 i
*>i[2]:[0]:[0]:[48]:[aabb.cc00.a000]:[32]:[10.90.10.61]/272
                      10.66.253.3                       100          0 65002 i
*>l[2]:[0]:[0]:[48]:[aabb.cc00.b000]:[32]:[10.90.10.71]/272
                      10.77.253.3                       100      32768 i
*>i[3]:[0]:[32]:[10.66.253.3]/88
                      10.66.253.3                       100          0 65002 i
*>l[3]:[0]:[32]:[10.77.253.3]/88
                      10.77.253.3                       100      32768 i
Route Distinguisher: 10.77.255.3:32787    (L2VNI 10020)
*>l[2]:[0]:[0]:[48]:[aabb.cc00.b000]:[0]:[0.0.0.0]/216
                      10.77.253.3                       100      32768 i
*>l[2]:[0]:[0]:[48]:[aabb.cc00.b000]:[32]:[10.90.20.71]/272
                      10.77.253.3                       100      32768 i
*>l[3]:[0]:[32]:[10.77.253.3]/88
                      10.77.253.3                       100      32768 i
Route Distinguisher: 10.77.255.3:32817    (L2VNI 10050)
*>l[2]:[0]:[0]:[48]:[aabb.cc00.b000]:[0]:[0.0.0.0]/216
                      10.77.253.3                       100      32768 i
*>i[2]:[0]:[0]:[48]:[aabb.cc00.d000]:[0]:[0.0.0.0]/216
                      10.66.253.3                       100          0 65002 i
*>i[3]:[0]:[32]:[10.66.253.3]/88
                      10.66.253.3                       100          0 65002 i
*>l[3]:[0]:[32]:[10.77.253.3]/88
                      10.77.253.3                       100      32768 i
</code></pre>
 - клиенты имеют L2 и L3   связность 
<pre><code>
a-esx-01#ping vrf vrfA 10.90.10.61
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.90.10.61, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 19/35/69 ms
a-esx-01#
a-esx-01#ping vrf vrfA 10.90.50.1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.90.50.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 18/31/61 ms
a-esx-01#ping vrf vrfA 10.90.50.71
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.90.50.71, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 48/51/56 ms
</code></pre>

- таблица марщрутизации overlay на Leaf
<pre><code>
b-nxos-lf3# sh ip route vrf vrfLAN
IP Route Table for VRF "vrfLAN"
'*' denotes best ucast next-hop
'**' denotes best mcast next-hop
'[x/y]' denotes [preference/metric]
'%<string>' in via output denotes VRF <string>
10.90.10.0/24, ubest/mbest: 1/0, attached
    *via 10.90.10.1, Vlan10, [0/0], 03:19:22, direct
10.90.10.1/32, ubest/mbest: 1/0, attached
    *via 10.90.10.1, Vlan10, [0/0], 03:19:22, local
10.90.10.61/32, ubest/mbest: 1/0, attached
    *via 10.90.10.61, Vlan10, [190/0], 03:18:15, hmm
10.90.10.71/32, ubest/mbest: 1/0
    *via 10.77.253.3%default, [200/0], 00:50:41, bgp-65002, internal, tag 65001 (evpn) segid: 10999 tunnelid:
0xa4dfd03 encap: VXLAN
10.90.20.71/32, ubest/mbest: 1/0
    *via 10.77.253.3%default, [200/0], 00:50:41, bgp-65002, internal, tag 65001 (evpn) segid: 10999 tunnelid:
0xa4dfd03 encap: VXLAN
10.90.50.0/24, ubest/mbest: 1/0
    *via 10.90.255.1, Vlan111, [110/50], 01:10:35, ospf-5, intra
10.90.255.0/24, ubest/mbest: 1/0, attached
    *via 10.90.255.2, Vlan111, [0/0], 03:25:28, direct
10.90.255.2/32, ubest/mbest: 1/0, attached
    *via 10.90.255.2, Vlan111, [0/0], 03:25:28, local
</code></pre>

<pre><code>
a-nxos-lf3# sh ip route vrf vrfLAN
IP Route Table for VRF "vrfLAN"
'*' denotes best ucast next-hop
'**' denotes best mcast next-hop
'[x/y]' denotes [preference/metric]
'%<string>' in via output denotes VRF <string>
10.90.10.0/24, ubest/mbest: 1/0, attached
    *via 10.90.10.1, Vlan10, [0/0], 02:59:49, direct
10.90.10.1/32, ubest/mbest: 1/0, attached
    *via 10.90.10.1, Vlan10, [0/0], 02:59:49, local
10.90.10.61/32, ubest/mbest: 1/0
    *via 10.66.253.3%default, [200/0], 00:56:04, bgp-65001, internal, tag 65002 (evpn) segid: 10999 tunnelid:
0xa42fd03 encap: VXLAN
10.90.10.71/32, ubest/mbest: 1/0, attached
    *via 10.90.10.71, Vlan10, [190/0], 01:08:24, hmm
10.90.20.0/24, ubest/mbest: 1/0, attached
    *via 10.90.20.1, Vlan20, [0/0], 02:59:49, direct
10.90.20.1/32, ubest/mbest: 1/0, attached
    *via 10.90.20.1, Vlan20, [0/0], 02:59:49, local
10.90.20.71/32, ubest/mbest: 1/0, attached
    *via 10.90.20.71, Vlan20, [190/0], 01:07:59, hmm
10.90.50.0/24, ubest/mbest: 1/0
    *via 10.66.253.3%default, [200/0], 00:56:04, bgp-65001, internal, tag 65002 (evpn) segid: 10999 tunnelid:
0xa42fd03 encap: VXLAN
</code></pre>



#####  Адресное пространство. 

#####   DCI линк

интерфейсы p2p  между  A-B  - 10.1.1.0/24 

#####  DataCenter A:   

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

|       Оборудование       | Интерфейс      | Ip адрес       | Тип сети | VFR  |
| :----------------------: | -------------- | -------------- | -------- | ---- |
|         b-db-01          | Ethernet0/0.10 | 10.90.10.61/24 |          |      |
| модель  DataBase сервера |                |                |          |      |
|                          |                |                |          |      |



##### 6. Листинг  конфигурации оборудования 

[a-nxos-sp1](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20multipod%20v2/a-nxos-sp1.md)

[a-nxos-sp2](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20multipod%20v2/a-nxos-sp2.md)

[a-nxos-lf3](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20multipod%20v2/a-nxos-lf3.md)

[a-esx-01](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20multipod%20v2/a-esx-01.md)

[b-nxos-sp1](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20multipod%20v2/b-nxos-sp1.md)

[b-nxos-sp2](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20multipod%20v2/b-nxos-sp2.md)

[b-nxos-lf3](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20multipod%20v2/b-nxos-lf3.md)

[b-wf-wan11](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20multipod%20v2/b-fw-wan11.md)

[b-db-01](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20multipod%20v2/b-db-01.md)
