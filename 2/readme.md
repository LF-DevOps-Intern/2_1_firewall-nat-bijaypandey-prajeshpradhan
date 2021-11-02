# Question 2

> 2.	Create one vm with 2 network interfaces one should behave as WAN and another as LAN. Create another vm attaching the previously created LAN interface to it. 
a.	Implement NAT in the first vm, so that the second vm can access the internet.
Note: Configure the first vm as a router, so make the LAN interfaces in the first vm as gateway to the LAN network. And in the second vm configure the gateway to the ip of the first vm LAN ip.


First VM needs to have two network adapters set as:

![First VM Net Adapter](screenshots/Screenshot%202021-11-03%20034302.png)
![First VM Net Adapter](screenshots/Screenshot%202021-11-03%20034331.png)

Second VM needs only one adapter, i.e the Host -only adapter (Name must be same)

---

#### To configure the First VM

Firstly we set IP forward as 1

```
echo 1 > /proc/sys/net/ipv4/ip_forward
```

Run these commands to set the IPTABLE Routing rules for NATing

```
modprobe iptable_nat
iptables -F 
iptables -t nat -A POSTROUTING -o enp0s3 -j MASQUERADE
iptables -A FORWARD -i enp0s8 -o enp0s3 -j ACCEPT
```

---

#### To configure the Second VM

We need to set the default gateway as the private IP of the first VM (Server), which can be done with the command

```
route add default gw 192.168.56.1
```

In this way Linux system can be used as NAT server and so can be used to access private IP

---

##### Client Routes (VM2):

![Client Route](screenshots/Screenshot%202021-11-03%20042409.png)

##### Server Routes (VM1):

![Server Route](screenshots/Screenshot%202021-11-03%20042510.png)

##### Pinging to google.com:

![Ping to Google](screenshots/Screenshot%202021-11-03%20042539.png)
![Trace to Google](screenshots/Screenshot%202021-11-03%20042829.png)


