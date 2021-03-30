<pre><code>
!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname a-esx-01
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
ip vrf vrfA
!
ip vrf vrfB
!
ip vrf vrfC
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
interface Ethernet0/0.10
 encapsulation dot1Q 10
 ip vrf forwarding vrfA
 ip address 10.90.10.71 255.255.255.0
!
interface Ethernet0/0.20
 encapsulation dot1Q 20
 ip vrf forwarding vrfB
 ip address 10.90.20.71 255.255.255.0
!
interface Ethernet0/0.50
 encapsulation dot1Q 50
 ip vrf forwarding vrfC
 ip address 10.90.50.71 255.255.255.0
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
ip forward-protocol nd
!
!
no ip http server
no ip http secure-server
ip route vrf vrfA 0.0.0.0 0.0.0.0 10.90.10.1
ip route vrf vrfB 0.0.0.0 0.0.0.0 10.90.20.1
ip route vrf vrfC 0.0.0.0 0.0.0.0 10.90.50.1
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