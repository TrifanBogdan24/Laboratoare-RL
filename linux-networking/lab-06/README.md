# Laborator 06


## Task 01


```sh
student@host:~$ sudo su -
root@host:~#  ip address add 192.168.0.1/24 dev veth-red
root@host:~# ip address show dev veth-red
5: veth-red@if4: <BROADCAST,MULTICAST> mtu 1450 qdisc noqueue state DOWN group default qlen 1000
    link/ether 0e:ae:52:5c:56:36 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 192.168.0.1/24 scope global veth-red
       valid_lft forever preferred_lft forever

root@host:~# go red
student@red:~$
root@red:~$ ip address add 192.168.0.2/24 dev red-eth0
root@red:~$  ip address show dev red-eth0
4: red-eth0@if5: <BROADCAST,MULTICAST> mtu 1450 qdisc noqueue state DOWN group default qlen 1000
    link/ether de:21:e9:1b:b1:48 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 192.168.0.2/24 scope global red-eth0
       valid_lft forever preferred_lft forever
```


```sh
root@host:~# # Nu exista conectivitate intre cele doua statii
root@host:~# ping 192.168.0.2
PING 192.168.0.2 (192.168.0.2) 56(84) bytes of data.
^C
--- 192.168.0.2 ping statistics ---
2 packets transmitted, 0 received, 100% packet loss, time 1006ms
```



```sh
root@host:~# ip link show dev veth-red
5: veth-red@if4: <BROADCAST,MULTICAST> mtu 1450 qdisc noqueue state DOWN mode DEFAULT group default qlen 1000
    link/ether 0e:ae:52:5c:56:36 brd ff:ff:ff:ff:ff:ff link-netnsid 0
root@host:~# ip link set dev veth-red up
root@host:~# ip link show dev veth-red
5: veth-red@if4: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1450 qdisc noqueue state LOWERLAYERDOWN mode DEFAULT group default qlen 1000
    link/ether 0e:ae:52:5c:56:36 brd ff:ff:ff:ff:ff:ff link-netnsid 0
root@host:~# # Interfata este in 'LOWERLAYERDOWN', intrucat celalalt capat al legaturii nu este pornit (UP).

root@host:~# go red
student@red:~$ 
student@red:~$ sudo su -
root@red:~$  ip link set dev red-eth0 up
root@red:~$ ip link show dev red-eth0
4: red-eth0@if5: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1450 qdisc noqueue state UP mode DEFAULT group default qlen 1000
    link/ether de:21:e9:1b:b1:48 brd ff:ff:ff:ff:ff:ff link-netnsid 0
```


Testez din nou folosind `ping`:

```sh
root@red:~$  ping 192.168.0.1
PING 192.168.0.1 (192.168.0.1) 56(84) bytes of data.
64 bytes from 192.168.0.1: icmp_seq=1 ttl=64 time=0.046 ms
64 bytes from 192.168.0.1: icmp_seq=2 ttl=64 time=0.022 ms
^C
--- 192.168.0.1 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1010ms
rtt min/avg/max/mdev = 0.022/0.034/0.046/0.012 ms
```


```sh
root@host:~#  ping -c 1 192.168.0.2
PING 192.168.0.2 (192.168.0.2) 56(84) bytes of data.
64 bytes from 192.168.0.2: icmp_seq=1 ttl=64 time=0.033 ms

--- 192.168.0.2 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.033/0.033/0.033/0.000 ms
```


```sh
root@host:~# ip addr flush dev veth-red 
root@host:~# ip addr show dev veth-red 
5: veth-red@if4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1450 qdisc noqueue state UP group default qlen 1000
    link/ether 0e:ae:52:5c:56:36 brd ff:ff:ff:ff:ff:ff link-netnsid 0
```


Reven la configuratia initiala:

```sh
root@red:~$ ip addr show dev red-eth0 
4: red-eth0@if5: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1450 qdisc noqueue state UP group default qlen 1000
    link/ether de:21:e9:1b:b1:48 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 192.168.0.2/24 scope global red-eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::dc21:e9ff:fe1b:b148/64 scope link 
       valid_lft forever preferred_lft forever
root@red:~$ ip addr flush dev red-eth0 
root@red:~$ ip addr show dev red-eth0 
4: red-eth0@if5: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1450 qdisc noqueue state UP group default qlen 1000
    link/ether de:21:e9:1b:b1:48 brd ff:ff:ff:ff:ff:ff link-netnsid 0
```


## Task 02


Atribuire adrese IP:

- Spatiul 10.10.10.0/24
  - host/veth-red: 10.10.10.1/24
  - red/red-eth0: 10.10.10.2/24

- Spatiul 10.10.20.0/24
  -  host/veth-green: 10.10.20.1/24
  -  green/green-eth0: 10.10.20.2/24



