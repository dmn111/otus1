<pre><code>
!
! Last configuration change at 12:55:32 EET Mon Mar 29 2021
!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname b-fw-wan11
!
boot-start-marker
boot-end-marker
!
!
!
no aaa new-model
clock timezone EET 2 0
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180
!
!
!
!
!
!
!
!


!
ip vrf vrfLAN
!
!
!
!
ip cef
no ipv6 cef
!
multilink bundle-name authenticated
!
!
!
!
!
!
!
!
!
redundancy
!
!
! 
!
!
!
!
!
!
!
!
!
!
!
!
interface Ethernet0/0
 no ip address
!
interface Ethernet0/0.11
!
interface Ethernet0/0.50
 encapsulation dot1Q 50
 ip address 10.90.50.1 255.255.255.0
 no ip redirects
 no ip proxy-arp
 ip ospf 1 area 0
!
interface Ethernet0/0.111
 encapsulation dot1Q 111
 ip address 10.90.255.1 255.255.255.0
 ip ospf 1 area 0
!
interface Ethernet0/1
 no ip address
 shutdown
!
interface Ethernet0/2
 no ip address
 shutdown
!
interface Ethernet0/3
 no ip address
 shutdown
!
router ospf 1
 router-id 10.90.255.1
 passive-interface default
 no passive-interface Ethernet0/0.111
!
ip forward-protocol nd
!
!
no ip http server
no ip http secure-server
ip route 10.0.0.0 255.0.0.0 10.90.255.2
!
!
!
!
control-plane
!
!
!
!
!
!
!
!
line con 0
 logging synchronous
line aux 0
line vty 0 4
 login
 transport input none
!
!
end

b-fw-wan11#exit





























































b-fw-wan11 con0 is now available





Press RETURN to get started.









