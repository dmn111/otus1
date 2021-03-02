### 					  						Конфигурация  L2  оverlay сети с использованием  технологии vxlan evpn type-2	



#### Схема сети:

![](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20type-2/vxlan%20evpn%20type-2.PNG)

#### Описание сети

 	Построена модель overlay сети на оcнове технологии vxlan evpn type-2 на базе uderlay сети CLOS в рамках одного ЦОД.  Для моделирования  выбран один  L2 сегмент распределенный на три Leaf. В качестве конечных устройств выступают интрефейс SVI (ip -192.168.10.10/24)  коммутатора sw10 и   интерфейс e0/0 (ip -192.168.10.7/24) маршрутизатора R7. Для резервирования подключения коммутатора SW10  на уровне L2 использована технолгия vPC ( mlag), для резервирования L3 шлюза технология anycast-gateway. В результате конфигруции оборудования достигнуты целевыепоказатели.

L2  связность обеспечена:

<pre><code>
R7#ping 192.168.10.10 repeat 10
Type escape sequence to abort.
Sending 10, 100-byte ICMP Echos to 192.168.10.10, timeout is 2 seconds:
!!!!!!!!!!
Success rate is 100 percent (10/10), round-trip min/avg/max = 88/165/303 ms
R7#sh arp
Protocol  Address          Age (min)  Hardware Addr   Type   Interface
Internet  192.168.10.1            6   0000.0000.0010  ARPA   Ethernet0/0
Internet  192.168.10.7            -   aabb.cc00.a000  ARPA   Ethernet0/0
Internet  192.168.10.10          43   aabb.cc80.5000  ARPA   Ethernet0/0
</code></pre>


Таблица маршрутов адресного пространства  l2vpn evpn  протокола bgp  содержит маршруты MAC + IP к граничным хостам

- a-nxos-lf7

<pre><code>
a-nxos-lf7# sh bgp l2vpn evpn vni-id 10010
BGP routing table information for VRF default, address family L2VPN EVPN
BGP table version is 1536, Local Router ID is 10.77.253.7
Status: s-suppressed, x-deleted, S-stale, d-dampened, h-history, *-valid, >-best
Path type: i-internal, e-external, c-confed, l-local, a-aggregate, r-redist, I-injected
Origin codes: i - IGP, e - EGP, ? - incomplete, | - multipath, & - backup, 2 - best2
   Network            Next Hop            Metric     LocPrf     Weight Path
Route Distinguisher: 10.77.253.7:32777    (L2VNI 10010)
*>l[2]:[0]:[0]:[48]:[aabb.cc00.a000]:[0]:[0.0.0.0]/216
                      10.77.253.7                       100      32768 i
* i[2]:[0]:[0]:[48]:[aabb.cc80.5000]:[0]:[0.0.0.0]/216
                      10.77.253.5                       100          0 i
*>i                   10.77.253.5                       100          0 i
*>l[2]:[0]:[0]:[48]:[aabb.cc00.a000]:[32]:[192.168.10.7]/248
                      10.77.253.7                       100      32768 i
* i[2]:[0]:[0]:[48]:[aabb.cc80.5000]:[32]:[192.168.10.10]/248
                      10.77.253.5                       100          0 i
*>i                   10.77.253.5                       100          0 i
*>i[3]:[0]:[32]:[10.77.253.5]/88
                      10.77.253.5                       100          0 i
* i                   10.77.253.5                       100          0 i
*>l[3]:[0]:[32]:[10.77.253.7]/88
                      10.77.253.7                       100      32768 i
</code></pre>

- a-nxos-lf3
<pre><code>
a-nxos-lf3# sh bgp l2vpn evpn vni-id 10010
BGP routing table information for VRF default, address family L2VPN EVPN
BGP table version is 83, Local Router ID is 10.77.253.3
Status: s-suppressed, x-deleted, S-stale, d-dampened, h-history, *-valid, >-best
Path type: i-internal, e-external, c-confed, l-local, a-aggregate, r-redist, I-injected
Origin codes: i - IGP, e - EGP, ? - incomplete, | - multipath, & - backup, 2 - best2
   Network            Next Hop            Metric     LocPrf     Weight Path
Route Distinguisher: 10.77.253.3:32777    (L2VNI 10010)
*>l[2]:[0]:[0]:[48]:[aabb.cc00.5000]:[0]:[0.0.0.0]/216
                      10.77.253.5                       100      32768 i
