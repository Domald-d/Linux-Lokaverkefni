# Linux-Lokaverkefni
Lokaverkefni fyrir Linux

1. Install and configure the server1, client1 and client2 with hostnames and domain as ddp.is

we begin by going on the server1 and editing the config files  /etc/hosts and /etc/hostname  there we change the files like this

![Screenshot 2023-10-12 120026](https://github.com/Domald-d/Linux-Lokaverkefni/assets/78101890/662a2d6c-4c1d-42d2-b26c-e0c2506b862e)


2. configure server1 with static IP Address, from the IP Address block 192.168.100.0/24. The 
server must be configured with the 10th usable IP Address

first thing we do is configure the 01-network-manager yaml file like so

![Screenshot from 2023-10-12 14-07-13](https://github.com/Domald-d/Linux-Lokaverkefni/assets/78101890/a8705deb-2e16-4bbf-b6ce-4b5673761c4b)

next step is to do the command netplan apply and reboot when we are back on our machine we can do  ip add to see if our server has been configured correctly like so

![Screenshot from 2023-10-12 14-09-05](https://github.com/Domald-d/Linux-Lokaverkefni/assets/78101890/869fa22e-2195-403b-a705-bdb73d452e3c)

3. Install and configure DHCP on server1, so both clients get an IP Addresses, Gateway, DNS 
IP address and domain name automatically via DHCP

