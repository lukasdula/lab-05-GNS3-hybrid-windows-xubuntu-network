# 1 - DHCP Configuration on Xubuntu Server

## **1.1 Introduction**

This section explains how to configure the DHCP service on Xubuntu Server to automatically assign IP addresses to both Windows and Xubuntu clients. The ISC DHCP server runs on Xubuntu with a static IP 192.168.20.10, distributing addresses from two pools: 192.168.10.100–150 and 192.168.20.100–150. Router R1 uses an IP helper address to forward DHCP requests to the server, enabling dynamic IP allocation across both network segments.


![](images/Pasted%20image%2020251110015412.png)

## **1.2 Network Topology**

| Device               | Interface | Connected to → | Peer Interface | IP Address                | Subnet Mask   | Gateway      |
| -------------------- | --------- | -------------- | -------------- | ------------------------- | ------------- | ------------ |
| **R1 (Router)**      | Gi0/1     | SW1            | Gi0/2 (SW1)    | 192.168.10.1              | 255.255.255.0 | –            |
| **R1 (Router)**      | Gi0/0     | SW2            | Gi0/2 (SW2)    | 192.168.20.1              | 255.255.255.0 | –            |
| **Windows-PC1**      | Ethernet0 | SW1            | Gi0/1 (SW1)    | DHCP (192.168.10.100–150) | 255.255.255.0 | 192.168.10.1 |
| **Xubuntu-Server**   | gi0/0     | SW2            | Gi0/0 (SW2)    | 192.168.20.10             | 255.255.255.0 | 192.168.20.1 |
| **Xubuntu-Client-1** | gi0/0     | SW2            | Gi0/1 (SW2)    | DHCP (192.168.20.100–150) | 255.255.255.0 | 192.168.20.1 |

## **1.3 Steps**

1. Configure router **R1** with IP addresses on both interfaces and add an **IP helper address** to forward DHCP requests to the Xubuntu server.
    
2. Assign a static IP address **192.168.20.10** to the Xubuntu server and install the **ISC DHCP server** package.
    
3. Define the network interface used by the DHCP service and create the configuration file with address ranges, subnet mask, and gateway for clients.
    
4. Add a static route on the Xubuntu server to send all outgoing traffic through the router.
    
5. Enable and start the DHCP service, making sure it runs on the correct network interface.
    
6. Verify that both clients (Windows-PC1 and Xubuntu-Client2) receive IP addresses dynamically from the DHCP server.
    
7. Test network connectivity with ping commands between all devices to confirm DHCP and routing functionality.
    

## **1.4 Router (R1) Configuration**

* Configure the router interface connected to the Windows network. Assign an IP address and add an **IP helper address** to forward DHCP broadcasts to the Xubuntu DHCP server located in a different subnet.
    
* Configure the router interface connected to the Linux network with an IP address that will serve as the default gateway for both the Xubuntu server and client.
    

```
enable
configure terminal
interface gi0/1
ip address 192.168.10.1 255.255.255.0
no shutdown
ip helper-address 192.168.20.10
exit
interface gi0/0
ip address 192.168.20.1 255.255.255.0
no shutdown
exit
write memory
```
![](images/Pasted%20image%2020251109224812.png)


## **1.5 Xubuntu DHCP Server Configuration**


1. Assign a static IP address **192.168.20.10/24** to the Xubuntu server on interface **ens3** to ensure consistent network identification.
    
 ```
sudo ip addr add 192.168.20.10/24 dev ens3
sudo ip link set ens3 up
 ```
![](images/Pasted%20image%2020251109235553.png)


2. Install the **ISC DHCP Server** package using the package manager to provide dynamic IP addressing for clients.
    
```
sudo apt update
sudo apt install isc-dhcp-server -y
```
![](images/Pasted%20image%2020251110000154.png)

---
### DHCP Configuration

1. Open the DHCP configuration file and specify the network interface for the service.
    
 ```
 sudo nano /etc/default/isc-dhcp-server
 ```