*>l[2]:[0]:[0]:[48]:[aabb.cc00.5010]:[0]:[0.0.0.0]/216
                      10.77.253.5                       100      32768 i
*>i[2]:[0]:[0]:[48]:[aabb.cc00.a000]:[0]:[0.0.0.0]/216
                      10.77.253.7                       100          0 i
*>l[2]:[0]:[0]:[48]:[aabb.cc80.5000]:[0]:[0.0.0.0]/216
                      10.77.253.5                       100      32768 i
*>i[2]:[0]:[0]:[48]:[aabb.cc00.a000]:[32]:[192.168.10.7]/248
                      10.77.253.7                       100          0 i
*>l[2]:[0]:[0]:[48]:[aabb.cc80.5000]:[32]:[192.168.10.10]/248
                      10.77.253.5                       100      32768 i
* i[3]:[0]:[32]:[10.77.253.5]/88
                      10.77.253.5                       100          0 i
*>l                   10.77.253.5                       100      32768 i
*>i[3]:[0]:[32]:[10.77.253.7]/88
                      10.77.253.7                       100          0 i
</code></pre>

- a-nxos-lf4
<pre><code>
a-nxos-lf4# sh bgp l2vpn evpn vni-id 10010
BGP routing table information for VRF default, address family L2VPN EVPN
BGP table version is 422, Local Router ID is 10.77.253.4
Status: s-suppressed, x-deleted, S-stale, d-dampened, h-history, *-valid, >-best
Path type: i-internal, e-external, c-confed, l-local, a-aggregate, r-redist, I-injected
Origin codes: i - IGP, e - EGP, ? - incomplete, | - multipath, & - backup, 2 - best2
   Network            Next Hop            Metric     LocPrf     Weight Path
Route Distinguisher: 10.77.253.4:32777    (L2VNI 10010)
*>i[2]:[0]:[0]:[48]:[aabb.cc00.a000]:[0]:[0.0.0.0]/216
                      10.77.253.7                       100          0 i
*>l[2]:[0]:[0]:[48]:[aabb.cc80.5000]:[0]:[0.0.0.0]/216
                      10.77.253.5                       100      32768 i
*>i[2]:[0]:[0]:[48]:[aabb.cc00.a000]:[32]:[192.168.10.7]/248
                      10.77.253.7                       100          0 i
*>l[2]:[0]:[0]:[48]:[aabb.cc80.5000]:[32]:[192.168.10.10]/248
                      10.77.253.5                       100      32768 i
*>l[3]:[0]:[32]:[10.77.253.5]/88
                      10.77.253.5                       100      32768 i
*>i[3]:[0]:[32]:[10.77.253.7]/88
                      10.77.253.7                       100          0 i
</code></pre>

- a-nxos-sp1

<pre><code>
a-nxos-sp1# sh bgp l2vpn evpn 
BGP routing table information for VRF default, address family L2VPN EVPN
BGP table version is 818, Local Router ID is 10.77.255.1
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
*>i[2]:[0]:[0]:[48]:[aabb.cc80.5000]:[32]:[192.168.10.10]/248
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
*>i[3]:[0]:[32]:[10.77.253.5]/88
                      10.77.253.5                       100          0 i

Route Distinguisher: 10.77.253.7:32777
*>i[2]:[0]:[0]:[48]:[aabb.cc00.a000]:[0]:[0.0.0.0]/216
                      10.77.253.7                       100          0 i
*>i[2]:[0]:[0]:[48]:[aabb.cc00.a000]:[32]:[192.168.10.7]/248
                      10.77.253.7                       100          0 i
*>i[3]:[0]:[32]:[10.77.253.7]/88
                      10.77.253.7                       100          0 i

</code></pre>

Таблицы  mac адресов на leaf  (команда для nexus v9000)

<pre><code>
a-nxos-lf3# sh show system internal l2fwder mac
Legend: 
* - primary entry, G - Gateway MAC, (R) - Routed MAC, O - Overlay MAC
age - seconds since last seen,+ - primary entry using vPC Peer-Link,
(T) - True, (F) - False, C - ControlPlane MAC
   VLAN     MAC Address      Type      age     Secure NTFY Ports
---------+-----------------+--------+---------+------+----+------------------
*    10    aabb.cc00.a000    static   -          F     F  nve-peer1 10.77.253.7  
*    10    aabb.cc00.5010   dynamic   00:01:59   F     F       Po10  
*    10    5000.0004.0007    static   -          F     F        Po5  
+    10    aabb.cc00.5000   dynamic   00:01:59   F     F       Po10  
G     -    5000:0003:0007    static   -          F     F   sup-eth1(R)
G    10    5000.0003.0007    static   -          F     F   sup-eth1(R)
*    10    aabb.cc80.5000   dynamic   00:09:40   F     F       Po10  
    1           1         -00:00:00:00:00:10         -             1

