# sisyphus
Configuration notes for IWUs computer cluster sisyphus
The following 7-part series was used as a guide: https://www.youtube.com/watch?v=JcwAaJNCGJk&list=PLWk-zl-RHnbc8PS55ZiJHFtPTQobegyH5&index=3


## Hardware

1. 24 Port POE+ Netgear (managed) switch
2. 2x Dell R625s
   * Dual Socket, AMD EPYC Milan w/1024 GB DDR4 @ 3200 Mhz
3. Schneider electric 2.7 kW UPS
4. Adata head node (not sure of specs aside from 8GB RAM, 256 TB NVME, and 2TB SSD)

## OS

Ubuntu 24.04 

configuration of network

1. Headnode IWU-net facing network interface enables internet connectivity and SSH access. Other NIC configured for LAN.
   * enp2s0
      * inet: 10.112.32.47
   * enp0s32f6
      * IPV4: 10.42.0.1
      * Subnet Mask: 255.255.255.0
      * Gateway: NONE
      * DNS: 8.8.8.8,4.2.2.2
    * enabled ip forwarding via "echo "net.ipv4.ip_forward=1" | sudo tee -a /etc/sysctl.conf" & "sudo sysctl -p"
    * Set up NAT (Masquerading) on Headnode via: "sudo iptables -t nat -A POSTROUTING -o enp1s0 -j MASQUERADE" & verified w/ "sudo iptables -t nat -L -n -v"
    * Then saved the rules w/ "sudo apt install iptables-persistent" &
"sudo netfilter-persistent save"


2. Worknodes
   * worknode1
      * IPV4: 10.42.0.2
      * Subnet Mask: 255.255.255.0
      * Gateway: 10.42.0.1
      * DNS: 8.8.8.8, 4.2.2.2
    * worknode2
      * IPV4: 10.42.0.3
      * Subnet Mask: 255.255.255.0
      * Gateway: 10.42.0.1
      * DNS: 8.8.8.8, 4.2.2.2
     
#openssh
installed openssh and restart service


