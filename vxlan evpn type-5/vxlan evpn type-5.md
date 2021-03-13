### 					  						Конфигурация  оverlay сети с использованием  технологии vxlan evpn (type-5)



#### Схема сети:

![](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20type-5/vxlan%20evpn%20type-5.PNG)

#### Описание сети

​	Построена модель overlay сети на оcнове технологии vxlan evpn на базе uderlay сети CLOS в рамках одного ЦОД.  В данной работе изучается возможность маршрутизации  между сегментами VNI  рапроложеных на разных Leaf . Для организации маршрутизации используется один L3 VNI присутвующий на всех Leaf  и ассоциированный с опредленным VRF (vrfABC). Данная технология позволяет не дублировать все VNI на все Leaf при этом обеспечивая маршрутизацию между множеством VNI  (благодаря одному L3 VNI).  В  лабораторной работе также рассмотрена возможность передачи маршрутов type-5 в адресном пространстве l2vpn evpn  объявленным (advertise) из  адресного простраства  ipv4 unicast

​	 	В качестве конечных устройств выступают интрефейс SVI (ip -192.168.10.50/24) коммутатора sw10 и   интерфейсы  e0/0.20 (ip -192.168.20.7/24 vrfA),  e0/0.30 ( ip - 192.168.30.7/24 vrfB),  e0/0.50 (ip - 192.168.50.7/24 vrfC) маршрутизатора R7. 

В результате настройки L3 VNI 10999 ассоцированного  c VRF  vrfABC  на каждом leaf  обеспечена  маршрутизация между VNI 10010 (Leaf3, Leaf4) и  VNI 10020 (Leaf7), VNI 10030 (Leaf7).

L3  связность между  sw10 и R7 достигнута:
<pre><code>
sw10#ping  192.168.20.7
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.20.7, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 49/84/120 ms
sw10#ping  192.168.30.7
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.30.7, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 63/118/155 ms
</code></pre>

Таблица маршрутов адресного пространства  l2vpn evpn  протокола bgp  

- a-nxos-lf7

<pre><code>
a-nxos-lf7# sh bgp l2vpn evpn
BGP routing table information for VRF default, address family L2VPN EVPN
BGP table version is 14599, Local Router ID is 10.77.253.7
Status: s-suppressed, x-deleted, S-stale, d-dampened, h-history, *-valid, >-best
Path type: i-internal, e-external, c-confed, l-local, a-aggregate, r-redist, I-injected
Origin codes: i - IGP, e - EGP, ? - incomplete, | - multipath, & - backup, 2 - best2
   Network            Next Hop            Metric     LocPrf     Weight Path
Route Distinguisher: 10.77.253.3:32777
*>i[2]:[0]:[0]:[48]:[aabb.cc00.5000]:[0]:[0.0.0.0]/216
                      10.77.253.5                       100          0 i
*>i[2]:[0]:[0]:[48]:[aabb.cc00.5010]:[0]:[0.0.0.0]/216
                      10.77.253.5                       100          0 i
*>i[2]:[0]:[0]:[48]:[aabb.cc80.5000]:[0]:[0.0.0.0]/216
                      10.77.253.5                       100          0 i
*>i[2]:[0]:[0]:[48]:[aabb.cc80.5000]:[32]:[192.168.10.50]/272
                      10.77.253.5                       100          0 i
*>i[3]:[0]:[32]:[10.77.253.5]/88
                      10.77.253.5                       100          0 i

Route Distinguisher: 10.77.253.4:32777
*>i[2]:[0]:[0]:[48]:[aabb.cc00.5000]:[0]:[0.0.0.0]/216
                      10.77.253.5                       100          0 i
*>i[2]:[0]:[0]:[48]:[aabb.cc00.5010]:[0]:[0.0.0.0]/216
                      10.77.253.5                       100          0 i
