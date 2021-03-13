<pre><code>
!Command: show running-config
!Running configuration last done at: Sat Mar 13 10:55:36 2021
!Time: Sat Mar 13 13:43:29 2021

version 9.2(2) Bios:version  
hostname a-nxos-lf7
vdc a-nxos-lf7 id 1
  limit-resource vlan minimum 16 maximum 4094
  limit-resource vrf minimum 2 maximum 4096
  limit-resource port-channel minimum 0 maximum 511
  limit-resource u4route-mem minimum 248 maximum 248
  limit-resource u6route-mem minimum 96 maximum 96
  limit-resource m4route-mem minimum 58 maximum 58
  limit-resource m6route-mem minimum 8 maximum 8

nv overlay evpn
feature ospf
feature bgp
feature interface-vlan
feature vn-segment-vlan-based
feature nv overlay

no password strength-check
username admin password 5 $5$GUhcLLiY$mabDN7CV7XNrjLzvnbbsKRdj1qUyd4/bdxFXVW0qYbD  role network-admin
ip domain-lookup
copp profile strict
snmp-server user admin network-admin auth md5 0x3d4238519bbcc2c14275a61810781543 priv 0x3d4238519bbcc2c14275a61810781543 localizedkey
rmon event 1 description FATAL(1) owner PMON@FATAL
rmon event 2 description CRITICAL(2) owner PMON@CRITICAL
rmon event 3 description ERROR(3) owner PMON@ERROR
rmon event 4 description WARNING(4) owner PMON@WARNING
rmon event 5 description INFORMATION(5) owner PMON@INFO

fabric forwarding anycast-gateway-mac 0000.0000.0010
vlan 1,20,30,50,999
vlan 20
  vn-segment 10020
vlan 30
  vn-segment 10030
vlan 999
  vn-segment 10999

ip prefix-list prLocalNet seq 5 permit 192.168.0.0/16 le 24 
route-map rmRedistStatic permit 100
  match ip address prefix-list prLocalNet 
vrf context management
vrf context vrfABC
  vni 10999
  ip route 192.168.130.0/24 Null0
  ip route 192.168.150.0/24 192.168.50.7
  rd auto
  address-family ipv4 unicast
    route-target both auto
    route-target both auto evpn
hardware access-list tcam region racl 0
hardware access-list tcam region arp-ether 256 double-wide


interface Vlan1

interface Vlan20
  no shutdown
  vrf member vrfABC
  no ip redirects
  ip address 192.168.20.1/24
  fabric forwarding mode anycast-gateway

interface Vlan30
  no shutdown
  vrf member vrfABC
  ip address 192.168.30.1/24
  fabric forwarding mode anycast-gateway

interface Vlan50
  no shutdown
  vrf member vrfABC
  ip address 192.168.50.1/24

interface Vlan999
  no shutdown
  vrf member vrfABC
  ip forward

interface nve1
  no shutdown
  host-reachability protocol bgp
  source-interface loopback3
  member vni 10020
    ingress-replication protocol bgp
  member vni 10030
    ingress-replication protocol bgp
  member vni 10999 associate-vrf

interface Ethernet1/1
  switchport mode trunk
  switchport trunk allowed vlan 10,20,30,50

interface Ethernet1/2

interface Ethernet1/3

interface Ethernet1/4
  no switchport
  ip address 10.77.1.10/30
  ip ospf network point-to-point
  no ip ospf passive-interface
  ip router ospf 1 area 0.0.0.0
  no shutdown

interface Ethernet1/5
  no switchport
  ip address 10.77.2.10/30
  ip ospf network point-to-point
  no ip ospf passive-interface
  ip router ospf 1 area 0.0.0.0
  no shutdown

interface Ethernet1/6

interface Ethernet1/7

interface Ethernet1/8

interface Ethernet1/9

interface Ethernet1/10

interface Ethernet1/11

interface Ethernet1/12

interface Ethernet1/13

interface Ethernet1/14

interface Ethernet1/15

interface Ethernet1/16

interface Ethernet1/17

interface Ethernet1/18

interface Ethernet1/19

