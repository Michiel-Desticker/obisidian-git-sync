
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

### Secure unused ports

```
interface range fa0/8-24
shutdown

#activate again
no shutdown
```

### Enable Port Security

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

### Limit and Learn MAC Addresses

Set maximum number of MAC addresses allowed on a port
```
Switch(config-if)# switchport port-security maximum _value_
```

Show maximum number (depends on switch)
```
S1(config)# interface f0/1
S1(config-if)# switchport port-security maximum ? 
  <1-8192>  Maximum addresses
S1(config-if)# switchport port-security maximum
```

#### Learn MAC addresses in 3 different ways

1. Manually Configured
```
Switch(config-if)# switchport port-security mac-address _mac-address_
```

2. Dynamically Learned
```
switchport port-security
```
3. Dynamically Learned - Sticky
```
Switch(config-if)# switchport port-security mac-address sticky
```

Veryify configuration
```
show port-security interface
show port-security address
```

Example 
```
*Mar  1 00:12:38.179: %LINK-3-UPDOWN: Interface FastEthernet0/1, changed state to up
*Mar  1 00:12:39.194: %LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/1, changed state to up
S1#**conf t**
Enter configuration commands, one per line.  End with CNTL/Z.
S1(config)#
S1(config)# **interface fa0/1**
S1(config-if)# **switchport mode access**
S1(config-if)# **switchport port-security**
S1(config-if)# **switchport port-security maximum 2**
S1(config-if)# **switchport port-security mac-address aaaa.bbbb.1234**
S1(config-if)# **switchport port-security mac-address sticky** 
S1(config-if)# **end**
S1# **show port-security interface fa0/1**
Port Security              : Enabled
Port Status                : Secure-up
Violation Mode             : Shutdown
Aging Time                 : 0 mins
Aging Type                 : Absolute
SecureStatic Address Aging : Disabled
Maximum MAC Addresses      : 2
Total MAC Addresses        : 2
Configured MAC Addresses   : 1 #static MAC address
Sticky MAC Addresses       : 1 #sticky MAC address
Last Source Address:Vlan   : a41f.7272.676a:1
Security Violation Count   : 0
S1# **show port-security address**
               Secure Mac Address Table
-----------------------------------------------------------------------------
Vlan    Mac Address       Type                          Ports   Remaining Age
                                                                   (mins)    
----    -----------       ----                          -----   -------------
1    a41f.7272.676a    SecureSticky                  Fa0/1        -
1    aaaa.bbbb.1234    SecureConfigured              Fa0/1        -
-----------------------------------------------------------------------------
Total Addresses in System (excluding one mac per port)     : 1
Max  Addresses limit in System (excluding one mac per port) : 8192
```

### Port Security Aging

Enable or disable static aging for the secure port, or to set the aging time or type
```
Switch(config-if)# **switchport port-security aging** { **static** | **time** _time_ | **type** {**absolute** | **inactivity**}}
```