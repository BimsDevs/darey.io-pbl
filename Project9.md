
## Tooling Website deployment automation with Continuous Integration. Introduction to Jenkins

This project is about automation of routine tasks. In this project we are going to start automating part of our routine tasks with a free and open source automation server called Jenkins. It is one of the most popular CI/CD tools, it was created by a former Sun Microsystems developer Kohsuke Kawaguchi and the project was originally named “Hudson”. So siimply put, Jenkins is a free and open source automation server. It helps automate the parts of software development related to building, testing, and deploying, facilitating continuous integration and continuous delivery.
 
Following from project 8, we will be utilising Jenkins CI capabilities to make sure that every change made to the source code in GitHub https://github.com/BimsDevs/tooling will be automatically be updated to the Tooling Website.
 
According to CircleCI, Continuous integration (CI) is a software development strategy that increases the speed of development while ensuring the quality of the code that teams deploy. Developers continually commit code in small increments (at least daily, or even several times a day), which is then automatically built and tested before it is merged with the shared repository.
 
 
Task
Enhance the architecture prepared in Project 8 by adding a Jenkins server, configure a job to automatically deploy source codes changes from Git to NFS server.

Below is our target architecture output

<img src="https://user-images.githubusercontent.com/78465247/111720720-68b21b80-8856-11eb-8c4f-f24036085e2c.PNG" width="700" height="400">

#### Step 1 - Install and Start Jenkins server
    a. Create an AWS EC2 server based on Ubuntu Server 20.04 LTS and name it “Jenkins”

         Private IP address: 172.31.26.160; Public IP address: 35.177.4.83

    b.Install JDK (since Jenkins is a Java-based application)
        sudo apt update
        sudo apt install default-jdk-headless

<img src="https://user-images.githubusercontent.com/78465247/112415897-91d22080-8d1c-11eb-9dbf-7a198727e69e.PNG" width="800" height="400">

    c. Install Jenkins

      Add the repository key to the system:
      wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -

      Then append (add) the Debian package repository address to the server’s sources.list:
      sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > \
  	   /etc/apt/sources.list.d/jenkins.list'

