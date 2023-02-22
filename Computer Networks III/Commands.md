
## Module 10

| Command       | Explanation          |
| ------------- | -------------------- |
| `no cpd run`    | disable CDP globally |
| `cdp run`       | enable CDP globally  |
| `no cdp enable` | disable CDP on port  |
| `cdp enable`    | enable CDP on port                     |

## Module 11

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
S1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
S1(config)#
S1(config)# interface fa0/1
S1(config-if)# switchport mode access
S1(config-if)# switchport port-security
S1(config-if)# switchport port-security maximum 2
S1(config-if)# switchport port-security mac-address aaaa.bbbb.1234
S1(config-if)# switchport port-security mac-address sticky 
S1(config-if)# end
S1# show port-security interface fa0/1
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
S1# show port-security address
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
Switch(config-if)# switchport port-security aging { static | time _time_ | type {absolute | inactivity}}
```

| Parameter         | Description                                                                                                                                                            |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| static            | Enable aging for statically configured secure addresses on this port.                                                                                                  |
| time _time_   | Specify the aging time for this port. The range is 0 to 1440 minutes. If the time is 0, aging is disabled for this port.                                               |
| type absolute | Set the absolute aging time. All the secure addresses on this port age out exactly after the time (in minutes) specified and are removed from the secure address list. |
| type inactivity                  |        Set the inactivity aging type. The secure addresses on this port age out only if there is no data traffic from the secure source address for the specified time period.                                                                                                                                                               |

Example configuring aging type to 10 minutes of inactivity
```
S1(config)# interface fa0/1
S1(config-if)# switchport port-security aging time 10 
S1(config-if)# switchport port-security aging type inactivity 
S1(config-if)# end
S1# show port-security interface fa0/1
Port Security              : Enabled
Port Status                : Secure-up
Violation Mode             : Shutdown
Aging Time                 : 10 mins
Aging Type                 : Inactivity
SecureStatic Address Aging : Disabled
Maximum MAC Addresses      : 2
Total MAC Addresses        : 2
Configured MAC Addresses   : 1
Sticky MAC Addresses       : 1
Last Source Address:Vlan   : a41f.7272.676a:1
Security Violation Count   : 0
```

### Port Security Violation Modes

```
Switch(config-if)# switchport port-security violation { protect | restrict | shutdown}
```


| Mode                   | Description                                                                                                                                                                                                                                                                                             |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| shutdown (default) | The port transitions to the error-disabled state immediately, turns off the port LED, and sends a syslog message. It increments the violation counter. When a secure port is in the error-disabled state, an administrator must re-enable it by entering the shutdown and no shutdown commands. |
| restrict           | The port drops packets with unknown source addresses until you remove a sufficient number of secure MAC addresses to drop below the maximum value or increase the maximum value. This mode causes the Security Violation counter to increment and generates a syslog message.                           |
| protect                       |      This is the least secure of the security violation modes. The port drops packets with unknown MAC source addresses until you remove a sufficient number of secure MAC addresses to drop below the maximum value or increase the maximum value. No syslog message is sent.                                                                                                                                                                                                                                                                                                   |

restrict nuttigste

| Violation mode | Discards Offending Traffic | Sends Syslog Message | Increase Violation Counter | Shuts Down Port |
| -------------- | -------------------------- | -------------------- | -------------------------- | --------------- |
| Protect        | Yes                        | No                   | No                         | No              |
| Restrict       | Yes                        | Yes                  | Yes                        | No              |
| Shutdown       | Yes                        | Yes                  | Yes                        | Yes                |

Example 
```
S1(config)# interface f0/1
S1(config-if)# switchport port-security violation restrict
S1(config-if)# end
S1#
S1# show port-security interface f0/1
Port Security              : Enabled
Port Status                : Secure-up
Violation Mode             : Restrict
Aging Time                 : 10 mins
Aging Type                 : Inactivity
SecureStatic Address Aging : Disabled
Maximum MAC Addresses      : 2
Total MAC Addresses        : 2
Configured MAC Addresses   : 1
Sticky MAC Addresses       : 1
Last Source Address:Vlan   : a41f.7272.676a:1
Security Violation Count   : 0
```

### Verify Port Security

Port Security for All Interfaces
```
S1# show port-security
Secure Port  MaxSecureAddr  CurrentAddr  SecurityViolation  Security Action
                (Count)       (Count)          (Count)
---------------------------------------------------------------------------
      Fa0/1              2            2                  0         Shutdown
---------------------------------------------------------------------------
Total Addresses in System (excluding one mac per port)     : 1
Max Addresses limit in System (excluding one mac per port) : 8192
```

Port Security for a Specific Interface
```
S1# show port-security interface fastethernet 0/1
Port Security          	    : Enabled
Port Status            	    : Secure-up
Violation Mode              : Shutdown
Aging Time                  : 10 mins
Aging Type                  : Inactivity
SecureStatic Address Aging  : Disabled
Maximum MAC Addresses       : 2
Total MAC Addresses         : 2
Configured MAC Addresses    : 1
Sticky MAC Addresses        : 1
Last Source Address:Vlan    : a41f.7273.018c:1
Security Violation Count    : 0
```

Verify Learned MAC Addresses
```
S1# show run interface fa0/1
Building configuration...

