

Ansible Configuration Management - Automate Project 7 to 10
===========================================================

Ansible is a modern configuration management tool that facilitates the task of setting up and maintaining remote servers, with a minimalist design intended to get users up and running quickly. Ansible uses an inventory file to keep track of which hosts are part of your infrastructure, and how to reach them for running commands and playbooks.

Most of the routine tasks are automated with Ansible Configuration Management. It also involves writing code using declarative language such as YAML.

Ansible Client as a Jump Server (Bastion Host)
A Jump Server (sometimes also referred as Bastion Host) is an intermediary server through which access to the internal network can be provided. If you think about the current architecture you are working on, ideally, the webservers would be inside a secured network which cannot be reached directly from the Internet. That means, even DevOps engineers cannot SSH into the Web servers directly and can only access it through a Jump Server - it provide better security and reduces attack surface.

On the diagram below the Virtual Private Network (VPC) is divided into two subnets - Public subnet has public IP addresses and Private subnet is only reachable by private IP addresses.



When you reach Project 15, you will see a Bastion host in proper action. But for now, we will develop Ansible scripts to simulate the use of a Jump box/Bastion host to access our Web Servers.

Task
Install and configure Ansible client to act as a Jump Server/Bastion Host

Create a simple Ansible playbook to automate servers configuration

Step 1 - Install and configure Ansible on EC2 Instance
Update Name tag on your Jenkins EC2 Instance to Jenkins-Ansible. We will use this server to run playbooks.

In your GitHub account create a new repository and name it ansible-config-mgt.

Install Ansible

sudo apt update

sudo apt install ansible

Check your Ansible version by running ansible --version



Configure Jenkins build job to save your repository content every time you change it - this will solidify your Jenkins configuration skills acquired in Project 9.
Create a new Freestyle project ansible in Jenkins and point it to your 'ansible-config-mgt' repository.








Configure Webhook in GitHub and set webhook to trigger ansible build.
Enable webhooks in GitHub repository settings.
Go into the tooling github repository:
https://github.com/BimsDevs/tooling

Go to webhook in settings and add webhook:

Go to the Payload box and input the URL: http://jenkins_server_public_ip _address.github-webhook/

http://35.178.239.175:8080/github-webhook/



Configure a Post-build job to save all (**) files, like you did it in Project 9.


Blocker:
Unable to successfully build the job.

 Solution: Updated Jenkins URL with the new public IP address under Jenkins 

              Location. Also ensured that the file in Github was under the Master's record.



Test your setup by making some change in README.MD file in master branch and make sure that builds starts automatically and Jenkins saves the files (build artifacts) in following folder

ls /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/







Note: Trigger Jenkins project execution only for /main (master) branch.

Now your setup will look like this:



Allocate Elastic IP address to Jenkins - Ansible server
Tip Every time you stop/start your Jenkins-Ansible server - you have to reconfigure GitHub webhook to a new IP address, in order to avoid it, it makes sense to allocate an Elastic IP to your Jenkins-Ansible server (you have done it before to your LB server in Project 10). Note that Elastic IP is free only when it is being allocated to an EC2 Instance, so do not forget to release Elastic IP once you terminate your EC2 Instance.

Steps:

Open the Amazon EC2 console at https://console.aws.amazon.com/ec2/.

In the navigation pane, choose Network & Security, Elastic IPs.

Choose Allocate Elastic IP address.

I have chosen Amazon's pool of IPv4 addresses, since the IPv4 address is to be allocated from Amazon's pool of IPv4 addresses.

(Optional) Add or remove a tag.

Choose Allocate.

Click Allocate new address in the Elastic IPs page.

Click on the row of the newly created elastic IP, select Actions and click on Associate elastic address.

Choose the EC2 instance you are integrating.

Click on Associate



Elastic IP address: 18.132.219.41
Step 2 - Prepare the development environment using Visual Studio Code
Downloaded Visual Studio code as the Integrated development environment (IDE) or Source-code Editor that can be used to write some codes. Configured it to connect to Github with the steps below:

Steps to link Visual Studio Code to Github (https://code.visualstudio.com/docs/editor/github)

Open the remote window

On the bar space, select remote SSH - Open configuration file

Select file path to configuration file

Configure the host, host name, user and identity file

Ctrl+S to save. Then launch new terminal







Start by connecting the remote servers to the Ansible-Jenkins server:

To get started with ansible, you must configure the Ansible-Jenkins master controller to connect to your remote host servers, for best practice and security this is achieved using ssh.

All remote host servers would be stored in an inventory file with the below structure in different environments in this instance. 

Note: Ansible uses TCP port 22 by default, which means it needs to ssh into target remote/slave node servers from the Jenkins-Ansible host.

Two ways are:

Copy your private (.pem) key and provide a path to it so Ansible could use it to connect. Do not forget to change permissions to your private key chmod 400 key.pem, otherwise EC2 will not accept the key.

Generate a new ssh key, then print the output of the public key by running the below command and copy the public key of the Ansible-Jenkins into the authorization key file of each of the remote servers

Ansible-Jenkins Server:

sudo ssh-keygen

sudo cat .ssh/id_rsa.pub

or

cd .ssh

sudo cat id_rsa.pub

Copy the public authorisation key file and paste in the authorisation key file of the connecting servers.





Connecting server and repeat for the other servers :  sudo vi .ssh/authorized_key



Step 3 - Begin Ansible Development
In your ansible-config-mgt GitHub repository, a new branch was created. This will be used for the development of a new feature. (ansible-5511-feature01)


 In the VS Code Integrated Terminal:

Create a folder and name it ansible: mkdir ansible

cd  into ansible: cd ansible

Clone your repository into the server: 

git clone https://github.com/BimsDevs/ansible-config-mgt.git



Checkout the newly created feature branch to your local machine and start building your code and directory structure
git checkout ansible-5511-feature01



Create a directory and name it playbooks - it will be used to store all the playbook files.
mkdir playbooks

Within the playbooks folder, create your first playbook, and name it common.yml
cd playbooks

touch common.yml

Create a directory and name it inventory - it will be used to keep the hosts organised.
mkdir inventory

Within the inventory folder, create an inventory file (.yml) for each environment (Development, Staging Testing and Production) dev, staging, uat, and prod respectively.
cd inventory

touch dev.yml staging.yml uat.yml prod.yml



Step 4 - Set up an Ansible Inventory
An Ansible inventory file defines the hosts and groups of hosts upon which commands, modules, and tasks in a playbook operate. Since our intention is to execute Linux commands on remote hosts, and ensure that it is the intended configuration on a particular server that occurs. It is important to have a way to organize our hosts in such an Inventory.

Save below inventory structure in the inventory/dev file to start configuring your development servers. Ensure to replace the IP addresses according to your own setup.

Note: Ansible uses TCP port 22 by default, which means it needs to ssh into target servers from Jenkins-Ansible host - for this you need to copy your private (.pem) key and provide a path to it so Ansible could use it to connect. Do not forget to change permissions to your private key chmod 400 key.pem, otherwise EC2 will not accept the key. Also notice, that your Load Balancer user is ubuntu and user for RHEL-based servers is ec2-user.

IP Addresses (private)
NFS : 172.31.7.178

Webserver 1: 172.31.16.162

Webserver 2: 172.31.16.41

Database: 172.31.18.63

Load balancer: 172.31.46.227

Update the inventory/dev.yml file with this snippet of code and save (ctrl+s):
[nfs]

ansible_ssh_user='ec2-user' ansible_ssh_private_key_file=<path-to-.pem-private-key>

[webservers]

ansible_ssh_user='ec2-user' ansible_ssh_private_key_file=<path-to-.pem-private-key>

ansible_ssh_user='ec2-user' ansible_ssh_private_key_file=<path-to-.pem-private-key>

[db]

ansible_ssh_user='ec2-user' ansible_ssh_private_key_file=<path-to-.pem-private-key>

[lb]

ansible_ssh_user='ubuntu' ansible_ssh_private_key_file=<path-to-.pem-private-key>

Updated version
[nfs]

172.31.7.178 ansible_ssh_user='ec2-user' 

[webservers]

172.31.16.162 ansible_ssh_user='ec2-user' 

172.31.16.41 ansible_ssh_user='ec2-user' 

[db]

172.31.18.63 ansible_ssh_user='ubuntu' 

[lb]

172.31.46.227 ansible_ssh_user='ubuntu'

Output




Step 5 - Create a Common Playbook
It is time to start giving Ansible the instructions on what needs to be performed on all servers listed in inventory/dev.

In the common.yml playbook you will write configuration for repeatable, re-usable, and multi-machine tasks that is common to systems within the infrastructure.

Update your playbooks/common.yml file with following code:
name: update web, nfs and db servers
  hosts: webservers, nfs, db

  remote_user: ec2-user

  become: yes

  become_user: root

  tasks:

  - name: ensure wireshark is at the latest version

    yum:

      name: wireshark

      state: latest

name: update LB server
  hosts: lb

  remote_user: ubuntu

  become: yes

  become_user: root

  tasks:

  - name: ensure wireshark is at the latest version

    apt:

      name: wireshark

      state: latest





Examine the code above and try to make sense out of it. This playbook is divided into two parts, each of them is intended to perform the same task: install wireshark utility (or make sure it is updated to the latest version) on your RHEL 8 and Ubuntu servers. It uses root user to perform this task and respective package manager: yum for RHEL 8 and apt for Ubuntu.

Run ansible: 

ansible-playbook -i inventory/dev.yml playbooks/common.yml



Feel free to update this playbook with following tasks:

Create a directory and a file inside it

Change timezone on all servers

Sample syntax:
name: Set timezone to Asia/Tokyo
Hosts: ubuntu

  timezone:

    name: Asia/Tokyo

Run shell Script etc. ..

hosts: all

  tasks:

  - name: Creating a new directory and set time zone with a simple script

    file:

      path: "/your path"  

      state: directory

Updated version:
name: Creating a directory and set time zone with a simple script 
  hosts: all 

  become: true 

  tasks:

  - name: Creating a directory 

    file: 

      path: /playbooks/new-folder  

      state: directory

-  name: Creating a new file inside the new directory

   file:

     path: /playbooks/new-folder/new-file

     state: touch

name: Set timezone to Europe/London
   timezone: 

    name: Europe/London

name: Status 
  command: echo "All tasks completed"





Overall Outputs:











For a better understanding of Ansible playbooks - watch this video from RedHat and read this article.

Step 6 - Update GIT with the latest code
Now all of your directories and files live on your machine and you need to push changes made locally to GitHub.

In the real world, you will be working within a team of other DevOps engineers and developers. It is important to learn how to collaborate with the help of GIT. In many organisations there is a development rule that do not allow you to deploy any code before it has been reviewed by an extra pair of eyes - it is also called "Four eyes principle".

Now you have a separate branch, you will need to know how to raise a Pull Request (PR), get your branch peer reviewed and merged to the master branch.

Commit your code into GitHub:

Use git commands to add, commit and push your branch to GitHub.
git status

git add

           git add inventory/

           git add playbooks/

Or 'git add . ' to add all files / changes

git commit -m "commit message"

git commit -m "Updated playbooks with new timezone and created new directory and a file inside it"



Create a Pull request (PR) from terminal to github repository
git status

git push

    

. Wear a hat of another developer for a second, and act as a reviewer.

. If the reviewer is happy with your new feature development, merge the code to the master branch.

Steps:
Go into the branch which is to be merged to the master in github

Click on Compare and Pull request tab (green)

Click on Merge Pull Request 

Confirm Merge









Head back on your terminal, checkout from the feature branch into the master, and pull down the latest changes.
git checkout master

git pull



Once your code changes appear in master branch - Jenkins will do its job and save all the files (build artifacts) to /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/ directory on Jenkins-Ansible server.



Step 7 - Run first Ansible test
Now, it is time to execute ansible-playbook command and verify if your playbook actually works:

sudo ansible-playbook -i /var/lib/jenkins/jobs/ansible/builds//archive/inventory/dev.yml /var/lib/jenkins/jobs/ansible/builds//archive/playbooks/common.yml

sudo ansible-playbook -i /var/lib/jenkins/jobs/ansible/builds/11/archive/inventory/dev.yml /var/lib/jenkins/jobs/ansible/builds/11/archive/playbooks/common.yml

Output





Blocker:
The below ansible playbook syntax was run but gave an error.

sudo ansible-playbook -i /var/lib/jenkins/jobs/ansible/builds/11/archive/inventory/dev.yml /var/lib/jenkins/jobs/ansible/builds/11/archive/playbooks/common.yml

Solution: 'sudo' was removed and it worked as per above output. This is because the parameter 'become' has been marked as 'yes' in the playbook which implies the sudo privilege already exists and does not need to be duplicated.

ansible-playbook -i /var/lib/jenkins/jobs/ansible/builds/11/archive/inventory/dev.yml /var/lib/jenkins/jobs/ansible/builds/11/archive/playbooks/common.yml



You can go to each of the servers and check if wireshark has been installed by running which wireshark or wireshark --version



Your updated with Ansible architecture now looks like this:



Repeat once again
Update your ansible playbook with some new Ansible tasks and go through the full checkout -> change codes -> commit -> PR -> merge -> build -> ansible-playbook cycle again to see how easily you can manage a servers fleet of any size with just one command!

Checkout into branch: git checkout ansible-5511-feature01

Create a new file: touch test

Push to github: 

Add random comments into the test file in github

Commit change (github)

Compare and Pull request (github)

Merge and confirm merge

Ensure that the (elastic) IP address in the webhook is correct.

Confirm new build and changes are success in Jenkins (Jenkins)

Execute ansible:

ansible-playbook -i /var/lib/jenkins/jobs/ansible/builds/11/archive/inventory/dev.yml /var/lib/jenkins/jobs/ansible/builds/11/archive/playbooks/common.yml  

Checkout into branch: git checkout ansible-5511-feature01
Created a new file: touch test





Push to github: git push


Added random comments into the test file in github
Commit change (github)

Compare and Pull request (github)

Merge and confirm merge



Ensure that the (elastic) IP address in the webhook is correct.


Ensure that the (elastic) IP address in the webhook is correct.
  Confirm new build and changes are success in Jenkins (Jenkins)





Execute ansible:
ansible-playbook -i /var/lib/jenkins/jobs/ansible/builds/11/archive/inventory/dev.yml /var/lib/jenkins/jobs/ansible/builds/11/archive/playbooks/common.yml  





The above shows the execution of automated routine tasks by implementing this Ansible project.

Credits:
darey.io

https://phoenixnap.com/kb/ansible-create-file 

https://docs.ansible.com/ansible/latest/collections/community/general/timezone_module.html

https://www.mydailytutorials.com/ansible-create-files/

https://linuxize.com/post/how-to-set-or-change-timezone-in-linux/