*>i[2]:[0]:[0]:[48]:[aabb.cc80.5000]:[0]:[0.0.0.0]/216
                      10.77.253.5                       100          0 i
*>i[2]:[0]:[0]:[48]:[aabb.cc80.5000]:[32]:[192.168.10.50]/272
                      10.77.253.5                       100          0 i
*>i[3]:[0]:[32]:[10.77.253.5]/88
                      10.77.253.5                       100          0 i

Route Distinguisher: 10.77.253.7:32787    (L2VNI 10020)
*>l[2]:[0]:[0]:[48]:[aabb.cc00.a000]:[0]:[0.0.0.0]/216
                      10.77.253.7                       100      32768 i
*>l[2]:[0]:[0]:[48]:[aabb.cc00.a000]:[32]:[192.168.20.7]/272
                      10.77.253.7                       100      32768 i
*>l[3]:[0]:[32]:[10.77.253.7]/88
                      10.77.253.7                       100      32768 i

Route Distinguisher: 10.77.253.7:32797    (L2VNI 10030)
*>l[2]:[0]:[0]:[48]:[aabb.cc00.a000]:[0]:[0.0.0.0]/216
                      10.77.253.7                       100      32768 i
*>l[2]:[0]:[0]:[48]:[aabb.cc00.a000]:[32]:[192.168.30.7]/272
                      10.77.253.7                       100      32768 i
*>l[3]:[0]:[32]:[10.77.253.7]/88
                      10.77.253.7                       100      32768 i

Route Distinguisher: 10.77.253.7:3    (L3VNI 10999)
*>l[5]:[0]:[0]:[24]:[192.168.90.0]/224
                      10.77.253.7                       100      32768 i
*>l[5]:[0]:[0]:[24]:[192.168.130.0]/224
                      10.77.253.7              0        100      32768 ?
*>l[5]:[0]:[0]:[24]:[192.168.150.0]/224
                      10.77.253.7              0        100      32768 ?

</code></pre>



- a-nxos-lf3
<pre><code>
a-nxos-lf3# sh bgp  l2vpn evpn
BGP routing table information for VRF default, address family L2VPN EVPN
BGP table version is 9692, Local Router ID is 10.77.253.3
Status: s-suppressed, x-deleted, S-stale, d-dampened, h-history, *-valid, >-best
Path type: i-internal, e-external, c-confed, l-local, a-aggregate, r-redist, I-injected
Origin codes: i - IGP, e - EGP, ? - incomplete, | - multipath, & - backup, 2 - best2
   Network            Next Hop            Metric     LocPrf     Weight Path
Route Distinguisher: 10.77.253.3:32777    (L2VNI 10010)
*>l[2]:[0]:[0]:[48]:[aabb.cc00.5000]:[0]:[0.0.0.0]/216
                      10.77.253.5                       100      32768 i
*>l[2]:[0]:[0]:[48]:[aabb.cc00.5010]:[0]:[0.0.0.0]/216
                      10.77.253.5                       100      32768 i
*>l[2]:[0]:[0]:[48]:[aabb.cc80.5000]:[0]:[0.0.0.0]/216
                      10.77.253.5                       100      32768 i
*>l[2]:[0]:[0]:[48]:[aabb.cc80.5000]:[32]:[192.168.10.50]/272
                      10.77.253.5                       100      32768 i
*>l[3]:[0]:[32]:[10.77.253.5]/88
                      10.77.253.5                       100      32768 i

Route Distinguisher: 10.77.253.7:3
*>i[5]:[0]:[0]:[24]:[192.168.90.0]/224
                      10.77.253.7                       100          0 i
*>i[5]:[0]:[0]:[24]:[192.168.130.0]/224
                      10.77.253.7              0        100          0 ?
*>i[5]:[0]:[0]:[24]:[192.168.150.0]/224
                      10.77.253.7              0        100          0 ?

