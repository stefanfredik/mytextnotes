# All About Cisco Router Configuraion

## IP Address

```bash
sh ip interface
```

### Setup DHCP Client

```bash
configure terminal
interface [interface name]
ip address dhcp
no shutdown
end
wr
```

### Show IP Dhcp Client

```bash
sh ip interface brief
```

### Add IP Address

```bash
configure terminal
interface [interface name]
ip address [address] [subnet mask]
end
wr
```

### Remove IP Address

```bash
configure terminal
interface [interface name]
no ip address
end
```

### Add DNS

```bash
ip domain lookup #Enables DNS-based host name-to-address translation. This command is enabled by default.
configure terminal
ip name-server [dns ip ] #Ex : ip name-server 8.8.8.8
end
```

## Interface

### Disable/ Enable Interface

Disable

```bash
configure terminal
interface [interface name]
shutdown
end
```

Enable

```bash
configure terminal
interface [interface name]
no shutdown
end
```
