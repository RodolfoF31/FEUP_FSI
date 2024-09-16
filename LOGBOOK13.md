# SEED Labs – Sniffing and Spoofing

For setup, we installed Scapy for Python3, accomplished with a simple sudo pip3 install scapy command. We then crafted a Python script named mycode.py, which successfully imported Scapy's modules and displayed the default IP packet fields, confirming the proper functioning of Scapy within our environment.

## Task 1.1A: Sniffing Packets

For these task, we executed a Python program designed to sniff network packets using Scapy. The program defines a callback function, print_pkt(), which is invoked for each packet captured by the sniff function. This callback function outputs certain details about each packet.

```
#!/usr/bin/env python3
from scapy.all import *
def print_pkt(pkt):
pkt.show()
pkt = sniff(filter=’icmp’, prn=print_pkt)
```

Firstly, we made the program executable with the command chmod a+x mycode.py. Then, to capture packets, we ran the program with root privileges using sudo ./sniffer.py. Running it as root is necessary because sniffing packets requires access to network interfaces at a low level, which is a privilege typically reserved for the root user.

<img src="/images/image1lab13.png" alt="Original Image">

From another terminal on the same network, we send ping requests to your machine. We did this with a command like ping your machine ip address, that we get from running ifconfig. 

<img src="/images/image2lab13.png" alt="Original Image">

## Task 1.1B: Sniffing Packets


To focus your Scapy packet sniffer on capturing only ICMP packets, we need to set a filter using the Berkeley Packet Filter (BPF) syntax. BPF is a powerful filtering system that allows you to define which packets your sniffer sees based on various protocol and field conditions. For ICMP packets, we set the filter as we setted in task 1.1A. Here is the result:

<img src="/images/image3lab13.png" alt="Original Image">

When we run the sniffer program with the filter set to capture only ICMP packets, the program will ignore all other types of network traffic and only process ICMP packets, and it will print the details of the ICMP packet to our console.

We intended to adjust the Scapy sniffing script to include a BPF filter for capturing TCP packets from a particular IP address destined for port 23. The filter syntax would be filter="tcp and src [source IP] and dst port 23".

We would generate relevant network traffic using a tool like Telnet, connecting from the source IP to any host on port 23. This would create TCP packets that match the filter criteria. However, we didnt receive any packet, if the script had run as intended, it should have captured and displayed information about TCP packets from the specified source IP heading to port 23.

Next, we modify the Scapy sniffing script to include a BPF filter specifically designed to capture traffic to and from the 128.230.0.0/16 subnet. The filter expression would be something like filter="src net 128.230.0.0/16", which is designed to capture all traffic crossing the boundaries of the specified network.

```
#!/usr/bin/env python3
from scapy.all import *
def print_pkt(pkt):
pkt.show()
pkt = sniff(filter='src net 128.230.0.0/16', prn=print_pkt)
```

Then we pinged a device from another subnet, we run the sniffer program and capture the intercepted packets wich shows destination as the victim ip.

<img src="/images/image4lab13.png" alt="Original Image">

## Task 1.2: Spoofing ICMP Packets

We created a Python script utilizing Scapy to construct and send a spoofed ICMP echo request packet. The script includes:

```
from scapy.all import *
a = IP() 
a.dst = ’10.9.0.5’ 
b = ICMP() 
p = a/b
p.show()
send(p) 
```

We the set up Wireshark on the target VM to monitor incoming packets and look for an echo reply indicating that the spoofed packet was accepted.

<img src="/images/image5lab13.png" alt="Original Image">

## Task 1.3: Traceroute

This task aims to utilize Scapy to estimate the number of routers (hops) between the virtual machine and a chosen destination, akin to the functionality of the traceroute tool. This will be achieved by incrementally increasing the Time-To-Live (TTL) field of sent packets and monitoring ICMP error messages returned by routers along the path.

```
a = IP()
a.dst = ’8.8.8.8’
a.ttl = 1
b = ICMP()
send(a/b)
```

Begin with the TTL field of the IP object (a.ttl) set to 1. This means the first router that receives the packet will drop it and, if compliant, send back an ICMP Time Exceeded message.

<img src="/images/image6lab13.png" alt="Original Image">

For each TTL value, an ICMP Time Exceeded message is expected from each router the packet traverses. The source IP of these messages will reveal the route taken by the packets.

When a.ttl is set to 3 the packet reaches the expected destination:

<img src="/images/image7lab13.png" alt="Original Image">

The Wireshark capture data indicates a network route where packets originate from the IP 10.0.2.15 and pass through at least one router with the IP 128.230.0.1. The destination for the ICMP echo request packets is identified as 8.8.8.8, Google's public DNS server, whereas the destination for the TCP packets involved in the three-way handshake is 35.224.170.84, which is likely a web server.

##  Task 1.4: Sniffing and-then Spoofing

The goal of this task is to create a sniff-and-then-spoof program that captures ICMP echo requests on a virtual machine and responds with spoofed echo replies, ensuring that the ping command always receives a reply, regardless of the target's availability.

```
#!/usr/bin/env python3
from scapy.all import *


def spoof_reply(packet):
    
    if packet.haslayer(ICMP) and packet.getlayer(ICMP).type == 8:
        ip = IP(src=packet[IP].dst, dst=packet[IP].src)
        icmp = ICMP(type=0, id=packet[ICMP].id, seq=packet[ICMP].seq)
        data = packet[Raw].load if packet.haslayer(Raw) else b''
        spoofed_packet = ip/icmp/data
        send(spoofed_packet, verbose=0)
        print(f"Sent spoofed reply from {ip.src} to {ip.dst}")

sniff(filter="icmp", prn=spoof_reply, store=0)
```

This program monitors network traffic for ICMP echo requests and responds to each one with a forged ICMP echo reply, regardless of whether the original destination is up or not. The script defines a function, spoof_reply, that crafts these fake replies by swapping the source and destination IP addresses in the captured packet, setting the ICMP type to 0 (echo reply), and preserving the original sequence number and data. It then sends this spoofed packet back to the unsuspecting source. This function is called for each ICMP packet detected by the Scapy sniff function, which listens for all ICMP traffic on the network.

We ran the Scapy script on the VM to actively listen for ICMP requests and send out spoofed replies. Initiated ping requests from the user container to various IP addresses including no-existence ones to generate ICMP echo requests.

<img src="/images/image8lab13.png" alt="Original Image">


The output from the ping command indicates that ICMP echo requests sent to an IP address, which does not exist, is receiving replies. This shows that the ping to a non-existent IP address is unexpectedly successful. The statistics show that 7 out of 10 packets were received, which is indicative of the sniff-and-then-spoof program running on the VM intercepting the ICMP requests and sending spoofed replies, creating the illusion that the IP address is alive and responding.

<img src="/images/image9lab13.png" alt="Original Image">


The ping output shows multiple responses for some of the ICMP echo requests sent to the IP address 8.8.8.8, marked with "(DUP!)" to indicate duplicates. This behavior is consistent with what one would expect to see when a sniff-and-then-spoof program is active on the network. As the legitimate echo replies come from the actual destination, the spoofing program on the VM also sends out fabricated replies. This results in the ping command receiving two responses for some of the echo requests: one from the actual target and one from the spoofing program.