</code></pre>

<pre><code>
a-nxos-lf7# show system internal l2fwder mac
Legend: 
        * - primary entry, G - Gateway MAC, (R) - Routed MAC, O - Overlay MAC
        age - seconds since last seen,+ - primary entry using vPC Peer-Link,
        (T) - True, (F) - False, C - ControlPlane MAC
   VLAN     MAC Address      Type      age     Secure NTFY Ports
---------+-----------------+--------+---------+------+----+------------------
*    10    aabb.cc00.a000   dynamic   22:38:11   F     F     Eth1/1  
G    10    5000.0009.0007    static   -          F     F   sup-eth1(R)
*    10    aabb.cc00.5010    static   -          F     F  nve-peer1 10.77.253.5  
*    10    aabb.cc00.5000    static   -          F     F  nve-peer1 10.77.253.5  
*    10    aabb.cc80.5000    static   -          F     F  nve-peer1 10.77.253.5  
    1           1         -00:00:00:00:00:10         -             1

</code></pre>

Состояние vpc:
<pre><code>
a-nxos-lf3# sh vpc brief 
Legend:
(*) - local vPC is down, forwarding via vPC peer-link
vPC domain id                     : 1   
Peer status                       : peer adjacency formed ok      
vPC keep-alive status             : peer is alive                 
Configuration consistency status  : success 
Per-vlan consistency status       : success                       
Type-2 consistency status         : success 
vPC role                          : primary, operational secondary
Number of vPCs configured         : 1   
Peer Gateway                      : Disabled
Dual-active excluded VLANs        : -
Graceful Consistency Check        : Enabled
Auto-recovery status              : Disabled
Delay-restore status              : Timer is off.(timeout = 30s)
Delay-restore SVI status          : Timer is off.(timeout = 10s)
Operational Layer3 Peer-router    : Disabled
vPC Peer-link status
---------------------------------------------------------------------
id    Port   Status Active vlans    
--    ----   ------ -------------------------------------------------
1     Po5    up     1,10                                                                 
vPC status
----------------------------------------------------------------------------
Id    Port          Status Consistency Reason                Active vlans
--    ------------  ------ ----------- ------                ---------------
10    Po10          up     success     success               10                          
</code></pre>

Состояние   nve peers 

<pre><code>
a-nxos-lf7# sh nve peers 
Interface Peer-IP          State LearnType Uptime   Router-Mac       
--------- ---------------  ----- --------- -------- -----------------
nve1      10.77.253.5      Up    CP        22:37:04 n/a         
</code></pre>

Сотсояние   nve vni (c включенным Suppress ARP )

<pre><code>
a-nxos-lf7# sh nve vni 
Codes: CP - Control Plane        DP - Data Plane          
       UC - Unconfigured         SA - Suppress ARP        
       SU - Suppress Unknown Unicast 
       Xconn - Crossconnect      
       MS-IR - Multisite Ingress Replication
Interface VNI      Multicast-group   State Mode Type [BD/VRF]      Flags
--------- -------- ----------------- ----- ---- ------------------ -----
nve1      10010    UnicastBGP        Up    CP   L2 [10]            SA   
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

#####   Граничные утройтсва подключенные к CLOS

| Оборудование | Интерфейс | Ip адрес        |
| :----------: | --------- | --------------- |
|      R7      | e0/0      | 192.168.10.7/24 |

| Оборудование | Интерфейс | Ip адрес         |
| :----------: | --------- | ---------------- |
|     sw10     | vlan10    | 192.168.10.10/24 |



##### Конфигурация оборудования :

[a-nxos-sp1](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20type-2/a-nxos-sp1.conf.md)

[a-nxos-sp2](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20type-2/a-nxos-sp2.conf.md)

[a-nxos-lf3](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20type-2/a-nxos-lf3.conf.md)

[a-nxos-lf4](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20type-2/a-nxos-lf4.conf.md)

[a-nxos-lf7](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20type-2/a-nxos-lf7.conf.md)

[R10](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20type-2/R10.conf.md)

[SW10](https://github.com/dmn111/otus1/blob/master/vxlan%20evpn%20type-2/SW10.conf.md)