Route Distinguisher: 10.77.253.7:32787
*>i[2]:[0]:[0]:[48]:[aabb.cc00.a000]:[32]:[192.168.20.7]/272
                      10.77.253.7                       100          0 i

Route Distinguisher: 10.77.253.7:32797
*>i[2]:[0]:[0]:[48]:[aabb.cc00.a000]:[32]:[192.168.30.7]/272
                      10.77.253.7                       100          0 i

</code></pre>

Маршруты type-5 сформированы  на результате объявления статитческих маршрутов адресного простраства  ipv4 unicast на маршрутизаторе a-nxos-lf7 двумя способами: 
<pre><code>
 address-family ipv4 unicast
      network 192.168.90.0/24
      redistribute static route-map rmRedistStatic
</code></pre>

Таблица маршрутизации  на  a-nxos-lf7  vrfABC
<pre><code>
a-nxos-lf7# sh ip route vrf vrfABC
IP Route Table for VRF "vrfABC"
'*' denotes best ucast next-hop
'**' denotes best mcast next-hop
'[x/y]' denotes [preference/metric]
'%<string>' in via output denotes VRF <string>

192.168.10.50/32, ubest/mbest: 1/0
    *via 10.77.253.5%default, [200/0], 17:05:29, bgp-65000, internal, tag 65000 (evpn) segid: 10999
tunnelid: 0xa4dfd05 encap: VXLAN

192.168.20.0/24, ubest/mbest: 1/0, attached
    *via 192.168.20.1, Vlan20, [0/0], 3d15h, direct
192.168.20.1/32, ubest/mbest: 1/0, attached
    *via 192.168.20.1, Vlan20, [0/0], 3d15h, local
192.168.20.7/32, ubest/mbest: 1/0, attached
    *via 192.168.20.7, Vlan20, [190/0], 3d15h, hmm
192.168.30.0/24, ubest/mbest: 1/0, attached
    *via 192.168.30.1, Vlan30, [0/0], 16:34:30, direct
192.168.30.1/32, ubest/mbest: 1/0, attached
    *via 192.168.30.1, Vlan30, [0/0], 16:34:30, local
192.168.30.7/32, ubest/mbest: 1/0, attached
    *via 192.168.30.7, Vlan30, [190/0], 16:29:09, hmm
192.168.50.0/24, ubest/mbest: 1/0, attached
    *via 192.168.50.1, Vlan50, [0/0], 00:45:31, direct
192.168.50.1/32, ubest/mbest: 1/0, attached
    *via 192.168.50.1, Vlan50, [0/0], 00:45:31, local
192.168.90.0/24, ubest/mbest: 1/0, attached
    *via 192.168.90.1, Lo90, [0/0], 02:24:41, direct
192.168.90.1/32, ubest/mbest: 1/0, attached
    *via 192.168.90.1, Lo90, [0/0], 02:24:41, local
192.168.130.0/24, ubest/mbest: 1/0
    *via Null0, [1/0], 01:01:04, static
192.168.150.0/24, ubest/mbest: 1/0
    *via 192.168.50.7, [1/0], 00:42:28, static

</code></pre>


Отдельно  таблица маршрутов type-5 на a-nxos-lf3.

