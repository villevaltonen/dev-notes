## Random notes

### Storage
- Hard link vs symbolic link -> inode can have multiple hard links
- `tmpfs` is in-memory
- User permissions are shown as user-group-others (e.g. rw-rw-r) - "ugo" for short
    - Permissions
        - read (4)
        - write (2)
        - execute (1)
- `/etc/fstab` for pesistent mounts
    - Do not make persistent mounts on `/mnt`, it's meant for temporary mounts

### Networking

#### IPv4
- Internet -> router -> local network -> nodes (local machines)
    - Nodes have ip addresses
    - Often router uses the last or the first ip in the range of the network, e.g. 192.168.1.254
- DNS used for resolving hostnames to ip addresses

#### IPv6
- Addresses are 128 bits in hexadecimal (0-9 and a-f)
- Leading zeros can be omitted
- [address]:port
- Default subnet mask is /64
- Node bit of the address may contain the MAC address of the device
- ::1/127 localhost
- :: unspecified
- ::/0 default route
- 2000::/3 global unicast address
- fd00::/8 unique local address (private addresses)
- fe80::/64 link-local address
- ff00:/8 multicast
- fe80:: address is used by default
- Options:
    - Through static addressing
    - DHCPv6

#### Runtime network configs
- On servers ip address config is set statically via `nmcli` or `nmt`
- On workstations ip address config is typhically handed out by a a DHCP server
- `ip` command is used to set runtime configs
- `ifconfig` is obsolete nowadays, `ip` has replaced it

#### Routing
- Static routing to access another network, e.g. from 10.0.0.0/24 to 10.168.0.0/24
- Can be added to define a route to a network, which is not behind the default gateway
- E.g. `ip route show` -> `ip route add 192.2.0.0/24 via 10.0.0.10`

#### Tools
```bash
ip addr add dev ens160 10.0.0.10/24
ip link set ens160 up
ip link st ens160 down
ip route show
ip route add default via 10.0.0.1
ip -s link
ethtool
networkctl
```

#### Device names
- Old Linux style: eth0, wlan0
- Modern Linux style: named according to their physical location in the computer with the help of the driver
- system-udevd generates device names
- Examples
    - em123 (ethernet motherboard port number)
    - eno123 (ethernet onboard)
    - p<port>p<slot> (PCI, PCI port)
    - if drive does not provide enough information, eth0 is used

#### DNS related
- `hostnamectl` to set hostname
- `/etc/hosts` for lookups
- DNS name servers are configured in `/etc/resolve.conf`

#### DNS related tools
```bash
ping
ss # open local ports
dig # for dns testing
nmap # scan open ports, use only on your own hosts
netstat # replaced by ss
nslookup # replaced by dig
```
### Services
- `systemctl` to manage services
```bash
# systemctl options
isolate
daemon-reload
isolate
list-dependencies
get-default
set-default
```

### Kernel module related commands
```bash
lsmod
modprobe
modprobe -r
modinfo
depmod
```

### Kernel tuning
- `/proc` is used for kernel tuning and optimisation
- E.g. `echo 60 > /proc/sys/vm/swappiness`, but this is not persistent
- `sysctl` is used for persitent configurations
- `systemctl list-unit-files -t socket`
- Control groups are controllers for CPU, memory and blkio (IO)
- `cfconfig`
- `cgred`
- Cgroup settings are written to `/sys/fs/cgroups`
- `ulimit -a`
- `iotop`
- `vmstat`

### Firewalls
- Kernel -> netfilter -> input | forward | output
- `iptables` and `nftables`
- `firewall-cmd`
    - Zone: a collection of network cards
    - Interface: an individual network card assigned to a zone
    - Service: an XML-based config that specifies ports to be opened and modules that should be used
    - Forward ports: used to send traffic from a specific port to another, which may be used even on another machine
    - Masquerading: provides NAT on a router
- `ufw`

### NAT (network address translation)
- Private network should be protected from public internet with NAT

#### How it works
0. A packet from 10.0.0.10 to 1.1.1.1
0. Packet goes to a router
0. The router issues a packet with a source 192.168.1.1 to 1.1.1.1, hence the private ip is masked (192.168.1.1 is the ip of the router)
0. MAC address are included in the packet, so the router knows to which ip to forward the returning message based on the MAC address mapping

### Security related
- SELinux for Red Hat
- AppArmor for Ubuntu etc.