<img src="https://user-images.githubusercontent.com/78465247/112417224-fdb58880-8d1e-11eb-9545-d28febfd08c3.PNG"  width="800" height="400">

     Run update so that apt will use the new repository, then install Jenkins and its dependencies
           sudo apt update
           sudo apt-get install jenkins
           
  <img src="https://user-images.githubusercontent.com/78465247/112415027-05732e00-8d1b-11eb-810d-314c1495eb23.PNG" width="700" height="400">
  
  d. Start the Jenkins Server as Jenkins has been installed\
       	   sudo systemctl start jenkins
 
  e. Verify that Jenkins is up and running\
           sudo systemctl status jenkins
      
  <img src="https://user-images.githubusercontent.com/78465247/112416138-0016e300-8d1d-11eb-858e-34c9cdcb4f13.PNG"  width="800" height="400">
  
  f. Open TCP port 8080 by creating a new Inbound rule in EC2 Security Group. By default Jenkins server uses this port.
     ![23](https://user-images.githubusercontent.com/78465247/112418516-87665580-8d21-11eb-8a0e-83fe2c66170c.PNG)
    
  g. Perform initial Jenkins setup.
          Go into the web browser to access Jenkins.

	 http://<Jenkins-Server-Public-IP-Address-or-Public-DNS-Name>:8080

	 http://35.177.4.83:8080


#### Output:
  <img src="https://user-images.githubusercontent.com/78465247/112418720-f3e15480-8d21-11eb-896c-cee2c0e2e21c.PNG" width="800" height="400">
  
There will be a prompt to provide a default admin password.This can be retrieved from the server using the command below:

			sudo cat /var/lib/jenkins/secrets/initialAdminPassword

               
     ![23](https://user-images.githubusercontent.com/78465247/112419159-d1037000-8d22-11eb-8624-626acafdee6a.PNG)  
               
  <img src="https://user-images.githubusercontent.com/78465247/112419004-7e29b880-8d22-11eb-9159-0f23efea005e.PNG" width="700" height="400">
  
  
  Then you will be asked which plugings to install - choose suggested plugins
  
  ![9](https://user-images.githubusercontent.com/78465247/112421440-ef6b6a80-8d26-11eb-841c-3e282c7a0150.PNG)

 
  ![12](https://user-images.githubusercontent.com/78465247/112421070-4c1a5580-8d26-11eb-92e5-4cc3a9d398fe.PNG)
  
 
 Once the plugins installation has been completed, create an admin user and you will get your Jenkins server address.

   <img src="https://user-images.githubusercontent.com/78465247/112419720-ce554a80-8d23-11eb-884d-0ee5f3c6a11f.PNG" width="800" height="400">

   <img src="https://user-images.githubusercontent.com/78465247/112419645-a7971400-8d23-11eb-8ce9-ac94aac14821.PNG" width="800" height="400">

   <img src="https://user-images.githubusercontent.com/78465247/112419743-d90fdf80-8d23-11eb-9ca9-834dc5712282.PNG" width="800" height="400">
   
   The installation is completed!
 
### Step 2 - Configure Jenkins to retrieve source codes from GitHub using Webhooks
In this part, you will learn how to configure a simple Jenkins job/project (these two terms can be used interchangeably). This job will be triggered by GitHub webhooks and will execute a ‘build’ task to retrieve codes from GitHub and store it locally on Jenkins server.
 
#### STEPS

1.Enable webhooks in GitHub repository settings. \
   a. Go into the tooling github repository: \
        https://github.com/BimsDevs/tooling \
   b.  Settings \
   c.  Click on Webhooks on the menu \
   d.  Add Webhooks\
   e.  Go to the Payload box and input the URL: \
            http://jenkins_server_public_ip _address.github-webhook/
 
            http://52.56.144.83:8080/github-webhook/

   f. Content type: Application/jason\
   g. Select just the hook event\
   h. Click on Add Webhook
 
 
#### Output

 <img src="https://user-images.githubusercontent.com/78465247/112420457-2476bd80-8d25-11eb-9b6e-40bfd8c9fea3.PNG" width="800" height="400">
 
 
### 2. Create a ‘Freestyle project’ 
  Go to Jenkins web console, click “New Item” and create a “Freestyle project”
  
![15](https://user-images.githubusercontent.com/78465247/112421981-0eb6c780-8d28-11eb-8b50-afb6e629cdc1.PNG)
![16](https://user-images.githubusercontent.com/78465247/112422007-18d8c600-8d28-11eb-883c-36ecd4ff71ab.PNG)


### 3.Configure Jenkins Freestyle project to enable it access Github repository files.  
 
First, connect your Github repository to Jenkins Freestyle project by copying the Tooling Github repository URL and placing it in the Jenkins project along with the credentials (user/password) under the GIT repository section, so Jenkins could access files in the repository. 
 
           https://github.com/BimsDevs/tooling.git
           
![17](https://user-images.githubusercontent.com/78465247/112422218-6d7c4100-8d28-11eb-8842-a1ba2fb8c731.PNG)
![18](https://user-images.githubusercontent.com/78465247/112422230-72d98b80-8d28-11eb-9872-93c71b989ae6.PNG)
![19](https://user-images.githubusercontent.com/78465247/112422241-766d1280-8d28-11eb-9106-7070eea2df0e.PNG)


Save the configuration and let us try to run the build. For now we can only do it manually. Click “Build Now” button, if you have configured everything correctly, the build will be successful and you will see it under #1


![20](https://user-images.githubusercontent.com/78465247/112422389-b59b6380-8d28-11eb-8853-2017b4c45e1e.PNG)
![21](https://user-images.githubusercontent.com/78465247/112422396-baf8ae00-8d28-11eb-924b-15650acb40df.PNG)


Above output confirm that the Jenkins Build was successful
 
Verify that the Jenkins build was successful:\
Open the build and check in “Console Output” if it has run successfully.
 
 
The above output confirms it is running. However, this still needs to be automated as it is currently only running when we trigger it manually. 
 
 
### 4. Automate Jenkins job that receives files from GitHub by webhook trigger

Click “Configure” your job/project and add these two configurations

      a.Configure triggering the job from GitHub webhook:
      
      ![22](https://user-images.githubusercontent.com/78465247/112426096-378e8b00-8d2f-11eb-8832-e2bd5ba2a5e7.PNG)
  
       
      b. Configure “Post-build Actions” to archive all the files - files resulted from a build are called “artifacts”.\
           *Click on Post-Build Action and select archive artifacts and save.

   ![23](https://user-images.githubusercontent.com/78465247/112426118-42492000-8d2f-11eb-9953-d586a79586e7.PNG)



The confirms the creation of an automated Jenkins job that receives files from GitHub by webhook trigger (this method is considered as ‘push’ because the changes are being ‘pushed’ and files transfer is initiated by GitHub). There are also other methods: trigger one job (downstreadm) from another (upstream), poll GitHub periodically and others.
 
By default, the artifacts are stored on Jenkins server locally\
ls /var/lib/jenkins/jobs/tooling_github/builds/<build_number>/archive/


### Step 3 - Configure Jenkins to copy files to NFS server via SSH
Now we have our artifacts saved locally on Jenkins server, the next step is to copy them to our NFS server to /mnt/apps directory.
 
Jenkins is a highly extendable application and there are 1400+ plugins available. We will need a plugin that is called “Publish Over SSH”.
 
#### 1. Install “Publish Over SSH” plugin.


On main dashboard select “Manage Jenkins” and choose “Manage Plugins” menu item.
 
On “Available” tab search for “Publish Over SSH” plugin and install it by clicking on ‘install without restart’

![31](https://user-images.githubusercontent.com/78465247/112424100-bb467880-8d2b-11eb-8591-60e76a74d436.PNG)
![25](https://user-images.githubusercontent.com/78465247/112424139-d0230c00-8d2b-11eb-879f-cc990b46a27b.PNG)


#### 2. Configure the job/project to copy artifacts over to NFS server.

On main dashboard select “Manage Jenkins” and choose “Configure System” menu item.


![26](https://user-images.githubusercontent.com/78465247/112424267-095b7c00-8d2c-11eb-962e-c538d0ef44ad.PNG)

Scroll down to Publish over SSH plugin configuration section and configure it to be able to connect to your NFS server:
 
a. Provide a private key (content of .pem file that you use to connect to NFS server via SSH/Putty) \
b. Arbitrary name: ‘credentials to connect to NFS server’ \
c. Hostname - can be private IP address of your NFS server: I managed to use both the private and public IP address:‘172.31.7.178’ \
d. Username - ec2-user (since NFS server is based on EC2 with RHEL 8) \
e. Remote directory - /mnt/apps since our Web Servers use it as a mounting point to retrieve files from the NFS server \
f. Test the configuration and make sure the connection returns Success. Remember, that TCP port 22 on NFS server must be open to receive SSH connections. 
 
Grant permission so that Jenkins can access /mnt/apps in NFS server: \
	chown ec2-user:ec2-user /mnt/apps/

![29](https://user-images.githubusercontent.com/78465247/112426463-e6cb6200-8d2f-11eb-8877-14b9aee49c23.PNG)

![27](https://user-images.githubusercontent.com/78465247/112426542-02366d00-8d30-11eb-9f29-0ccae5a04ed5.PNG)


Save the configuration, open your Jenkins job/project configuration page and add another “Post-build Action”

![28](https://user-images.githubusercontent.com/78465247/112426722-53def780-8d30-11eb-8879-23761a0ac6f7.PNG)


We need to configure it to send all files produced by the build into the previously defined remote directory. The task here is to copy all files and directories - so we use (**). 

Otherwise, we can use [this syntax](http://ant.apache.org/manual/dirtasks.html#patterns) can be used If you want to apply some particular pattern to define which files to send.


       
 


