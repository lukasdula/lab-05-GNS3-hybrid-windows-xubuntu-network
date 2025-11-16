# 2 - HTTP Apache Communication

<br><br>

## **2.1 Introduction**

This chapter focuses on setting up and verifying HTTP communication between Xubuntu and Windows clients using the Apache2 web server. The goal is to install and configure Apache on the Xubuntu server, replace the default web page with a custom one, and confirm access from both clients through IP-based HTTP requests. Tests include local access, cross-network communication, and verification using _curl_ and a web browser.

![](images/Pasted%20image%2020251110214733.png)

<br><br>

## **2.2 Topology**

| Device               | Interface | Connected to → | Peer Interface | IP Address                | Subnet Mask   | Gateway      |
| -------------------- | --------- | -------------- | -------------- | ------------------------- | ------------- | ------------ |
| **R1 (Router)**      | Gi0/1     | SW1            | Gi0/2 (SW1)    | 192.168.10.1              | 255.255.255.0 | –            |
| **R1 (Router)**      | Gi0/0     | SW2            | Gi0/2 (SW2)    | 192.168.20.1              | 255.255.255.0 | –            |
| **Windows-PC1**      | Ethernet0 | SW1            | Gi0/1 (SW1)    | DHCP (192.168.10.100–150) | 255.255.255.0 | 192.168.10.1 |
| **Xubuntu-Server**   | gi0/0     | SW2            | Gi0/0 (SW2)    | 192.168.20.10             | 255.255.255.0 | 192.168.20.1 |
| **Xubuntu-Client-1** | gi0/0     | SW2            | Gi0/1 (SW2)    | DHCP (192.168.20.100–150) | 255.255.255.0 | 192.168.20.1 |

<br><br>

## **2.3 Steps**

1. Install the **Apache2** package on the Xubuntu server using the package manager.
    
2. Verify that the Apache service is running and enabled to start automatically.
    
3. Edit the default web page to include a custom text for testing.
    
4. Ensure that the firewall does not block HTTP traffic on port 80 (open or disable if necessary).
    
5. Test local access from the Xubuntu server using a browser or the `curl` command.
    
6. From the Xubuntu client, access the web page hosted on the server using the IP address `http://192.168.20.10`.
    
7. From the Windows client, verify HTTP access by opening the same address in a browser to confirm cross-network communication.

<br><br>

## **2.4 Apache Installation, Check and Configuration**

#### Xubuntu-Server

* Update package lists and install the Apache2 web server package.

 ```
sudo apt update
sudo apt install apache2 -y
 ```
![](images/Pasted%20image%2020251110184047.png)

### Apache Service Verification

* Check the Apache service status to confirm it is active and running.

 ```
sudo systemctl status apache2
 ```
![](images/Pasted%20image%2020251110184550.png)

### Edit Default Web Page

* Edit the default Apache web page to display a custom message and design for testing the HTTP service.

```
sudo nano /var/www/html/index.html
```
![](images/Pasted%20image%2020251110205545.png)


* Replace the existing content with the following example:

```
<html>
  <head>
    <title>Lab 05 - Apache Test Page</title>
    <style>
      body {
        background-color: #d6eaf8;
        font-family: Verdana, sans-serif;
        text-align: center;
        margin-top: 120px;
      }
      h1 {
        color: #1b4f72;
      }
      p {
        color: #154360;
        font-size: 18px;
      }
    </style>
  </head>
  <body>
    <h1>Lab 05 - HTTP Communication Test</h1>
    <p>This web page is hosted on the Xubuntu server and confirms that Apache is working correctly.</p>
    <p>Access this page from Windows or Xubuntu clients using <b>http://192.168.20.10</b>.</p>
  </body>
</html>
```
![](images/Pasted%20image%2020251110205517.png)


* Open a web browser on the **Xubuntu server** and visit:

```
http://localhost
```
![](images/Pasted%20image%2020251110210021.png)

<br><br>

## **2.5 Verify HTTP Communication from Clients**

* Open a web browser on the **Windows-PC1** and enter the Xubuntu server’s IP address in the address bar:  

 ```
http://192.168.20.10
 ```
![](images/Pasted%20image%2020251110213334.png)

* Open a web browser on the **Xubuntu-client-1** and do the same test using the same address:  

 ```
http://192.168.20.10
 ```
![](images/Pasted%20image%2020251110213443.png)

Both clients display the same “Lab 05 – HTTP Communication Test” page, confirming that the Apache server on the Xubuntu machine is correctly reachable over the network.

<br><br>

## **2.6 Conclusion**

HTTP communication between Windows and Xubuntu systems is successfully established through the Apache2 web server running on the Xubuntu Server. Both clients (Windows PC and Xubuntu Client) access the same custom test page via the server’s IP address, confirming proper configuration of Apache and full connectivity across the hybrid network.

---

Next part: [SSH Remote Access between Systems](03-ssh-remote-access-between-systems.md)





