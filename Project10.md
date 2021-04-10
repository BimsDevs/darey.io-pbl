
## Load Balancer Solution With Nginx and SSL/TLS

This project involves the implementation of an Nginx Load Balancing Web Solution with secured HTTPS connection and with periodically updated SSL/TLS certificates. \
In this project, the aim is to configure an Nginx  as a Load balancer solution. A load balancer intelligently distributes traffic from clients across multiple servers without the clients having to understand how many servers are in use or how they are configured. \
We will also take a step further to ensure that the connections to the above Web solution are secured and the information is encrypted in transit. Hence, the connection over secured HTTP (HTTPS protocol), its purpose and implementation requirements is also looked at. \
When data is moving between a client (browser) and a Web Server over the Internet - it passes through multiple network devices and, if the data is not encrypted, it can be relatively easily intercepted by someone who has access to the intermediate equipment. This kind of information security threat is called Man-In-The-Middle (MIMT) attack.
This threat is real - users that share sensitive information (bank details, social media access credentials, etc.) via non-secured channels, risk their data to be compromised and used by cybercriminals.

### Transport Layer Security / Secure Sockets Layer 
SSL and its newer version, TSL - is a security technology that protects connection from MITM attacks by creating an encrypted session between browser and Web server. Here we will refer to this family of cryptographic protocols as SSL/TLS - even though SSL was replaced by TLS, the term is still being widely used. 

A security protocol (cryptographic protocol or encryption protocol) is an abstract or concrete protocol that performs a security-related function and applies cryptographic methods, often as sequences of cryptographic primitives. A protocol describes how the algorithms should be used.
 
SSL/TLS uses digital certificates to identify and validate a Website. A browser reads the certificate issued by a Certificate Authority (CA) to make sure that the website is registered in the CA so it can be trusted to establish a secure connection.
More details on the different types of SSL/TLS certificates can be found here - here. Tutorial on how it works can also be found here - here or an additional resource here. 
In this project I have registered my website with LetsEnrcypt Certificate Authority, and used a shell client recommended by LetsEncrypt - cetrbot to automate certificate issuance. 

### Task
This project consists of two parts:
Configure Nginx as a Load Balancer
Register a new domain name and configure secured connection using SSL/TLS certificates


My target architecture will look like this:
<img src="https://user-images.githubusercontent.com/78465247/114275355-2b3a4b80-9a1a-11eb-9b5b-db618058b5a6.PNG" width="800" height="400">

### Part 1
### Configure Nginx As A Load Balancer
   1. created an EC2 VM based on Ubuntu Server 20.04 LTS and named it Nginx LB. This will serve as my Loadbalancer: nginx-lb

   2. Open Firewall - TCP port 80 for HTTP connections, and TCP port 443 to 
      secure HTTPS connections.

   3. Update /etc/hosts file for local DNS with Web Servers’ names (e.g. 
       Web1 and Web2) and their local IP addresses. 

   4. Install and configure Nginx as a load balancer to point traffic to the 
      resolvable DNS names of the webservers. 
      
IP Addresses:\
Web1 - 172.31.16.162 \
Web2 - 172.31.16.41 \
Nginx LB - 172.31.46.227 

##### Install Nginx
sudo apt update \
sudo apt install nginx 
 
##### Configure Nginx LB using Web Servers’ names defined in /etc/hosts
 sudo cat /etc/hosts 
 
 <img src="https://user-images.githubusercontent.com/78465247/114276243-af420280-9a1d-11eb-8d80-9c64016fedc3.PNG" width="800" height="400">
 
 
##### Configure Nginx as a load balancer to point traffic to the resolvable DNS names of the webservers.

sudo vi /etc/nginx/nginx.conf 

### Amend the domain name in below and paste in the file: 



sudo vi /etc/nginx/nginx.conf 

#insert following configuration into http section 

 upstream myproject { \
    server Web1 weight=5; \
    server Web2 weight=5; \
  } 

server { \
    listen 80; \
    server_name www.domain.com; \
    location / { \
      proxy_pass http://myproject; 
    } \
  } \
#comment out this line \
#include /etc/nginx/sites-enabled/*; 



### Amended code


sudo vi /etc/nginx/nginx.conf 

#insert following configuration into http section 

 upstream myproject { \
    server Web1 weight=5; \
    server Web2 weight=5; \
  } 

server { \
    listen 80; \
     server_name www.bimsie.godaddysites.com; \
    location / { \
      proxy_pass http://myproject; 
    } \
  } \