<pre><code>
a-nxos-lf3#  sh bgp  l2vpn evpn route-type 5
BGP routing table information for VRF default, address family L2VPN EVPN
Route Distinguisher: 10.77.253.7:3
BGP routing table entry for [5]:[0]:[0]:[24]:[192.168.90.0]/224, version 9630
Paths: (1 available, best #1)
Flags: (0x000002) (high32 00000000) on xmit-list, is not in l2rib/evpn, is not in HW
  Advertised path-id 1
  Path type: internal, path is valid, is best path, no labeled nexthop
             Imported to 1 destination(s)
  Gateway IP: 0.0.0.0
  AS-Path: NONE, path sourced internal to AS
    10.77.253.7 (metric 81) from 10.77.255.1 (10.77.255.1)
      Origin IGP, MED not set, localpref 100, weight 0
      Received label 10999
      Extcommunity: RT:65000:10999 ENCAP:8 Router MAC:5000.0009.0007
      Originator: 10.77.253.7 Cluster list: 10.77.255.1
  Path-id 1 not advertised to any peer
BGP routing table entry for [5]:[0]:[0]:[24]:[192.168.130.0]/224, version 9675
Paths: (1 available, best #1)
Flags: (0x000002) (high32 00000000) on xmit-list, is not in l2rib/evpn, is not in HW
  Advertised path-id 1
  Path type: internal, path is valid, is best path, no labeled nexthop
             Imported to 1 destination(s)
  Gateway IP: 0.0.0.0
  AS-Path: NONE, path sourced internal to AS
    10.77.253.7 (metric 81) from 10.77.255.1 (10.77.255.1)
      Origin incomplete, MED 0, localpref 100, weight 0
      Received label 10999
      Extcommunity: RT:65000:10999 ENCAP:8 Router MAC:5000.0009.0007
      Originator: 10.77.253.7 Cluster list: 10.77.255.1
  Path-id 1 not advertised to any peer
BGP routing table entry for [5]:[0]:[0]:[24]:[192.168.150.0]/224, version 9678
Paths: (1 available, best #1)
Flags: (0x000002) (high32 00000000) on xmit-list, is not in l2rib/evpn, is not in HW
  Advertised path-id 1
  Path type: internal, path is valid, is best path, no labeled nexthop
             Imported to 1 destination(s)
  Gateway IP: 0.0.0.0
  AS-Path: NONE, path sourced internal to AS
    10.77.253.7 (metric 81) from 10.77.255.1 (10.77.255.1)
      Origin incomplete, MED 0, localpref 100, weight 0
      Received label 10999
      Extcommunity: RT:65000:10999 ENCAP:8 Router MAC:5000.0009.0007
      Originator: 10.77.253.7 Cluster list: 10.77.255.1
  Path-id 1 not advertised to any peer
</code></pre>

Проверка доступности L3 сегментов распространяемых с помощью update type-5 c конечного устройства sw10
<code><pre>
sw10#ping 192.168.150.7
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.150.7, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 80/94/118 ms

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

| Оборудование | Интерфейс   | Ip адрес               |
| :----------: | ----------- | ---------------------- |
|  a-nxos-lf3  | loopback1   | 10.77.255.3/32         |
|              | e1/1        |                        |
|              | e1/2        | 10.77.1.2/30           |
|              | e1/3        | 10.77.2.6/30           |
|              | e1/4        |                        |
|              | int vlan 10 | 192.168.10.1/24 vrfABC |
|              |             |                        |
|              |             |                        |

| Оборудование | Интерфейс   | Ip адрес               |
| :----------: | ----------- | ---------------------- |
|  a-nxos-lf4  | loopback1   | 10.77.255.4/32         |
|              | e1/1        |                        |
|              | e1/2        | 10.77.2.2/30           |
|              | e1/3        | 10.77.1.6/30           |
|              | e1/4        |                        |
|              | int vlan 10 | 192.168.10.1/24 vrfABC |
|              |             |                        |

| Оборудование | Интерфейс   | Ip адрес               |
| :----------: | ----------- | ---------------------- |
|  a-nxos-lf7  | loopback1   | 10.77.255.7/32         |
|              | e1/1        |                        |
|              | e1/4        | 10.77.1.10/30          |
|              | e1/5        | 10.77.2.10/30          |
|              | int vlan 20 | 192.168.20.1/24 vrfABC |
|              | int vlan 30 | 192.168.30.1/24 vrfABC |
|              | int vlan 50 | 192.168.50.1/24 vrfABC |
|              | loopback90  | 192.168.90.1/24 vrfABC |

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

[R10](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20type-5/R10.conf.md)

[SW10](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20type-5/SW10.conf.md)
