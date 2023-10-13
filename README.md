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

first thing we need to do is sudo apt install isc-dhcp-server next step will be to edit the dhcp config file
we can do so with vim /etc/dhcp/dhcp.conf and create a subnet like so

![Screenshot from 2023-10-04 12-49-14](https://github.com/Domald-d/Linux-Lokaverkefni/assets/78101890/14986954-8557-40ae-b794-fbad4742a8e2)

next what we do is specify what interface we want this dhcp to be allocated to and we put it as ens37 like so

![Screenshot from 2023-10-12 14-16-14](https://github.com/Domald-d/Linux-Lokaverkefni/assets/78101890/349bf214-4d5e-4e8f-b550-02932df8d0bc)

now we restart the dhcp service and check it's status like so

![dhcp systemctl status](https://github.com/Domald-d/Linux-Lokaverkefni/assets/78101890/03b4f234-4e38-41e1-8199-f934da840caf)

4. Install and configure DNS server on server1, so Hostnames are resolved to IP Addresses.

  to install and configure dns we will first have to install dns i used sudo apt install -y bind9 bind9-utils
  then we have to change a few files like this one /etc/bind/named.conf.local
  our file should look like so

  ![dns config](https://github.com/Domald-d/Linux-Lokaverkefni/assets/78101890/2121cf58-6746-4893-9196-43f7023e5488)

  then we create the forward zones like so  /etc/bind/server1.ddp.is.db and same for the reverse lookup our config files should look like so

  ![dns config server1](https://github.com/Domald-d/Linux-Lokaverkefni/assets/78101890/6c4b9738-0c2f-4ab6-948f-ba6fdadd5636)

  and our reverse zone file

  ![reverse dns server1](https://github.com/Domald-d/Linux-Lokaverkefni/assets/78101890/19470889-dd66-42e3-b436-51231f252f13)

  and then we can double check it is all running good like so

  ![Screenshot from 2023-10-12 14-33-01](https://github.com/Domald-d/Linux-Lokaverkefni/assets/78101890/b931e827-b905-4fb6-8375-cb23091a9899)

5. Create the users accounts using a script, see the Users file

   i created a very simple and basic bash script for this here's my code for it

    ![Screenshot from 2023-10-12 14-37-02](https://github.com/Domald-d/Linux-Lokaverkefni/assets/78101890/d3004c8b-c76c-4ecd-ba66-84a48fc68b96)

  and as a result our users and groups are created like so

  ![Screenshot from 2023-10-07 23-30-30](https://github.com/Domald-d/Linux-Lokaverkefni/assets/78101890/65ccc1bc-fe7e-4f1e-baf5-d6d34577baad)

  and our groups

  ![Screenshot from 2023-10-07 23-29-33](https://github.com/Domald-d/Linux-Lokaverkefni/assets/78101890/3ff113c7-70f2-4084-afbe-5dc3e30eb9a6)

  6. Install and configure MySQL on server1 and create Human Resource database.

     first we install the mysql server like so sudo apt install mysql-server
     then we need to get into the mysql like so mysql -u root -p
     when we are in the mysql terminal we can do show databases; to see our databases like so and create our new database
     like this create database Human_Resources; and we will be able to see the database like so

     ![Screenshot from 2023-10-07 23-30-56](https://github.com/Domald-d/Linux-Lokaverkefni/assets/78101890/48f070cf-de56-4e13-990a-322a2aa0706a)

     now we create the tables like so  after we do the command use Human_Resources;
     CREATE TABLE Employees (kennitala etc) and we can see our tables like so

     ![Screenshot from 2023-10-12 14-50-30](https://github.com/Domald-d/Linux-Lokaverkefni/assets/78101890/f5336cce-3ef8-4077-bf6e-15d5d8b948ba)

     to see in our tables we can issue the command Describe Table name like so
     
     ![Screenshot from 2023-10-12 14-52-08](https://github.com/Domald-d/Linux-Lokaverkefni/assets/78101890/17d92de9-2d64-4a7a-9257-b115a0655841)


7. Due to data loss the company policy requires taking backups weekly, as system engineer 
you are required to schedule backups of home directories to run weekly at midnight each 
Friday

     so first the we are going to do is edit the crontab file like this
   
   ![Screenshot from 2023-10-13 08-41-05](https://github.com/Domald-d/Linux-Lokaverkefni/assets/78101890/7826d36b-9380-49f6-a6d1-29ccc9d3ea7d)

next what we want to do is make our bash back up script that backups every user directory on the server here is how that would look like

![Screenshot from 2023-10-13 08-42-48](https://github.com/Domald-d/Linux-Lokaverkefni/assets/78101890/df1e1d54-d857-41f6-b1d5-3ce966f341a1)

8. Install and configure NTP on the server and clients, server1 must be master server to 
synchronize the time of the clients.

  so the first thing we do is sudo apt install ntp and then we are going to edit the ntp.conf like so

  ![Screenshot from 2023-10-13 09-02-36](https://github.com/Domald-d/Linux-Lokaverkefni/assets/78101890/e2092433-e1a3-466d-9853-1df59e463513)

  and then we are going to change the conf file on the client as well like so

  
