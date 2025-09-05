# Configuring-VoIP-on-Packet-Tracer
Setting up a simple network of IP phones using CLI on Cisco Packet Tracer
1. First we need to set up our Necessary topology, and identify them.
Make sure you go onto the GUI of each phone and plug them with the power supply.
<img width="1096" height="768" alt="Screenshot 2025-09-05 095054" src="https://github.com/user-attachments/assets/3fa98eb8-1224-4232-adbe-78543a035ec8" />

2. Next we will need to configure the Switchports on the 2960 switch. This is to access the default vlan.
- conf t
- Switch(config)#int range fa0/1-7
- Switch(config-if-range)#switchport voice vlan 1

3. Configure trunk to the interface connecting to the router.
- Switch(config)#int fa0/1
- Switch(config-if)#switchport mode trunk
- do wr
- do show start (shows all the vlans)

4. Configure IP on the router interface and create a DHCP pool for voice.
- all IP phones need to get an IP address and so we have to configure on the router
Router(config)#int fa0/0
Router(config-if)#no shut
Router(config-if)#ip add 10.10.10.1 255.255.0.0

- Router(config)#service dhcp
- Router(config)#ip dhcp pool VOICE
- Router(dhcp-config)#network 10.10.10.0 255.255.255.0
- Router(dhcp-config)#default-router 10.10.10.1
- option 150 ?
- option 150 ip 10.10.10.1
- exit
- do wr (to save configuration)
<img width="655" height="482" alt="Screenshot 2025-09-05 110254" src="https://github.com/user-attachments/assets/54ce5faf-51ae-4c28-a1ac-538b4380288c" />

Now we'll go back to fast interface to make ip address class c
- Router(config)#int fa0/0
- Router(config-if)#ip add 10.10.10.1 255.255.255.0
- do wr


5. Configure telephony service
Router(config)#telephony-service 
Router(config-telephony)#max-ephones 6
Router(config-telephony)#max-dn 6
Router(config-telephony)#ip source-address 10.10.10.1 port 2000
Router(config-telephony)#auto assign 1 to 6
- exit and do wr
<img width="650" height="507" alt="Screenshot 2025-09-05 111854" src="https://github.com/user-attachments/assets/580b4f03-1deb-4c77-acbc-1dc87989bfd9" />
<img width="1100" height="628" alt="Screenshot 2025-09-05 124748" src="https://github.com/user-attachments/assets/b20de407-49b2-455b-b865-d14b1afb8977" />


6. Allocate dial numbers/extensions to the phones
- Router(config)#ephone-dn 1
- Router(config-ephone-dn)#number 101
- Do this for phones 1-6 and directory numbers 101-106
<img width="689" height="617" alt="Screenshot 2025-09-05 124809" src="https://github.com/user-attachments/assets/da455f9c-1206-4c11-9842-47b9c34cfd15" />

When all is said and done, you should be able to pick up one phone and dial the other phone no problem.

<img width="708" height="719" alt="Screenshot 2025-09-05 124947" src="https://github.com/user-attachments/assets/9f320846-9fbb-4f47-bb3c-9ec86dd01ae3" />
<img width="701" height="708" alt="Screenshot 2025-09-05 125004" src="https://github.com/user-attachments/assets/bb411b25-652d-4b70-8f3f-125f18c35e90" />


