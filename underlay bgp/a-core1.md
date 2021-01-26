<pre><code>
!
! Last configuration change at 21:49:44 EET Sat Jan 9 2021
!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname a-core1
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
 ip router isis 1
!
interface Ethernet0/0
 ip address 10.77.1.14 255.255.255.252
 ip router isis 1
 isis network point-to-point 
!
interface Ethernet0/1
 ip address 10.77.2.14 255.255.255.252
 ip router isis 1
 isis network point-to-point 
!
interface Ethernet0/2
 no ip address
 shutdown
!
interface Ethernet0/3
 no ip address
!
router isis 1
 net 49.0005.0000.0000.0001.00
 is-type level-2-only
 metric-style wide
!
ip forward-protocol nd
!
!
no ip http server
no ip http secure-server
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

