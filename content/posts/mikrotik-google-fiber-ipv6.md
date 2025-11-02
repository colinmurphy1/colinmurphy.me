---
title: "How to configure IPv6 with Google Fiber on MikroTik RouterOS"
date: 2025-11-01
draft: false
description: "How to get IPv6 to work with GFiber on MikroTik RouterOS"
tags:
  - networking
  - mikrotik
---

Google Fiber uses DHCPv6 to receive IPv6 prefixes instead of using router advertisements. In order to get IPv6 to work properly on a MikroTik router, some manual configuration is required.

## Configure the DHCPv6 client

First, we must configure a DHCPv6 client on the router, which is done using the below command. This command adds a new DHCPv6 client that operates off our WAN interface (ether1), and requests an IPv6 for the router's WAN interface, as well as a prefix to use to assign addresses to devices on the network.

```
/ipv6 dhcp-client
add add-default-route=yes interface=ether1 pool-name=gfiber request=address,prefix use-peer-dns=yes
```

If you wish to use your own DNS server, set `use-peer-dns` to no, and configure your DNS server manually at `/ip/dns`.

You should now be assigned a /56 prefix as well as an address for your WAN interface. To confirm, run the `/ipv6/dhcp-client/print` command.
```
[admin@router] > /ipv6/dhcp-client/print 
Columns: INTERFACE, STATUS, REQUEST, PREFIX, ADDRESS
# INTERFACE  STATUS  REQUEST  PREFIX                              ADDRESS                         
0 ether1     bound   address  2605:xxxx:xxxx:xxxx::/56, 23h54m8s  2605:xxxx:xxxx:xxxx::1, 23h54m8s
                     prefix   
```

## Assign IPv6 addresses to interfaces

Now that an IPv6 prefix has been allocated, we can now assign subnets to individual interfaces on the router. This can be done with the below command:

```
/ipv6 address
add eui-64=yes from-pool=gfiber interface=bridge
```

This command assigns a subnet from the `gfiber` pool created by the DHCPv6 client and assigns it to the bridge interface. EUI-64 must be enabled to generate a unique IPv6 address based on the device's MAC address. If you do not do this, you may run into issues with Duplicate Address Detection (DAD) with your interface's IPv6 address, resulting in loss of IPv6 connectivity on the interface.

## Testing IPv6

Now that this is configured, your clients should now begin receiving IPv6 addresses through router advertisements. You can test out IPv6 functionality by trying to ping an IPv6 service, e.g. google.com.

```bash
# ping -6 -c 4 google.com
PING google.com (2607:f8b0:4009:81a::200e) 56 data bytes
64 bytes from ord38s31-in-x0e.1e100.net (2607:f8b0:4009:81a::200e): icmp_seq=1 ttl=118 time=7.54 ms
64 bytes from ord38s31-in-x0e.1e100.net (2607:f8b0:4009:81a::200e): icmp_seq=2 ttl=118 time=7.37 ms
64 bytes from ord38s31-in-x0e.1e100.net (2607:f8b0:4009:81a::200e): icmp_seq=3 ttl=118 time=7.44 ms
64 bytes from ord38s31-in-x0e.1e100.net (2607:f8b0:4009:81a::200e): icmp_seq=4 ttl=118 time=7.37 ms

--- google.com ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3005ms
rtt min/avg/max/mdev = 7.365/7.429/7.543/0.072 ms

```