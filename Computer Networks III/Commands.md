
## Module 10

| Command       | Explanation          |
| ------------- | -------------------- |
| `no cpd run`    | disable CDP globally |
| `cdp run`       | enable CDP globally  |
| `no cdp enable` | disable CDP on port  |
| `cdp enable`    | enable CDP on port                     |

## Module 11

| Command | Explanation |
| ------- | ----------- |
|         |             |

Secure unused ports
```
interface range fa0/8-24
shutdown

#activate again
no shutdown
```

Enable Port Security
```
S1(config)# interface f0/1
S1(config-if)# switchport port-security
Command rejected: FastEthernet0/1 is a dynamic port.
S1(config-if)# switchport mode access
S1(config-if)# switchport port-security
S1(config-if)# end
```

Display current port security settings
```
S1# show port-security interface f0/1
Port Security              : Enabled
Port Status                : Secure-down
Violation Mode             : Shutdown
Aging Time                 : 0 mins
Aging Type                 : Absolute
SecureStatic Address Aging : Disabled
Maximum MAC Addresses      : 1
Total MAC Addresses        : 0
Configured MAC Addresses   : 0
Sticky MAC Addresses       : 0
Last Source Address:Vlan   : 0000.0000.0000:0
Security Violation Count   : 0
```

Other port security specifics
```
S1(config-if)# switchport port-security 
  aging        Port-security aging commands
  mac-address  Secure mac address
  maximum      Max secure addresses
  violation    Security violation mode  
S1(config-if)# switchport port-security
```

Set maximum number of MAC addresses allowed on a p