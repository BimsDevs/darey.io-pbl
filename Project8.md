
## Load Balancer Solution With Apache
 
The objective of this project is to deploy and configure an Apache Load Balancer for Tooling Website solution on a separate Ubuntu EC2 instance, ensuring that users can be served by Web servers through the Load Balancer.

A load balancer intelligently distributes traffic from clients across multiple servers without the clients having to understand how many servers are in use or how they are configured. Because the load balancer sits between the clients and the servers it can enhance the user experience by providing additional security, performance, resilience and simply scaling your website.

How it works:

![1](https://user-images.githubusercontent.com/78465247/111721655-44573e80-8858-11eb-9110-a772cd0399bf.PNG)

When we access a website in the Internet we use an URL and we do not really know how many servers are out there serving our requests, this complexity is hidden from a regular user, but in case of websites that are being visited by millions of users per day (like Google or Reddit) it is impossible to serve all the users from a single Web Server (it is also applicable to databases, but for now we will not focus on distributed DBs).
Each URL contains a domain name part, which is translated (resolved) to the IP address of a target server that will serve requests when opening a website on the Internet. Translation (resolution) of domain names is performed by DNS servers, the most commonly used one has a public IP address 8.8.8.8 and belongs to Google. You can try to query it with nslookup command:
 
nslookup 8.8.8.8; Server:  UnKnown\
Address:  103.86.99.99; Name:  dns.google; Address:  8.8.8.8
 
When you have just one Web server and load increases - you want to serve more and more customers, you can add more CPU and RAM or completely replace the server with a more powerful one - this is called “vertical scaling”. This approach has limitations - at some point you reach the maximum capacity of CPU and RAM that can be installed into your server.
Another approach used to cater for increased traffic is “horizontal scaling” - distributing load across multiple Web servers. This approach is much more common and can be applied almost seamlessly and almost infinitely (you can imagine how many server Google has to serve billions of search requests).
Horizontal scaling allows us to adapt to current load by adding (scale out) or removing (scale in) Web servers. Adjustment of number of servers can be done manually or automatically (for example, based on some monitored metrics like CPU and Memory load).
Property of a system (in our case it is Web tier) to be able to handle growing load by adding resources, is called “Scalability”.
In order to hide all this complexity and to have a single point of access with a single public IP address/name, a Load Balancer can be used. A Load Balancer (LB) distributes clients’ requests among underlying Web Servers/ Google servers.and makes sure that the load is distributed in an optimal way.
Let us take a look at the updated solution architecture with an LB added on top of Web Servers (for simplicity let us assume it is a software L7 Application LB, for example - Apache, NGINX or HAProxy)
In this project we will enhance our Tooling Website solution by adding a Load Balancer to distribute traffic between Web Servers and allow users to access our website using a single URL.
Task
Deploy and configure an Apache Load Balancer for Tooling Website solution on a separate Ubuntu EC2 intance. Make sure that users can be served by Web servers through the Load Balancer.We will implement this solution with 2 Web Servers, the approach will be the same for 3 and more Web Servers.

![2](https://user-images.githubusercontent.com/78465247/111722014-ed9e3480-8858-11eb-9be0-574cd16af378.PNG)

Prerequisites\
Ensure that the instances below are created on AWS: 

1.  Two RHEL8 Web Servers:(Web servers 1&2 as configured in Project 7)
2.  One MySQL DB Server for Apache LB (based on Ubuntu 20.04)
3.  One RHEL8 NFS server

![3](https://user-images.githubusercontent.com/78465247/111722040-f7c03300-8858-11eb-8bf4-2c0dc122224d.PNG)

![4](https://user-images.githubusercontent.com/78465247/111722326-8af96880-8859-11eb-8d65-e578ac97b617.PNG)

### STEPS:
 
#### 1.  Configure Apache As A Load Balancer

         a.  Create an Ubuntu Server 20.04 EC2 instance and name it Project-8-apache-lb.
         b.  Create a Security Group and open the TCP port 80 by adjusting the inbound rule.
         c.  Install Apache Load Balancer on Project-8-apache-lb server and configure it to point traffic coming to LB to both Web Servers:
 
##### Install apache2
sudo apt update\
sudo apt install apache2 -y\
sudo apt-get install libxml2-dev
 
##### Enable following modules:
sudo a2enmod rewrite\
sudo a2enmod proxy\
sudo a2enmod proxy_balancer\
sudo a2enmod proxy_http\
sudo a2enmod headers\
sudo a2enmod lbmethod_bytraffic
 
##### Restart apache2 service
sudo systemctl restart apache2

##### Output





![5](https://user-images.githubusercontent.com/78465247/111724697-99498380-885d-11eb-9c49-b8a96250b32d.PNG)

![10](https://user-images.githubusercontent.com/78465247/111724748-b54d2500-885d-11eb-9e0a-3fe1beedba8b.PNG)

![11](https://user-images.githubusercontent.com/78465247/111724754-b5e5bb80-885d-11eb-9021-7c0d79b8c83c.PNG)

![12](https://user-images.githubusercontent.com/78465247/111724755-b7af7f00-885d-11eb-8e39-0c816f456f24.PNG)

![13](https://user-images.githubusercontent.com/78465247/111724769-c302aa80-885d-11eb-8d8f-9ec5fe27e1b8.PNG)

![14](https://user-images.githubusercontent.com/78465247/111724775-c4cc6e00-885d-11eb-88d1-82ebadc28340.PNG)

![15](https://user-images.githubusercontent.com/78465247/111724785-c72ec800-885d-11eb-976c-ef295ca63975.PNG)

![16](https://user-images.githubusercontent.com/78465247/111724790-cac24f00-885d-11eb-8a78-c1a9365807fd.PNG)

![5](https://user-images.githubusercontent.com/78465247/111724794-cc8c1280-885d-11eb-8af3-e358f590c883.PNG)

![6](https://user-images.githubusercontent.com/78465247/111724802-d0b83000-885d-11eb-8aa3-d02827d81620.PNG)

![7](https://user-images.githubusercontent.com/78465247/111724811-d57ce400-885d-11eb-9bf0-484a7d9c6d37.PNG)

![8](https://user-images.githubusercontent.com/78465247/111724818-d746a780-885d-11eb-9340-8efc49734d55.PNG)

![9](https://user-images.githubusercontent.com/78465247/111724821-da419800-885d-11eb-9880-8ad1763c156d.PNG)

 
