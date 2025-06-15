# RHEL9 Administrator Series
## Fixed IP Address

| ***Date*** | 15-6-2025 |
| --- | --- |
| ***Author*** | Thanakorn P. |


show connection name
```
nmcli connection show
```
Set IP Address, Gateway, DNS, Method
```
# Set the IPv4 address and prefix (e.g., /24 for 255.255.255.0)
sudo nmcli connection modify "ens160" ipv4.addresses 192.168.1.100/24

# Set the default gateway
sudo nmcli connection modify "ens160" ipv4.gateway 192.168.1.1

# Set the DNS servers (space-separated)
sudo nmcli connection modify "ens160" ipv4.dns "8.8.8.8 8.8.4.4"

# Change the method to manual to disable DHCP
sudo nmcli connection modify "ens160" ipv4.method manual
```
Apply Change
```
sudo nmcli connection down "ens160"
sudo nmcli connection up "ens160"
```