Current configuration : 365 bytes
!
interface FastEthernet0/1
 switchport mode access
 switchport port-security maximum 2
 switchport port-security mac-address sticky
 switchport port-security mac-address sticky a41f.7272.676a
 switchport port-security mac-address aaaa.bbbb.1234
 switchport port-security aging time 10
 switchport port-security aging type inactivity
 switchport port-security
end
```

Verify Secure MAC Addresses
```
S1# show port-security address
               Secure Mac Address Table
-----------------------------------------------------------------------------
Vlan    Mac Address       Type                          Ports   Remaining Age
                                                                   (mins)
----    -----------       ----                          -----   -------------
   1    a41f.7272.676a    SecureSticky                  Fa0/1        -
   1    aaaa.bbbb.1234    SecureConfigured              Fa0/1        -
-----------------------------------------------------------------------------
Total Addresses in System (excluding one mac per port)     : 1
Max Addresses limit in System (excluding one mac per port) : 8192
```

### Steps to Mitigate VLAN Hopping Attacks

Step 1: Disable DTP (auto trunking) negotiations on non-trunking ports by using the switchport mode access interface configuration command.

Step 2: Disable unused ports and put them in an unused VLAN.

Step 3: Manually enable the trunk link on a trunking port by using the switchport mode trunk command.

Step 4: Disable DTP (auto trunking) negotiations on trunking ports by using the switchport nonegotiate command.

Step 5: Set the native VLAN to a VLAN other than VLAN 1 by using the switchport trunk native vlan _vlan_number_ command.

Example
```
-   FastEthernet ports 0/1 through fa0/16 are active access ports
-   FastEthernet ports 0/17 through 0/20 are not currently in use
-   FastEthernet ports 0/21 through 0/24 are trunk ports.
```

```
S1(config)# interface range fa0/1 - 16
S1(config-if-range)# switchport mode access
S1(config-if-range)# exit
S1(config)# 
S1(config)# interface range fa0/17 - 20
S1(config-if-range)# switchport mode access
S1(config-if-range)# switchport access vlan 1000
S1(config-if-range)# shutdown
S1(config-if-range)# exit
S1(config)# 
S1(config)# interface range fa0/21 - 24
S1(config-if-range)# switchport mode trunk
S1(config-if-range)# switchport nonegotiate
S1(config-if-range)# switchport trunk native vlan 999
S1(config-if-range)# end
```

### Mitigate DHCP Attacks

#### Steps to Implement DHCP Snooping
```
Step 1. Enable DHCP snooping by using the ip dhcp snooping global configuration command.

Step 2. On trusted ports, use the ip dhcp snooping trust interface configuration command.

Step 3. Limit the number of DHCP discovery messages that can be received per second on untrusted ports by using the ip dhcp snooping limit rate interface configuration command.

Step 4. Enable DHCP snooping by VLAN, or by a range of VLANs, by using the ip dhcp snooping _vlan_ global configuration command.
```
Example
```
S1(config)# ip dhcp snooping
S1(config)# interface f0/1
S1(config-if)# ip dhcp snooping trust
S1(config-if)# exit
S1(config)# interface range f0/5 - 24
S1(config-if-range)# ip dhcp snooping limit rate 6
S1(config-if-range)# exit
S1(config)# ip dhcp snooping vlan 5,10,50-52
S1(config)# end
```

Verification
```
S1# show ip dhcp snooping
Switch DHCP snooping is enabled
DHCP snooping is configured on following VLANs:
5,10,50-52
DHCP snooping is operational on following VLANs:
none
DHCP snooping is configured on the following L3 Interfaces:
Insertion of option 82 is enabled
   circuit-id default format: vlan-mod-port
   remote-id: 0cd9.96d2.3f80 (MAC)
Option 82 on untrusted port is not allowed
Verification of hwaddr field is enabled
Verification of giaddr field is enabled
DHCP snooping trust/rate is configured on the following Interfaces:
Interface                  Trusted    Allow option    Rate limit (pps)
-----------------------    -------    ------------    ----------------   
FastEthernet0/1            yes        yes             unlimited
  Custom circuit-ids:
FastEthernet0/5            no         no              6         
  Custom circuit-ids:
FastEthernet0/6            no         no              6         
  Custom circuit-ids:
S1# show ip dhcp snooping binding
MacAddress         IpAddress       Lease(sec) Type          VLAN Interface
------------------ --------------- ---------- ------------- ---- --------------------
00:03:47:B5:9F:AD  192.168.10.11   193185     dhcp-snooping 5    FastEthernet0/5
```

### Mitigate ARP Attacks

DAI Configuration Example
```
S1(config)# ip dhcp snooping
S1(config)# ip dhcp snooping vlan 10
S1(config)# ip arp inspection vlan 10
S1(config)# interface fa0/24
S1(config-if)# ip dhcp snooping trust
S1(config-if)# ip arp inspection trust
```

