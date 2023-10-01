# Linux Networking

## Intall NetWork Tools

```bash
apt install net-tools
```

Check Network Interfaces

```bash
sudo ifconfig -a
```

## Ifconfig

Enable Network Intefaces

```bash
ifconfig [interfaces name] up

or

ifup [interfaces name]
```

Disable Network Intefaces

```bash
ifconfig [interfaces name] down

or

ifdown [interfaces name]
```

Refresh Dhcp Client

```bash
sudo dhcpclient
```

Renew Dhcp Client

```bash
sudo dhclient -r
```

```bash
sudo dhclient -r [interface name]

or

sudo dhclient [interface name]
```

Assign/ Add Ip address to interfaces

```bash
ifconfig eth0 172.18.26.124
ifconfig eth0 netmask 255.255.255.224
ifconfig eth0 broadcast 172.18.26.63
```

```bash
ifconfig eth0172.18.26.124 netmask 255.255.255.224 broadcast 172.18.26.63
```

Config MTU

```bash
ifconfig [interface-name] mtu [mtu-value]
```

Change Mac Address

```bash
ifconfig eth0 hw ether AA:BB:CC:DD:EE:FF
```

Start DHCP Client

```bash
ifconfig [inetrface] dhcp start
```

## Routing

Check Routing Table

```bash
netstat -rn
```

Change Priority Metric Gateway Routing Linux

```bash
if metric [interface] [metric number]
```

Add Routing Table

```bash
ip route add [ip network] via [ip gateway]

#Example ::
# ip route add 10.10.10.0/24 via 192.168.1.1
```

### Firewall

Check Firewall Server Status

```bash
ufw app list
```

Activate Firewall

```bash
ufw enable
```

Check Firewall Status

```bash
ufw status
```

Hostname Check List - hostname -I Firewall Allow Traffic

```bash
ufw allow [network service]
```

Example :

```bash
ufw allow 'Apache2'
```

Firewall Check Status

```bash
ufw status
```

### DNS Server

Add Custom DNS Server

```bash
nano /etc/resolv.conf

nameserver 8.8.8.8
```
