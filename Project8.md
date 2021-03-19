
## Load Balancer Solution With Apache
 
The objective of this project is to deploy and configure an Apache Load Balancer for Tooling Website solution on a separate Ubuntu EC2 instance, ensuring that users can be served by Web servers through the Load Balancer.

A load balancer intelligently distributes traffic from clients across multiple servers without the clients having to understand how many servers are in use or how they are configured. Because the load balancer sits between the clients and the servers it can enhance the user experience by providing additional security, performance, resilience and simply scaling your website.

How it works:

![1](https://user-images.githubusercontent.com/78465247/111721655-44573e80-8858-11eb-9110-a772cd0399bf.PNG)

When we access a website in the Internet we use an URL and we do not really know how many servers are out there serving our requests, this complexity is hidden from a regular user, but in case of websites that are being visited by millions of users per day (like Google or Reddit) it is impossible to serve all the users from a single Web Server (it is also applicable to databases, but for now we will not focus on distributed DBs).
Each URL contains a domain name part, which is translated (resolved) to the IP address of a target server that will serve requests when opening a website on the Internet. Translation (resolution) of domain names is performed by DNS servers, the most commonly used one has a public IP address 8.8.8.8 and belongs to Google. You can try to query it with nslookup command:
 
nslookup 8.8.8.8; Server:  UnKnown
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