![](images/Pasted%20image%2020251110001239.png)

Set the interface lines as follows:

```
INTERFACESv4="ens3"
INTERFACESv6=""
```
![](images/Pasted%20image%2020251110001319.png)


2. Create the DHCP configuration file and define address pools for both subnets **192.168.10.0/24** (Windows network) and **192.168.20.0/24** (Linux network).
    
#### Command

```
sudo nano /etc/dhcp/dhcpd.conf
```
![](images/Pasted%20image%2020251110001919.png)

#### Configuration file

```
subnet 192.168.10.0 netmask 255.255.255.0 {
range 192.168.10.100 192.168.10.150;
option routers 192.168.10.1;
option subnet-mask 255.255.255.0;
option domain-name-servers 8.8.8.8;
default-lease-time 600;
max-lease-time 7200;
}

subnet 192.168.20.0 netmask 255.255.255.0 {
range 192.168.20.100 192.168.20.150;
option routers 192.168.20.1;
option subnet-mask 255.255.255.0;
option domain-name-servers 8.8.8.8;
default-lease-time 600;
max-lease-time 7200;
}

```
![](images/Pasted%20image%2020251110005028.png)

---
### Static Route Configuration

1. Add a static route on the Xubuntu server to ensure proper communication with devices in other subnets through the router.
    
2. Set the default gateway to the router’s IP address **192.168.20.1**.
    

 ```
 sudo ip route add default via 192.168.20.1
 ```
![](images/Pasted%20image%2020251110003250.png)

---
### DHCP Service Start and Verification

1. Enable and start the DHCP service to apply the configuration.
    

 ```
sudo systemctl enable isc-dhcp-server
sudo systemctl start isc-dhcp-server
 ```
![](images/Pasted%20image%2020251110003823.png)

2. Check the service status to confirm it is active and running.
    


```
sudo systemctl status isc-dhcp-server
```
![](images/Pasted%20image%2020251110005222.png)

## **1.6 Verify DHCP on each client**

3. Verify that both clients (Windows and Xubuntu) receive correct IP addresses from their respective DHCP pools.
    
4. On each client, display the assigned IP configuration.
    
#### **Windows-PC1**

 ```
 ipconfig /renew
 ```
![](images/Pasted%20image%2020251110010533.png)


#### **Xubuntu-client-1**

 ```
ip a
 ```
![](images/Pasted%20image%2020251110010825.png)

### Results

Both clients (Windows and Xubuntu) successfully received IP addresses dynamically from the Xubuntu DHCP server, confirming full functionality of the DHCP relay and address distribution across networks.



## 1.7 **Diagnostics**

### Windows Client Verification

1. Confirm that the Windows client received a valid IP address from the DHCP server.
    
2. Verify that the IP address, subnet mask, and gateway correspond to the Windows network.
    
3. Test connectivity using the ping command:
    
 ```
ping 192.168.10.1 (Router)
ping 192.168.20.10 (Xubuntu DHCP Server)
 ```
![](images/Pasted%20image%2020251110011745.png)


### Xubuntu Client Verification

1. Confirm that the Xubuntu client received a DHCP-assigned IP address from the pool.
    
2. Check network interface configuration and default gateway.
    
3. Test connectivity using the ping command:
    
 ```
ping 192.168.20.1 (Router)
ping 192.168.10.100 (Windows Client)
 ```
![](images/Pasted%20image%2020251110014038.png)


## **1.8 Conclusion**

This lab demonstrated the configuration of a Linux-based DHCP server and its integration within a routed multi-subnet network. Using an IP helper address on the router, clients from different VLANs successfully received their IP settings. Full connectivity between Xubuntu and Windows systems was verified, confirming proper DHCP relay and cross-platform communication.


---

**Next part:** [HTTP Configuration on Xubuntu Server](02-http-configuration-on-xubuntu-server.md)