interface Ethernet1/20

interface Ethernet1/21

interface Ethernet1/22

interface Ethernet1/23

interface Ethernet1/24

interface Ethernet1/25

interface Ethernet1/26

interface Ethernet1/27

interface Ethernet1/28

interface Ethernet1/29

interface Ethernet1/30

interface Ethernet1/31

interface Ethernet1/32

interface Ethernet1/33

interface Ethernet1/34

interface Ethernet1/35

interface Ethernet1/36

interface Ethernet1/37

interface Ethernet1/38

interface Ethernet1/39

interface Ethernet1/40

interface Ethernet1/41

interface Ethernet1/42

interface Ethernet1/43

interface Ethernet1/44

interface Ethernet1/45

interface Ethernet1/46

interface Ethernet1/47

interface Ethernet1/48

interface Ethernet1/49

interface Ethernet1/50

interface Ethernet1/51

interface Ethernet1/52

interface Ethernet1/53

interface Ethernet1/54

interface Ethernet1/55

interface Ethernet1/56

interface Ethernet1/57

interface Ethernet1/58

interface Ethernet1/59

interface Ethernet1/60

interface Ethernet1/61

interface Ethernet1/62

interface Ethernet1/63

interface Ethernet1/64

interface Ethernet1/65

interface Ethernet1/66

interface Ethernet1/67

interface Ethernet1/68

interface Ethernet1/69

interface Ethernet1/70

interface Ethernet1/71

interface Ethernet1/72

interface Ethernet1/73

interface Ethernet1/74

interface Ethernet1/75

interface Ethernet1/76

interface Ethernet1/77

interface Ethernet1/78

interface Ethernet1/79

interface Ethernet1/80

interface Ethernet1/81

interface Ethernet1/82

interface Ethernet1/83

interface Ethernet1/84

interface Ethernet1/85

interface Ethernet1/86

interface Ethernet1/87

interface Ethernet1/88

interface Ethernet1/89

interface Ethernet1/90

interface Ethernet1/91

interface Ethernet1/92

interface Ethernet1/93

interface Ethernet1/94

interface Ethernet1/95

interface Ethernet1/96

interface Ethernet1/97

interface Ethernet1/98

interface Ethernet1/99

interface Ethernet1/100

interface Ethernet1/101

interface Ethernet1/102

interface Ethernet1/103

interface Ethernet1/104

interface Ethernet1/105

interface Ethernet1/106

interface Ethernet1/107

interface Ethernet1/108

interface Ethernet1/109

interface Ethernet1/110

interface Ethernet1/111

interface Ethernet1/112

interface Ethernet1/113

interface Ethernet1/114

interface Ethernet1/115

interface Ethernet1/116

interface Ethernet1/117

interface Ethernet1/118

interface Ethernet1/119

interface Ethernet1/120

interface Ethernet1/121

interface Ethernet1/122

interface Ethernet1/123

interface Ethernet1/124

interface Ethernet1/125

interface Ethernet1/126

interface Ethernet1/127

interface Ethernet1/128

interface mgmt0
  vrf member management

interface loopback1
  ip address 10.77.255.7/32
  ip router ospf 1 area 0.0.0.0

interface loopback3
  ip address 10.77.253.7/32
  ip router ospf 1 area 0.0.0.0

interface loopback90
  vrf member vrfABC
  ip address 192.168.90.1/24
line console
line vty
boot nxos bootflash:/nxos.9.2.2.bin 
router ospf 1
  router-id 10.77.255.7
  passive-interface default
router bgp 65000
  template peer SPINE
    remote-as 65000
    update-source loopback1
    address-family l2vpn evpn
      send-community
      send-community extended
  neighbor 10.77.255.1
    inherit peer SPINE
  neighbor 10.77.255.2
    inherit peer SPINE
  vrf vrfABC
    address-family ipv4 unicast
      network 192.168.90.0/24
      redistribute static route-map rmRedistStatic
evpn
  vni 10020 l2
    rd auto
    route-target import auto
    route-target export auto
  vni 10030 l2
    rd auto
    route-target import auto
    route-target export auto

</code></pre>
