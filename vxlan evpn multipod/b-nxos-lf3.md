<pre><code>

!Command: show running-config
!Running configuration last done at: Mon Mar 29 11:07:56 2021
!Time: Mon Mar 29 21:10:58 2021

version 9.2(2) Bios:version  
hostname b-nxos-lf3
vdc b-nxos-lf3 id 1
  limit-resource vlan minimum 16 maximum 4094
  limit-resource vrf minimum 2 maximum 4096
  limit-resource port-channel minimum 0 maximum 511
  limit-resource u4route-mem minimum 248 maximum 248
  limit-resource u6route-mem minimum 96 maximum 96
  limit-resource m4route-mem minimum 58 maximum 58
  limit-resource m6route-mem minimum 8 maximum 8

cfs eth distribute
nv overlay evpn
feature ospf
feature bgp
feature interface-vlan
feature vn-segment-vlan-based
feature lacp
feature vpc
feature nv overlay

no password strength-check
username admin password 5 $5$uoMl41d/$Gy9Ucnqr1Ia25UQOfhH4mYq4Md5BuAFADuMZC2OFHJ6  role network-admin
ip domain-lookup
copp profile strict
snmp-server user admin network-admin auth md5 0x35313b60d3525899118ddd7959df5c80 priv 0x35313b60d3525899118ddd7959df5c80 localizedkey
rmon event 1 description FATAL(1) owner PMON@FATAL
rmon event 2 description CRITICAL(2) owner PMON@CRITICAL
rmon event 3 description ERROR(3) owner PMON@ERROR
rmon event 4 description WARNING(4) owner PMON@WARNING
rmon event 5 description INFORMATION(5) owner PMON@INFO

vlan 1,20,50,111,999
vlan 20
  vn-segment 10020
vlan 50
  vn-segment 10050
vlan 999
  vn-segment 10999

ip prefix-list prDmzNet seq 5 permit 10.90.50.0/24 
route-map rmRedistOSPF permit 100
  match ip address prefix-list prDmzNet 
vrf context management
vrf context vrfDMZ
vrf context vrfLAN
  vni 10999
  rd auto
  address-family ipv4 unicast
    route-target import 9999:10999
    route-target import 9999:10999 evpn
    route-target export 9999:10999
    route-target export 9999:10999 evpn


interface Vlan1

interface Vlan20
  vrf member vrfLAN

interface Vlan50
  vrf member vrfDMZ
  ip address 10.90.50.254/24

interface Vlan111
  no shutdown
  vrf member vrfLAN
  ip address 10.90.255.2/24
  no ip ospf passive-interface
  ip router ospf 5 area 0.0.0.0

interface Vlan999
  no shutdown
  vrf member vrfLAN
  ip forward

interface nve1
  no shutdown
  host-reachability protocol bgp
  source-interface loopback1
  member vni 10020
    ingress-replication protocol bgp
  member vni 10050
    ingress-replication protocol bgp
  member vni 10999 associate-vrf

interface Ethernet1/1
  no switchport
  no ip redirects
  ip address 10.66.1.2/30
  no ipv6 redirects
  ip ospf network point-to-point
  no ip ospf passive-interface
  ip router ospf 1 area 0.0.0.0
  no shutdown

interface Ethernet1/2
  no switchport
  no ip redirects
  ip address 10.66.2.6/30
  no ipv6 redirects
  ip ospf network point-to-point
  no ip ospf passive-interface
  ip router ospf 1 area 0.0.0.0
  no shutdown

interface Ethernet1/3
  switchport mode trunk
  switchport trunk allowed vlan 50,111

interface Ethernet1/4

interface Ethernet1/5

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

interface loopback0
  ip address 10.66.255.3/32
  ip router ospf 1 area 0.0.0.0

interface loopback1
  ip address 10.66.253.3/32
  ip router ospf 1 area 0.0.0.0
line console
line vty
boot nxos bootflash:/nxos.9.2.2.bin 
router ospf 1
  router-id 10.66.255.3
  passive-interface default
router ospf 5
  router-id 10.90.255.2
  passive-interface default
  vrf vrfLAN
router bgp 65002
  template peer SPINE
    remote-as 65002
    update-source loopback0
    address-family l2vpn evpn
      send-community
      send-community extended
  neighbor 10.66.255.1
    inherit peer SPINE
  neighbor 10.66.255.2
    inherit peer SPINE
  vrf vrfLAN
    address-family ipv4 unicast
      redistribute ospf 5 route-map rmRedistOSPF
evpn
  vni 10020 l2
    rd auto
    route-target import 9999:10020
    route-target export 9999:10020
  vni 10050 l2
    rd auto
    route-target import 9999:10050
    route-target export 9999:10050


</code></pre>