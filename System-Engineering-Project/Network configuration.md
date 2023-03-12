
| Subnet Name | Needed Size | Allocated Size | Address       | Mask | Dec Mask        | Assignable Range              | Broadcast     |
| ----------- | ----------- | -------------- | ------------- | ---- | --------------- | ----------------------------- | ------------- |
| VLAN 40     | 70          | 126            | 192.168.8.0   | /25  | 255.255.255.128 | 192.168.8.1 - 192.168.8.126   | 192.168.8.127 |
| VLAN 30     | 60          | 62             | 192.168.8.128 | /26  | 255.255.255.192 | 192.168.129 - 192.168.8.190   | 192.168.8.191 |
| VLAN 20     | 14          | 14             | 192.168.8.192 | /28  | 255.255.255.240 | 192.168.8.193 - 192.168.8.206 | 192.168.8.207 |
| VLAN 50     | 4           | 6              | 192.168.8.208 | /29  | 255.255.255.248 | 192.168.8.209 - 192.168.8.214 | 192.168.8.215 |

## Server Static IP Adresses (VLAN 20)

| Server            | Hostname   | IP address    | Default Gateway |
| ----------------- | ---------- | ------------- | --------------- |
| Domain Controller | AgentSmith | 192.168.8.195 | 192.168.8.193   |
| Mailserver        | Neo        | 192.168.8.196 | 192.168.8.193   |
| Webserver         | trinity    | 192.168.8.197 | 192.168.8.193   |
| Chatserver        | theoracle  | 192.168.8.198 | 192.168.8.193   |
| Minetest Server   | Dozer      | 192.168.8.199 | 192.168.8.193   |


## Subinterfaces Router 

| Subinterface | VLAN | IP address    | Subnet mask     |
| ------------ | ---- | ------------- | --------------- |
| G0/0/1.20    | 20   | 192.168.8.193 | 255.255.255.240 |
| G0/0/1.30    | 30   | 192.168.8.129 | 255.255.255.192 |
| G0/0/1.40    | 40   | 192.168.8.1   | 255.255.255.128 |
| G0/0/1.50    | 50   | 192.168.8.209 | 255.255.255.248 |



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