### red(red-eth0) <-> host(veth-red)

Pentru spatiul 10.10.10.0/24:

```sh
root@host:~# ip link set dev veth-red up
root@host:~# ip address add 10.10.10.1/24 dev veth-red
root@host:~# go red
student@red:~$ sudo su -
root@red:~$ ip link set dev red-eth0 up
root@red:~$ ip addr add 10.10.10.2/24 dev red-eth0   
```

Testare conectivitate pentru spatiul 10.10.10.0/24:
```sh
root@host:~# ping -c 1 10.10.10.2  # IP red
root@red:~$  ping -c 1 10.10.10.1  # IP host/veth-red
```



### green(green-eth0) <-> host(veth-green)

Pentru spatiul 10.10.20.0/24:
```sh
root@host:~# ip link set dev veth-green up
root@host:~# ip address add 10.10.20.1/24  dev veth-green
root@host:~# go green
student@green:~$ sudo su -
root@green:~$ ip link set dev green-eth0 up
root@green:~$ ip addr add 10.10.20.2/24 dev green-eth0
```


Testare conectivitate pentru spatiul 10.10.20.0/24:
```sh
root@host:~# ping -c 1 10.10.20.2      # IP green
root@green:~$ ping -c 1  10.10.20.1    # IP host/veth-green
```



## Task 03


> IP host/veth-red = `10.10.10.1`


```sh
root@host:~# 
root@host:~# go red
student@red:~$ sudo su -
root@red:~$  ip route add default via 10.10.10.1
root@red:~$ ip route show
default via 10.10.10.1 dev red-eth0 
10.10.10.0/24 dev red-eth0 proto kernel scope link src 10.10.10.2 
root@red:~$ 
logout
student@red:~$ 
exit
root@host:~# go green
student@green:~$ sudo su -
root@green:~$ ip route add default via 10.10.20.1
root@green:~$ ip route show
default via 10.10.20.1 dev green-eth0 
10.10.20.0/24 dev green-eth0 proto kernel scope link src 10.10.20.2 
```

> `ip route del default` sterge **default gateway**-ul.


Testare conectivitate:

```sh
root@red:~$ ping -c 1 10.10.20.2     # IP green
root@green:~$ ping -c 1 10.10.10.2   # IP red
```

Nu exista conectivitate intre `red` si `green`
(pe `host` nu este inca activata rutarea).



```sh
root@host:~# # Activare rutare
root@host:~# sysctl -w net.ipv4.ip_forward=1
net.ipv4.ip_forward = 1
root@host:~# sysctl net.ipv4.ip_forward  # Comanda de verificare
net.ipv4.ip_forward = 1
```



## Task 04


Deschid un nou TAB de terminal cu `Ctrl+Shift+T`.

```sh
# Terminal 1
student@red:~$ ping 10.10.20.2
```

```sh
# Terminal 2
root@host:~# tcpdump -n -i veth-red
```



## Task 04


Spatiul 10.10.30.0/24:
- host/veth-blue: 10.10.30.1/24
- blue/blue-eth0: 10.10.30.2/24


```sh
root@host:~# ip link set dev veth-blue up
root@host:~# ip addr add 10.10.30.1/24 dev veth-blue
root@host:~# go blue
student@blue:~$ sudo su -
root@blue:~$ ip link set dev blue-eth0 up
root@blue:~$ ip addr add 10.10.30.2/24 dev blue-eth0
root@blue:~$ ip route add default via 10.10.30.1
```

> Comenzi de verificare:
> - `ip l`
> - `ip a`
> - `ip r`



Testare conectivitate:

```sh
root@blue:~$ ping -c 1 10.10.30.1   # IP host/veth-blue
root@blue:~$ ping -c 1 10.10.10.2   # IP red
root@blue:~$ ping -c 1 10.10.20.2   # IP green
root@green:~$ ping -c 1 10.10.10.2  # IP red
```



## Task 05

```sh
root@host:~# # Printare tabela ARP
root@host:~# ip neighbour show
10.9.0.100 dev eth0 lladdr fa:16:3e:cb:12:ab STALE
10.9.0.1 dev eth0 lladdr a2:05:9c:00:00:02 REACHABLE
10.10.30.2 dev veth-blue lladdr 7a:91:83:1c:df:5c STALE
10.10.20.2 dev veth-green lladdr 3a:b9:86:42:0e:8b STALE
10.10.10.2 dev veth-red lladdr de:21:e9:1b:b1:48 STALE
fe80::a9fe:a9fe dev eth0 lladdr fa:16:3e:cb:12:ab STALE
```

> `STALE`: intrari nesigure.


