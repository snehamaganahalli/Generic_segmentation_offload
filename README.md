**Generic Segmentation Offload and Generic Receive Offload:**

**Generic Segmentation offload:**\
Large data blocks must be split into smaller packets to fit networks MTU\
Improve performace. How performance improved n GSO?

Without GSO:

Application=====> TCP/IP stack =======> segmentation ======> Driver=====> NIC

With GSO (up to down in stack):

Application===> TCP/IP stack =======> super packet ===> Driver (GSO)===> NIC

GSO improves performance by delaying segmentation until the driver layer. This allows the networking stack to process fewer larger (super) packet.\
This reduces the CPU overhead and improves the performance.

====================================

**Generic Receive Offload**:\

Receive side optimization:\
(GRO: down to up in stack) \
NIC => GRO (Kernel) ==> IP Layer ====> Transport Layer ====> Application

(GSO: DL)\
NIC => GRO (Kernel) ==> IP Layer ====> Transport Layer ====> Application

Fragmentation does not happen during GRO (Generic Receive Offload) based on the MTU (Maximum Transmission Unit).

============================================================

**Summary:**

stack ==> GSO ===> NIC ==> GRO ==> stack


client1 <========= brlan(eth4) <====== eth5(WAN) <====client2

send iperf traffic DL (WAN to LAN)

**Commands**

ethtool -K eth5 rx-udp-gro-forwarding on \
ethtool -K generic-segmentation-offload on \
If you are sending traffic from eth5 to eth4, then GRO should be enabled on eth5 and GSO should be enabled on eth4.

ethtool -k eth4 | grep -i offload \
ethtool -k eth4 | grep -i gro

Now if you do tcpdump, you will see the large packets on eth5.
Suppose you keep -l 600 in the iperf UDP command, then you can see 3 packets getting aggredated as a single packet.\
Suppose idenification filed (ip hdr filed) in the 3 IP packets are  1,2,3, then in the single aggregated packet you see the identification field as 1. And the next aggregated packet it will be 4 (aggregated packets 4,5,6). The identification field of the 1st small packet is kept as the identification filed in the aggregated packet.
