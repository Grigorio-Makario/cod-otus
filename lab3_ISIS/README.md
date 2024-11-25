### Underlay. IS-IS

### Цели
- Настроите ISIS в Underlay сети, для IP связанности между всеми сетевыми устройствами;
- Зафиксируете в документации - план работы, адресное пространство, схему сети, конфигурацию устройств;
- Убедитесь в наличии IP связанности между устройствами в ISIS домене;



### Общая топология

![img_3.png](lab_3_clos.png)



#### Настройка Spine-1

```
router isis Arista
net 49.0000.0100.6000.0001.00
authentication mode md5
authentication key secret
address-family ipv4 unicast
log-adjacency-changes

interface Loopback0
isis enable Arista

interface range Eth1, Eth2, Eth3
isis enable Arista
isis network point-to-point
logging event link-status
load-interval 30
bfd interval 100 min_rx 100 multiplier 3
no bfd echo

```

#### Настройка Spine-2

```
router isis Arista
net 49.0000.0100.6000.0002.00
authentication mode md5
authentication key secret
address-family ipv4 unicast
log-adjacency-changes

interface Loopback0
isis enable Arista

interface range Eth1, Eth2, Eth3
isis enable Arista
isis network point-to-point
logging event link-status
load-interval 30
bfd interval 100 min_rx 100 multiplier 3
no bfd echo
```

#### Настройка Leaf-1

```
router isis Arista
net 49.0000.0100.6001.0001.00
authentication mode md5
authentication key secret
log-adjacency-changes
address-family ipv4 unicast

interface Loopback0
isis enable Arista

interface range Eth1, Eth2
isis enable Arista
isis network point-to-point
logging event link-status
load-interval 30
bfd interval 100 min_rx 100 multiplier 3
no bfd echo

```

#### Настройка Leaf-2

```
router isis Arista
net 49.0000.0100.6001.0002.00
authentication mode md5
authentication key secret
log-adjacency-changes
address-family ipv4 unicast

interface Loopback0
isis enable Arista

interface range Eth1, Eth2
isis enable Arista
isis network point-to-point
logging event link-status
load-interval 30
bfd interval 100 min_rx 100 multiplier 3
no bfd echo

```

#### Настройка Leaf-3

```
router isis Arista
net 49.0000.0100.6001.0003.00
authentication mode md5
authentication key secret
log-adjacency-changes
address-family ipv4 unicast

interface Loopback0
isis enable Arista

interface range Eth1, Eth2
isis enable Arista
isis network point-to-point
logging event link-status
load-interval 30
bfd interval 100 min_rx 100 multiplier 3
no bfd echo

```


#### Проверка конфигурации

```
spine-1#sh isis neighbors

Instance  VRF      System Id        Type Interface          SNPA              State Hold time   Circuit Id
Arista    default  leaf-1           L1L2 Ethernet1          P2P               UP    28          0E
Arista    default  leaf-2           L1L2 Ethernet2          P2P               UP    30          0E
Arista    default  leaf-3           L1L2 Ethernet3          P2P               UP    23          0E

spine-1#sh ip route

VRF: default
Codes: C - connected, S - static, K - kernel,
       O - OSPF, IA - OSPF inter area, E1 - OSPF external type 1,
       E2 - OSPF external type 2, N1 - OSPF NSSA external type 1,
       N2 - OSPF NSSA external type2, B - Other BGP Routes,
       B I - iBGP, B E - eBGP, R - RIP, I L1 - IS-IS level 1,
       I L2 - IS-IS level 2, O3 - OSPFv3, A B - BGP Aggregate,
       A O - OSPF Summary, NG - Nexthop Group Static Route,
       V - VXLAN Control Service, M - Martian,
       DH - DHCP client installed default route,
       DP - Dynamic Policy Route, L - VRF Leaked,
       G  - gRIBI, RC - Route Cache Route

Gateway of last resort is not set

 C        10.60.0.1/32 is directly connected, Loopback0
 I L1     10.60.0.2/32 [115/30] via 10.60.2.1, Ethernet1
                                via 10.60.2.9, Ethernet2
                                via 10.60.2.17, Ethernet3
 I L1     10.60.1.1/32 [115/20] via 10.60.2.1, Ethernet1
 I L1     10.60.1.2/32 [115/20] via 10.60.2.9, Ethernet2
 I L1     10.60.1.3/32 [115/20] via 10.60.2.17, Ethernet3
 C        10.60.2.0/30 is directly connected, Ethernet1
 I L1     10.60.2.4/30 [115/20] via 10.60.2.1, Ethernet1
 C        10.60.2.8/30 is directly connected, Ethernet2
 I L1     10.60.2.12/30 [115/20] via 10.60.2.9, Ethernet2
 C        10.60.2.16/30 is directly connected, Ethernet3
 I L1     10.60.2.20/30 [115/20] via 10.60.2.17, Ethernet3

spine-2#sh isis neighbors

Instance  VRF      System Id        Type Interface          SNPA              State Hold time   Circuit Id
Arista    default  leaf-1           L1L2 Ethernet1          P2P               UP    25          0F
Arista    default  leaf-2           L1L2 Ethernet2          P2P               UP    24          0F
Arista    default  leaf-3           L1L2 Ethernet3          P2P               UP    23          0F

spine-2#sh ip route

VRF: default
Codes: C - connected, S - static, K - kernel,
       O - OSPF, IA - OSPF inter area, E1 - OSPF external type 1,
       E2 - OSPF external type 2, N1 - OSPF NSSA external type 1,
       N2 - OSPF NSSA external type2, B - Other BGP Routes,
       B I - iBGP, B E - eBGP, R - RIP, I L1 - IS-IS level 1,
       I L2 - IS-IS level 2, O3 - OSPFv3, A B - BGP Aggregate,
       A O - OSPF Summary, NG - Nexthop Group Static Route,
       V - VXLAN Control Service, M - Martian,
       DH - DHCP client installed default route,
       DP - Dynamic Policy Route, L - VRF Leaked,
       G  - gRIBI, RC - Route Cache Route

Gateway of last resort is not set

 I L1     10.60.0.1/32 [115/30] via 10.60.2.5, Ethernet1
                                via 10.60.2.13, Ethernet2
                                via 10.60.2.21, Ethernet3
 C        10.60.0.2/32 is directly connected, Loopback0
 I L1     10.60.1.1/32 [115/20] via 10.60.2.5, Ethernet1
 I L1     10.60.1.2/32 [115/20] via 10.60.2.13, Ethernet2
 I L1     10.60.1.3/32 [115/20] via 10.60.2.21, Ethernet3
 I L1     10.60.2.0/30 [115/20] via 10.60.2.5, Ethernet1
 C        10.60.2.4/30 is directly connected, Ethernet1
 I L1     10.60.2.8/30 [115/20] via 10.60.2.13, Ethernet2
 C        10.60.2.12/30 is directly connected, Ethernet2
 I L1     10.60.2.16/30 [115/20] via 10.60.2.21, Ethernet3
 C        10.60.2.20/30 is directly connected, Ethernet3
```

