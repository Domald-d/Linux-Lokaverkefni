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

  so the first thing we do is sudo apt install ntp and then we are going to edit the ntp.conf like so on our server

  ![Screenshot from 2023-10-13 09-02-36](https://github.com/Domald-d/Linux-Lokaverkefni/assets/78101890/e2092433-e1a3-466d-9853-1df59e463513)

  and then we are going to sudo apt install ntpdate and ntp on our clients and we are going to sync the time with our server but first we must edit the conf file to be like so

  ![Screenshot 2023-10-13 090810](https://github.com/Domald-d/Linux-Lokaverkefni/assets/78101890/72902104-e972-47de-897f-25125b941316)

  and then we can check if our client has synced sucesfully with our server

  ![Screenshot 2023-10-10 144826](https://github.com/Domald-d/Linux-Lokaverkefni/assets/78101890/add824f8-3034-452d-97fb-d48da73a84a6)

9. Install and configure syslog server on server1, server1 should get logs from both the clients 
for proactive management and monitoring.

  on our server we are going to edit the rsyslog.conf like this

  ![Screenshot from 2023-10-13 09-22-36](https://github.com/Domald-d/Linux-Lokaverkefni/assets/78101890/7e0c87d6-3e2f-42c9-a76a-32cf1bb6d332)

  next what we want to do is edit the rsyslog.conf on our clients like so

  ![Screenshot 2023-10-13 094505](https://github.com/Domald-d/Linux-Lokaverkefni/assets/78101890/b2206038-f8c9-4d05-8e28-16938081588d)
  
  and then we can see in our syslog files this

  ![Screenshot from 2023-10-05 14-32-52](https://github.com/Domald-d/Linux-Lokaverkefni/assets/78101890/43d576ee-ab84-49bd-8a23-301210e0cf54)

  ![Screenshot from 2023-10-06 09-37-51](https://github.com/Domald-d/Linux-Lokaverkefni/assets/78101890/2b2626e6-ee1b-435d-9494-fa73c725a6a2)

10. Install and configure Postfix on server1, so users can send and receive emails using Round 
Cube open-source software

so first what we do is sudo apt install postfix and set it as server1.ddp.is then what we will do is edit the conf file like so

![Screenshot from 2023-10-13 11-19-10](https://github.com/Domald-d/Linux-Lokaverkefni/assets/78101890/78e40067-9c97-4b57-80f0-95e876edc912)

  next step will be to install dovecot and it's dependencies and now we will need to edit a lot of config files like so

  ![Screenshot from 2023-10-13 11-20-51](https://github.com/Domald-d/Linux-Lokaverkefni/assets/78101890/05f74f0b-e0e1-4a7a-8ce1-c17b7ce4058c)
  
  ![Screenshot from 2023-10-13 11-22-51](https://github.com/Domald-d/Linux-Lokaverkefni/assets/78101890/95eec9b5-cfa8-4bf6-bce1-5e9fd92bb454)

  now we need to install roundcube and what we need to do is specify what database we want to use in the roundcube config like so

  ![Screenshot from 2023-10-13 11-24-36](https://github.com/Domald-d/Linux-Lokaverkefni/assets/78101890/6c866597-27af-40a3-bbc7-c7f28b285b0f)

  and then we can access roundcube and use our designated domain like user@server1.ddp.is or user@ddp.is

  ![Screenshot from 2023-10-07 23-31-34](https://github.com/Domald-d/Linux-Lokaverkefni/assets/78101890/bf661057-2fdc-4428-a59f-ee045aef449d)

11. Install and configure shared printers for each group, only users that belong to the group 
should print only, accept IT and Management groups should print and manage the printers.

 cups should already be installed on our machine but to be sure we can do sudo apt install cups

  next what we need to do is go into the cups interface what we can do there is add printers as an admin like so

  ![Screenshot from 2023-10-13 11-31-38](https://github.com/Domald-d/Linux-Lokaverkefni/assets/78101890/43775ae7-8c84-478f-855f-e7a9b5db4f05)

  and then after that we should have something like this

  ![Screenshot from 2023-10-11 13-24-05](https://github.com/Domald-d/Linux-Lokaverkefni/assets/78101890/2a06f0b8-09f5-4a62-9217-30133ad2e0ca)

  ![Screenshot from 2023-10-08 01-06-54](https://github.com/Domald-d/Linux-Lokaverkefni/assets/78101890/bf7630e4-28f6-4cc6-9819-27800676919e)

  now what we need to do is add the permissions to the printers like this so each group/department can print from their printers

  ![Screenshot from 2023-10-08 01-07-31](https://github.com/Domald-d/Linux-Lokaverkefni/assets/78101890/0be9af6b-9976-43b0-af1d-afdddbba33f7)

12. For security reasons, install and configure SSH on the server and clients, SSH login should 
use RSA keys instead of the password authentication

  on the server we will need to sudo apt install openssh-server and do the following commands after creating a RSA key

  ![Screenshot from 2023-10-09 17-18-21](https://github.com/Domald-d/Linux-Lokaverkefni/assets/78101890/a541f08c-6bee-4c90-b8ab-6f76033d920f)

  now on the clients we will install the openssh-client and create a RSA key and do the following
  ssh-copy-id server1@192.168.100.10
  and now we can ssh to the server without the need for password
  
![Screenshot 2023-10-10 144720](https://github.com/Domald-d/Linux-Lokaverkefni/assets/78101890/38515134-3a4f-48fb-8982-cfe4857cb2b5)

    
13. All unused ports should be closed, use NMAP for testing.

  so on the server we will sudo apt install nmap 
  and to check on our ports we will nmap 192.168.100.10 i.e. our server ip

  ![Screenshot from 2023-10-09 17-36-40](https://github.com/Domald-d/Linux-Lokaverkefni/assets/78101890/a6d1b4c4-52bc-4c7c-ab61-5f48ba3cd2d1)

  as we can see our server has only the ports we require open and keeps our server secure
