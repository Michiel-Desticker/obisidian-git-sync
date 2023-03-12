
| VLAN | IP address    | Subnet mask     |  Subinterface   | 
| ---- | ------------- | --------------- | --- |
| 20   | 192.168.8.193 | 255.255.255.240 |   G0/0/1.20  |
| 30   | 192.168.8.129 | 255.255.255.192 |   G0/0/1.30  |
| 40   | 192.168.8.1   | 255.255.255.128 |   G0/0/1.40  |
| 50   | 192.168.8.209 | 255.255.255.248 |   G0/0/1.50  |


## Router config

```
Router(config)#hostname R1

R1(config)#int g0/0/1.20

R1(config-subif)#encapsulation dot1Q 20

R1(config-subif)#ip address 192.168.8.193 255.255.255.240

R1(config-subif)#int g0/0/1.30

R1(config-subif)#encapsulation dot1Q 30

R1(config-subif)#ip address 192.168.8.129 255.255.255.192

R1(config-subif)#int g0/0/1.40

R1(config-subif)#ip address 192.168.8.1 255.255.255.128

R1(config)#int g0/0/1.40

R1(config-subif)#encapsulation dot1Q 40

R1(config-subif)#ip address 192.168.8.1 255.255.255.128

R1(config-subif)#int g0/0/1.50

R1(config-subif)#encapsulation dot1Q 50

R1(config-subif)#ip address 192.168.8.209 255.255.255.248

R1(config)#int g0/0/1

R1(config-if)#no shutdown
```

```
R1(config)#ip dhcp pool Cast

R1(dhcp-config)#network 192.168.8.0 255.255.255.128

R1(dhcp-config)#default-router 192.168.8.1

R1(dhcp-config)#dns-server 192.168.8.195

R1(dhcp-config)#exit

R1(config)#ip dhcp pool Crew

R1(dhcp-config)#network 192.168.8.128 255.255.255.192

R1(dhcp-config)#default-router 192.168.8.129

R1(dhcp-config)#dns-server 192.168.8.195
```

## Switch

```
Switch(config)#hostname S1

S1(config)#vlan 20

S1(config-vlan)#name Servers

S1(config-vlan)#vlan 30

S1(config-vlan)#name Crew

S1(config-vlan)#vlan 40

S1(config-vlan)#name Cast

S1(config-vlan)#vlan 50

S1(config-vlan)#name Router

S1(config-vlan)#exit

S1(config)#ip default-gateway 192.168.8.209

```

```
S1(config)#int f0/7

S1(config-if)#switchport mode access

S1(config-if)#switchport access vlan 40

S1(config-if)#int f0/8

S1(config-if)#switchport mode access

S1(config-if)#switchport access vlan 40

S1(config-if)#int f0/1 - 15

S1(config-if)#int f0/1

S1(config-if)#switchport mode access

S1(config-if)#switchport access vlan 20

S1(config-if)#int f0/2

S1(config-if)#switchport mode access

S1(config-if)#switchport access vlan 20

S1(config-if)#int f0/4

S1(config-if)#switchport mode access

S1(config-if)#switchport access vlan 20

S1(config-if)#int f0/5

S1(config-if)#switchport mode access

S1(config-if)#switchport access vlan 20

S1(config-if)#int f0/6

S1(config-if)#switchport mode access

S1(config-if)#switchport access vlan 20

S1(config-if)#int f0/9

S1(config-if)#switchport mode access

S1(config-if)#switchport access vlan 30

S1(config-if)#int f0/10

S1(config-if)#switchport mode access

S1(config-if)#switchport access vlan 30

S1(config-if)#int g0/1

S1(config-if)#switchport mode access

S1(config-if)#switchport access vlan 50

S1(config-if)#int g0/1

S1(config-if)#switchport mode trunk
```