```sh
root@host:~# ping -c 1 10.10.10.2    # IP red
root@host:~# ping -c 1 10.10.20.2    # IP green
root@host:~# ping -c 1 10.10.30.2    # IP blue

root@host:~# ip neighbor show
10.9.0.100 dev eth0 lladdr fa:16:3e:cb:12:ab STALE
10.9.0.1 dev eth0 lladdr a2:05:9c:00:00:02 REACHABLE
10.10.30.2 dev veth-blue lladdr 7a:91:83:1c:df:5c DELAY
10.10.20.2 dev veth-green lladdr 3a:b9:86:42:0e:8b REACHABLE
10.10.10.2 dev veth-red lladdr de:21:e9:1b:b1:48 REACHABLE
fe80::a9fe:a9fe dev eth0 lladdr fa:16:3e:cb:12:ab STALE
```


```sh
root@red:~$ ip neighbour show
10.10.10.1 dev red-eth0 lladdr 0e:ae:52:5c:56:36 STALE

root@red:~$ ping -c 1 10.10.10.1  # IP host/veth-red

root@red:~$ ip neighbour show
10.10.10.1 dev red-eth0 lladdr 0e:ae:52:5c:56:36 REACHABLE
```





```sh
root@green:~$ ip neighbour show
10.10.20.1 dev green-eth0 lladdr 5e:bb:b0:db:be:90 STALE

root@green:~$ ping -c 1 10.10.20.1  # IP host/veth-green

root@green:~$ ip neighbour show
10.10.20.1 dev green-eth0 lladdr 5e:bb:b0:db:be:90 DELAY

root@green:~$ ping -c 1 10.10.10.2  # IP red
root@green:~$ ping -c 1 10.10.30.2  # IP blue

root@green:~$ ip neighbour show
10.10.20.1 dev green-eth0 lladdr 5e:bb:b0:db:be:90 REACHABLE
```


```sh
root@blue:~$ ip neighbour show
10.10.30.1 dev blue-eth0 lladdr 8a:86:9d:5d:86:23 STALE

root@blue:~$ ping -c 1 10.10.30.1   # IP host/veth-blue

root@blue:~$ ip neighbour show
10.10.30.1 dev blue-eth0 lladdr 8a:86:9d:5d:86:23 DELAY

root@blue:~$ ping -c 1 10.10.30.1   # IP host/veth-blue

root@blue:~$ ip neighbour show
10.10.30.1 dev blue-eth0 lladdr 8a:86:9d:5d:86:23 REACHABLE
```

> `ARP request`-urile pot lua ceva timp pana sa updateze tabela.


## Task 06


```sh
root@host:~#  start_lab ip ex6
```


Atribuirea corecta adreselor IP:
- host/veth-red: 7.7.7.1/24
- red/red-eth0: 7.7.7.2/24

> IP-ul pentru red/red-eth0 a fost configurat gresit (cu masca gresita de **/32**).


```sh
root@host:~# ip addr delete 7.7.7.1/32 dev veth-red
root@host:~# ip addr add 7.7.7.1/24 dev veth-red
```


```sh
root@host:~# ping 7.7.7.2   # IP red
PING 7.7.7.2 (7.7.7.2) 56(84) bytes of data.
64 bytes from 7.7.7.2: icmp_seq=1 ttl=64 time=0.044 ms
64 bytes from 7.7.7.2: icmp_seq=2 ttl=64 time=0.021 ms
^C
--- 7.7.7.2 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1018ms
rtt min/avg/max/mdev = 0.021/0.032/0.044/0.011 ms
```


## Task 07


```sh
root@host:~# start_lab ip ex7
```


Probleme:
- IP-uri pe legatura host-blue folosesc masca pe `/32` (practic masca asta nu are niciun host)
- Pe langa faptul ca host/veth-blue foloseste o masca cu totul gresita (`/32`),
  pare ca IP-ul (fara masca) este unul de retea (`15.15.15.0`)
- Legatura de pe blue nu este pornita (este **DOWN**)
- blue nu are **default gateway**


O masca `/30` poate avea 2 host-uri asignabile.


```sh
root@host:~# ip addr delete 15.15.15.0/32 dev veth-blue
root@host:~# ip addr add 15.15.15.1/30 dev veth-blue
```

```sh
root@blue:~$ ip addr delete 15.15.15.2/32 dev blue-eth0
root@blue:~$ ip addr add 15.15.15.2/30 dev blue-eth0

root@blue:~$ ip link set dev blue-eth0 up
root@blue:~$ ip route add default via 15.15.15.1
```


```sh
root@blue:~$ ping -c 1 15.15.15.1    # IP host/veth-blue
root@blue:~$ ping -c 1 192.168.1.2   # IP green
root@blue:~$ ping -c 1 192.168.2.2   # IP red
```

TODO: continue
