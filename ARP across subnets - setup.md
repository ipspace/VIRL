Linux server setup
==================

VIRL does not save Linux server configuration, so you have to configure
IP addresses, subnet masks and summary routes manually after the server
boot is complete.

Passwords
---------
User 'cisco' in the Ubuntu image used for Linux servers in VIRL has password
'cisco' and sudo password 'cisco'.

Configuring S1
--------------
Execute these commands on S1:

    sudo rm /etc/resolv.conf
    sudo ifconfig eth1 10.0.0.5 netmask 255.255.255.0
    sudo ip route add 10.0.0.0/8 via 10.0.0.1

The first command disables DNS lookups (and reverse DNS lookups). Without it
you'll wait for ages for any printout from **arp** or **tcpdump**.

The second command changes the IP address and subnet mask on the *eth1* interface. The third command adds a static summary route for 10/8 pointing to the Cisco IOS router.

Configuring S2
--------------
Execute similar commands on S2:

    sudo rm /etc/resolv.conf
    sudo ifconfig eth1 10.0.1.5 netmask 255.255.255.0
    sudo ip route add 10.0.0.0/8 via 10.0.1.1

Configuring Cisco IOS
---------------------
Instead of using my VIRL topology file you might want to start with an empty
router configuration. The only configuration you need is the interface configuration:

    interface GigabitEthernet0/1
     ip address 10.0.1.1 255.255.0.0 secondary
     ip address 10.0.0.1 255.255.0.0
     arp timeout 20
