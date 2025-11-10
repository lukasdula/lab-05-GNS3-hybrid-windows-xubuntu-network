
# **Hybrid Windows–Xubuntu Network**


## **Introduction and Objectives**

This lab demonstrates the integration of essential network services across a hybrid Windows and Xubuntu environment. The project combines DHCP, HTTP (Apache), and SSH to provide automated address management, web communication, and secure remote access.  
The goal is to configure these services on the Xubuntu server, verify communication with Windows clients, and confirm full connectivity across both operating systems.



![](images/Pasted%20image%2020251110235336.png)


The objectives are

- Configure a DHCP server on Xubuntu for automatic IP address assignment
    
- Set up an Apache web server and verify HTTP access from all clients
    
- Enable secure SSH communication between Windows and Xubuntu systems
    
- Confirm end-to-end connectivity and service functionality in the hybrid network
    



## **Lab Structure**

1. [DHCP Configuration on Xubuntu Server](01-dhcp-configuration-on-xubuntu-server.md)
    
2. [HTTP Configuration on Xubuntu Server](02-http-configuration-on-xubuntu-server.md)
    
3. [SSH Remote Access between Systems](03-ssh-remote-access-between-systems.md)
    



## **Key Features**

- Centralized DHCP service on Xubuntu assigning IP address to both networks
    
- Apache2 web server providing HTTP access for Xubuntu and Windows clients
    
- Full SSH communication in both directions (Windows <-> Xubuntu)
    
- Proper routing and IP helper configuration on R1 for cross-network operation
    
- Verified functionality through ping, browser, and SSH tests
    



## **Author’s Note**

This lab represents the first complete hybrid integration between Windows and Linux systems in my network portfolio. Setting up DHCP, HTTP, and SSH together showed how cross-platform environments can operate smoothly.  
We can observe how both systems communicate through dynamic addressing, web access, and secure remote sessions, a great step toward understanding real-world mixed infrastructure management.

---

© 2025 – Lukas Dula | Home Network Lab & Portfolio