#comment out this line \
#include /etc/nginx/sites-enabled/*; 

## Output
<img src="https://user-images.githubusercontent.com/78465247/114277221-385b3880-9a22-11eb-9c70-0da6a9b56159.PNG" width="600" height="400">


### Restart Nginx and make sure the service is up and running
sudo systemctl restart nginx \
sudo systemctl status nginx

## Output
<img src="https://user-images.githubusercontent.com/78465247/114277308-94be5800-9a22-11eb-9d6f-889748403d7d.PNG"  width="600" height="200">

![Capture 21](https://user-images.githubusercontent.com/78465247/114277684-1ebaf080-9a24-11eb-9c12-f102ce355a56.PNG)


Test that nginx has been installed - I browsed the public IP of the EC2 instance.

## Output:
<img src="https://user-images.githubusercontent.com/78465247/114277012-3e044e80-9a21-11eb-9662-8bd56e3972f3.PNG"  width="800" height="400">

## Part 2
## Register a new domain name and configure secured connection using SSL/TLS certificates
This involves making necessary configurations in order to make connections to our Tooling Web Solution secured. 
  1. register a new domain name using any Domain name registrar - a company that manages reservation of domain names. The most popular ones are: Godaddy.com, Domain.com,     
     Bluehost.com.\
     I used Godaddy.com to register a new domain name and in ‘.com’ domain zone.

			https://bimsie.godaddysites.com/

 
  2. Allocate an Elastic IP and associate it with an EC2 server
  In order to avoid EC2 public IP address changing at reboot, I had to allocate it and enable a static IP address that does not change after reboot and then associate it with     the EC2 server. 
 
### Steps in allocating an IP address can be found [here](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.ht)
 
a. Open the Amazon EC2 console at https://console.aws.amazon.com/ec2/ \
b. In the navigation pane, choose Network & Security, Elastic IPs \
c. Choose Allocate Elastic IP address \
d. I have chosen Amazon's pool of IPv4 addresses, since the IPv4 address is to be allocated from Amazon's pool of IPv4 addresses \
e. (Optional) Add or remove a tag. \
f. Choose Allocate. 
 
 3. Connect elastic IP address to Nginx LB  instance
    After creating and allocating the the elastic IP address, we will now connect it to the connect it to the Ngnix LB EC2 instance with the steps below:
      a. Click Allocate new address in the Elastic IPs page. \
      b. Then, click Allocate on the next page. \
      c. Click on the row of the newly created elastic IP, select Actions and click on Associate elastic address. \
      d. Choose the EC2 instance you are integrating. \
      e. Click on Associate

  ## Outputs
 <img src="https://user-images.githubusercontent.com/78465247/114278247-c89b7c80-9a26-11eb-9007-008ea4bf81f5.PNG"  width="600" height="400">
 
 ![3](https://user-images.githubusercontent.com/78465247/114278340-43fd2e00-9a27-11eb-82fc-8080ee24590b.PNG)
 
 
#### Please note: Allocated Elastic IP address: 35.178.214.102

![4](https://user-images.githubusercontent.com/78465247/114278712-fd103800-9a28-11eb-9785-d00d8142c12f.PNG)

<img src="https://user-images.githubusercontent.com/78465247/114278406-a6eec500-9a27-11eb-8102-fdca9f27e49a.PNG" width="600" height="400">

<img src="https://user-images.githubusercontent.com/78465247/114278409-abb37900-9a27-11eb-94f9-b6bef6ec8a08.PNG" width="600" height="400">
 \


  4. Update 'A' record in the registrar to point to Nginx LB using Elastic IP Address
    We do this by Associating / connecting the Elastic IP to the domain (https://bimsie.godaddysites.com/) with the steps below: \
    a. Go to GoDaddy’s Domain Manager. \
    b. Go to the Manage DNS of the domain you are integrating. \
    c. Replace the Value of record with Type A with the elastic IP you just created. \
    d. Wait for changes to reflect (This takes at least 600 seconds to reflect, depending on the TTL you specified). \
    e. To check if successful, the domain should now load the EC2 instance you pointed to.
    
    
## Outputs
![8](https://user-images.githubusercontent.com/78465247/114280375-d6ee9600-9a30-11eb-8668-3459407da169.PNG)

![9](https://user-images.githubusercontent.com/78465247/114280377-da821d00-9a30-11eb-9673-ab248b89f99f.PNG)





 


 






