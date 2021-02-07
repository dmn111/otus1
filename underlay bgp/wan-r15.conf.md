<pre><code>
!
! Last configuration change at 19:16:32 EET Sat Feb 6 2021
!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname R15-WAN
!
boot-start-marker
boot-end-marker
!
!
no logging console
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
!
!
!
no ip domain lookup
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
interface Loopback1
 ip address 10.77.255.15 255.255.255.255
!
interface Loopback2
 ip address 10.66.255.15 255.255.255.255
!
interface Ethernet0/0
 ip address 10.77.1.14 255.255.255.252
!
interface Ethernet0/1
 ip address 10.77.2.14 255.255.255.252
!
interface Ethernet0/2
 no ip address
 shutdown
!
interface Ethernet0/3
 no ip address
 shutdown
!
router bgp 65300
 bgp log-neighbor-changes
 neighbor 10.77.1.13 remote-as 65000
 neighbor 10.77.1.13 timers 3 9
 neighbor 10.77.2.13 remote-as 65000
 neighbor 10.77.2.13 timers 3 9
 !
 address-family ipv4
  network 10.77.255.15 mask 255.255.255.255
  neighbor 10.77.1.13 activate
  neighbor 10.77.1.13 default-originate
  neighbor 10.77.1.13 advertisement-interval 0
  neighbor 10.77.2.13 activate
  neighbor 10.77.2.13 default-originate
  neighbor 10.77.2.13 advertisement-interval 0
  maximum-paths 5
 exit-address-family
!
ip forward-protocol nd
!
!
no ip http server
no ip http secure-server
ip route 0.0.0.0 0.0.0.0 Null0
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


</code></pre>

