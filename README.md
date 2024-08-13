# rtbh
Route Triggered Black Hole

## Topology

## Router Configuration

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
