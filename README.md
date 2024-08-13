# rtbh
Route Triggered Black Hole

## Topology

## ZebOS routing configuration

```
!
no service password-encryption
!
log syslog
!
router bgp 5555
 bgp router-id 10.1.20.4
 bgp graceful-restart restart-time 120
 redistribute kernel
 neighbor 10.1.20.9 remote-as 5555
!
line con 0
 login
line vty 0 39
 login
!
end
```
## VyOS routing configuration

```
 policy {
     route-map blackhole {
         rule 100 {
             action permit
             match {
                 ip {
                     nexthop {
                         address 10.1.20.200
                     }
                 }
             }
             set {
                 ip-next-hop 192.168.0.1
                 local-preference 5000
             }
         }
         rule 900 {
             action permit
         }
     }
 }
```

```
 protocols {
     bgp {
         address-family {
             ipv4-unicast {
                 network 10.1.10.0/24 {
                 }
             }
         }
         neighbor 10.1.20.4 {
             address-family {
                 ipv4-unicast {
                     route-map {
                         import blackhole
                     }
                 }
             }
             remote-as 5555
         }
         system-as 5555
     }
     static {
         route 192.168.0.0/24 {
             blackhole {
                 distance 254
             }
         }
     }
 }
```
