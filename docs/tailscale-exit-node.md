# Configuring Tailscale exit node (Debian 13)

To ensure the Tailscale Docker container works correctly as a gateway router on the local network, the following configurations were applied to the host:

1. **IP Forwarding on at Kernel of Debian:**
   ```bash
   sudo sysctl -w net.ipv4.ip_forward=1
   sudo sysctl -w net.ipv6.conf.all.forwarding=1
   ```

2. **NAT network masquerading via Iptables for the Tailscale subnet:**
   ```bash
   sudo iptables -t nat -A POSTROUTING -s 100.64.0.0/10 -j MASQUERADE
   sudo iptables -I FORWARD -i tailscale0 -j ACCEPT
   sudo iptables -I FORWARD -o tailscale0 -j ACCEPT
   ```