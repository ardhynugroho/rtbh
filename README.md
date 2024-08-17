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
## Note

```
ubuntu@attacker:~$ history
    1  hostname attacker
    2  sudo hostname attacker
    3  sudo vim /etc/hostname
    4  ab -c 100 -n 500000 http://app/
    5  curl app
    6  curl --interface 10.1.10.17 app
    7  ip add
    8  curl -v app
    9  curl --interface 10.1.10.17 -v app
   10  curl -v app
   11  curl app
   12  curl --interface 10.1.10.17 app
   13  curl 10.1.20.5
   14  curl 10.1.20.4
   15  ab
   16  apt update; apt install apache2-utils
   17  sudo apt update; sudo apt install apache2-utils
   18  sudo vim /etc/hosts
   19  curl app
   20  ip add
   21  sudo bash
   22  curl app
   23  ip ro
   24  sudo bash
   25  screen
   26  curl 10.1.20.55
   27  ab -c 100 -n 50000 http://10.1.20.55/
   28  curl 10.1.20.55
   29  history
ubuntu@attacker:~$
```
