
# **3 - SSH Remote Access between Systems**

## **3.1 Introduction**

This section focuses on enabling secure remote access between Windows and Xubuntu systems using SSH (Secure Shell). The goal is to configure the Xubuntu server to accept SSH connections and verify that both Xubuntu and Windows clients can connect to it remotely. SSH ensures encrypted communication, user authentication, and command-line access for network administration tasks.

![](images/Pasted%20image%2020251110233141.png)
## **3.2 Network Topology**

| Device               | Interface | Connected to → | Peer Interface | IP Address                | Subnet Mask   | Gateway      |
| -------------------- | --------- | -------------- | -------------- | ------------------------- | ------------- | ------------ |
| **R1 (Router)**      | Gi0/1     | SW1            | Gi0/2 (SW1)    | 192.168.10.1              | 255.255.255.0 | –            |
| **R1 (Router)**      | Gi0/0     | SW2            | Gi0/2 (SW2)    | 192.168.20.1              | 255.255.255.0 | –            |
| **Windows-PC1**      | Ethernet0 | SW1            | Gi0/1 (SW1)    | DHCP (192.168.10.100–150) | 255.255.255.0 | 192.168.10.1 |
| **Xubuntu-Server**   | gi0/0     | SW2            | Gi0/0 (SW2)    | 192.168.20.10             | 255.255.255.0 | 192.168.20.1 |
| **Xubuntu-Client-1** | gi0/0     | SW2            | Gi0/1 (SW2)    | DHCP (192.168.20.100–150) | 255.255.255.0 | 192.168.20.1 |

## **3.3 Steps**

1. Install and enable the SSH service on the Xubuntu server.  
2. Verify that the SSH daemon (sshd) is active and listening on port 22.  
3. Test SSH connection from Windows client to Xubuntu-server using server IP 192.168.20.10.  
4. Test SSH connection from Xubuntu server to Windows-PC1 using terminal.  

## **3.4 Install SSH**

* Install the OpenSSH package and enable the SSH service to allow secure remote access.

```
sudo apt update
sudo apt install openssh-server -y
```
![](images/Pasted%20image%2020251110221145.png)

 ```
sudo systemctl enable ssh
sudo systemctl start ssh
sudo systemctl status ssh
 ```
![](images/Pasted%20image%2020251110221256.png)

Verify that SSH is active and listening on port 22:

```
sudo ss -tuln | grep :22
```
![](images/Pasted%20image%2020251110221512.png)


The Xubuntu server is now ready to accept SSH connection.



## 3.5 SSH Connection Between Windows and Xubuntu Server

This section verifies full SSH connection between two systems in the network -> the Xubuntu server and the Windows client. The test confirms that secure remote sessions are established successfully, allowing authenticated users to run commands between both systems.

#### **Windows-PC1 to Xubuntu-Server**

```plaintext
ssh xubuntu@192.168.20.10
```
![](images/Pasted%20image%2020251110224112.png)

The Windows client connects to the Xubuntu server and displays the welcome message after successful authentication.


#### **Xubuntu Server to Windows-PC1**

```plaintext
ssh windows@192.168.10.100
```
![](images/Pasted%20image%2020251110231927.png)


The Xubuntu server initiates an SSH connection to the Windows client, confirming reverse connectivity if the Windows SSH feature is enabled.


>**Notes:**To allow SSH connection _from Xubuntu to Windows_, install **OpenSSH Server** via PowerShell (Admin) using:  
`Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0`  
Internet connection is required to download the package from Microsoft repositories.  
After installation, start the service with `Start-Service sshd`.

### Result

All SSH sessions authenticate successfully, confirming full connectivity SSH communication between Windows client and Xubuntu server.


## **3.6 Conclusion**

The SSH configuration enables secure remote communication between Windows and Xubuntu systems. The Xubuntu server accepts SSH connections, and both clients can connect and authenticate through encrypted sessions. The successful communication between all devices confirms stable connectivity and correct SSH setup within the hybrid network.
