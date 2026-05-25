# Ubuntu 22.04 Password Reset in Rescue Mode

## Boot into GRUB Recovery

1. Reboot the server.
2. Press and hold `Shift` or `Esc` to open the GRUB menu.
3. Select the Ubuntu boot entry.
4. Press `e` to edit boot parameters.
5. Find the line starting with:

```bash
linux
```

6. Replace:

```bash
ro quiet splash
```

with:

```bash
rw init=/bin/bash
```

7. Press:

```bash
Ctrl + X
```

or

```bash
F10
```

to boot.

---

## Reset Password

Once the shell opens, run:

```bash
passwd root
```

Enter the new password twice.

If resetting another user:

```bash
passwd username
```

---

## Save Changes

```bash
sync
```

---

## Remount Filesystem

```bash
mount -o remount,ro /
```

---

## Reboot Server

```bash
/sbin/reboot -f
```

If reboot does not work:

```bash
/sbin/init 6
```

---

## Login After Reboot

Login using:

```bash
root
```

now you are inside resque mode

# Configure Private IP in Ubuntu 22.04 (VPC Network)

This guide explains how to configure a private IP address manually using `ip` commands and permanently using Netplan in Ubuntu 22.04.

## Network Details

| Parameter | Value |
|-----------|-------|
| Interface | eth0 |
| Private IP | 10.1.2.2 |
| Netmask | 255.255.255.0 |
| CIDR | /24 |
| Gateway | 10.1.2.1 |
| DNS | 8.8.8.8, 1.1.1.1 |

---

# Temporary Configuration Using `ip` Command

## 1. Check Network Interface

```bash
ip a

2. Add Private IP
 
```bash
 ip addr add 10.1.2.2/24 dev eth0
```
3. Bring Interface Up
 
```bash
 ip link set eth0 up
```
4. Add Default Gateway
 
```bash
 ip route add default via 10.1.2.1
```
5. Configure DNS

```bash
 echo "nameserver 8.8.8.8" > /etc/resolv.conf
 echo "nameserver 1.1.1.1" >> /etc/resolv.conf
```

6. Verify Configuration
 
```bash
 ip a
 ip route
 ping 8.8.8.8
```

Permanent Configuration Using Netplan
1. Open Netplan Configuration File
 
```bash
 vim /etc/netplan/00-installer-config.yaml
```
2. Add Configuration
 
```bash
 network:
   version: 2
   ethernets:
     eth0:
       dhcp4: false
       addresses:
         - 10.1.2.2/24
       routes:
         - to: default
           via: 10.1.2.1
       nameservers:
         addresses:
           - 8.8.8.8
           - 1.1.1.1

```

3. Apply Configuration
 
```bash
 netplan generate
 netplan apply
```

4. Verify
 
```bash
 ip a
 ip route
 ping 8.8.8.8
```
Troubleshooting
Remove Existing IP

 
```bash
 ip addr del 10.1.2.2/24 dev eth0
```
Delete Default Route
 
```bash
 ip route del default
```

Restart Networking
 
```bash
 systemctl restart systemd-networkd
```

Check DNS Resolution
 
```bash
 ping google.com
```

Notes
 In VPC environments, configure the private IP inside the OS.
 Public IPs are generally mapped automatically through NAT.
 Ensure the gateway belongs to the same subnet.
 Use netplan try before netplan apply on remote systems to avoid SSH lockout.
