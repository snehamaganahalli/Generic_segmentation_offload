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



