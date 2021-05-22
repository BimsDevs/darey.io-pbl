

Tooling Website deployment automation with Continuous Integration. Introduction to Jenkins
==========================================================================================

This project is about automation of repetitive / routine tasks. In this project we are going to start automating part of our routine tasks with a free and open source automation server called [Jenkins](https://en.wikipedia.org/wiki/Jenkins_(software)). It is one of the most popular [CI/CD](https://en.wikipedia.org/wiki/CI/CD) tools, it was created by a former Sun Microsystems developer Kohsuke Kawaguchi and the project was originally named "Hudson". So siimply put, Jenkins is a [free and open source](https://en.wikipedia.org/wiki/Free_and_open-source_software) automation server. It helps automate the parts of [software development](https://en.wikipedia.org/wiki/Software_development) related to [building](https://en.wikipedia.org/wiki/Software_build), [testing](https://en.wikipedia.org/wiki/Test_automation), and [deploying](https://en.wikipedia.org/wiki/Software_deployment), facilitating [continuous integration](https://en.wikipedia.org/wiki/Continuous_integration) and [continuous delivery](https://en.wikipedia.org/wiki/Continuous_delivery). It is Java-based.

Following from project 8, we will be utilising Jenkins CI capabilities to make sure that every change made to the source code in GitHub https://github.com/BimsDevs/tooling  will be automatically be updated to the Tooling Website.

According to CircleCI, Continuous integration (CI) is a software development strategy that increases the speed of development while ensuring the quality of the code that teams deploy. Developers continually commit code in small increments (at least daily, or even several times a day), which is then automatically built and tested before it is merged with the shared repository.

Task
----

Enhance the architecture prepared in Project 8 by adding a Jenkins server, configure a job to automatically deploy source codes changes from Git to NFS server.

Below is our target architecture output

![](https://lh6.googleusercontent.com/2c0v2WtN8z5i05WBY_o4P1yZT2tLTPnnT20CGUATO1fgsaEfEc6STy3Mvz-eoLi8e2lB3pchLpvqBhSttwNdUIAi8iYfDs-vox-GaceW1xuMgxYkk7LLxnfGuvO6gF3ItjJwH0_m)

Step 1 - Install and Start Jenkins server
-----------------------------------------

1.  Create an AWS EC2 server based on Ubuntu Server 20.04 LTS and name it "Jenkins"

   Private IP address: 172.31.26.160; Public IP address: 35.177.4.83

1.  Install [JDK](https://en.wikipedia.org/wiki/Java_Development_Kit) (since Jenkins is a Java-based application)

sudo apt update

sudo apt install default-jdk-headless

![](https://lh5.googleusercontent.com/kyx2hKyAqes6HcIXSBWqIk6GAA4kOz1Seg9rw_IQoWTc2hhJYWq3-tiCNf92GP48s0ByHmZSrrDz8xuv5NoMv4qUs7kuwmoFHv6zQ2qg_UCbulkVZ6lSJJDi8mk3fg6nZHDg9qUn)

3\. Install Jenkins

Add the repository key to the system:

wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -

Then append (add) the Debian package repository address to the server's sources.list:

sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ >

      /etc/apt/sources.list.d/jenkins.list'

![](https://lh6.googleusercontent.com/77BKu3Kl9Dwpq697xEopjiPgRLfjB8ry7ylsTnAXnLmLOwFvo1PVvFmqljqP4_tYV-cp439kCqV3h721YVC8lhFPyhU3jCmqBNz47Sq3bhCjJkmk7b4E5IM9N9QotNN0fuwnq34O)

Run update so that apt will use the new repository, then install Jenkins and its dependencies

sudo apt update

sudo apt-get install jenkins

![](https://lh6.googleusercontent.com/hMqBEzXH6COG9IinOdSY71j1_9pGie5cOGpvuiYVXy-LwEsdlE7Oaam3YfCIrRIFcX5PvcqBRplntaZAtwVV3BIA6PXVPf4s4tYNrOsbxb4c9s-75Zg37oTIt0fCcsNXisQNZopg)

4\. Start the Jenkins Server as Jenkins has been installed

sudo systemctl start jenkins

5\. Verify that Jenkins is up and running 

sudo systemctl status jenkins

![](https://lh5.googleusercontent.com/O4bYZQmq9i-yrUCxr1VesSD1M1za33zGO5-q2YlmHgma2Lx7sTc-Q2l3qIiRljQD0amcOKVoDYP20orUFUIl-myuuDYhkrKZn5OMdMd2oDHGVrASKSRvWrgUMeUXpH1fktesI3NT)

6\. Open TCP port 8080 by creating a new Inbound rule in EC2 Security 

   Group. By default Jenkins server uses this port.

![](https://lh4.googleusercontent.com/Zqy4V240QCQC7SkUh-uIb32BWNxc1wSjpXbWoOrEvNpvHJTbsBfhvkTKTjWkaI9Asrh7PPTO0kZQP3InaagvRaCfNK1zvkGcXwAXstuWhv1NJJV8phCsZGpmnY4-yHABLvfIEsWZ)

7\. Perform initial Jenkins setup.

          Go into the web browser to access Jenkins.

http://<Jenkins-Server-Public-IP-Address-or-Public-DNS-Name>:8080

http://35.177.4.83:8080

Output:

![](https://lh4.googleusercontent.com/Wb6W7BZejq8P--FNMD4v6SiyCCjVwqMAEA6p9JIdwRrmWg3EFyWUfVIdaUNxVlOZMqJ318c0EDK6fCn4UcwmzH-vMeCHFihu142rNYzDKm9DD4WkQG5CdSi0naKNb6MsrdHZKZYP)

There will be a prompt to provide a default admin password.This can be retrieved from the server using the command below:

sudo cat /var/lib/jenkins/secrets/initialAdminPassword

![](https://lh6.googleusercontent.com/RVQY_H8JAqyw1d8YKj_c1C6CvZOwNkWrB-uEahusgjJEC7XvxN_Ez6R_8x9Vx74EFQ1CtFudhRqLGZLb5fTbCJ5_JdTuBJM7DYB4dY48prHvsnN5pb32AMWZbYRMn_tqqsl7x1BG)

![](https://lh4.googleusercontent.com/6CcX7CbfHTyO66mO95h9PCR0fSQYf07uXc1d1187V6khM3jjhaiefUQyDRr00gJA1zU0rTXljqVsTnmgMxMVFf23IjSvoK-qYKuQJxNnsTAIrJoE_YW-i8G6F2g4FBJ-O0IL4pK1)

Then you will be asked which plugings to install - choose suggested plugins.

![](https://lh3.googleusercontent.com/4jDSfj4jiAp-S5Wud3GA0gPcpxYSobns1Kho8uNhzewfLVx2atODaO5rcU_LSsv9ZNAEkhcIdtCeJb-dCXUbOIwxcJx2TvYDQzGiKGH0vG-yyNNpUTKDeiLzCWpPL0lrSRViJKEx)

![](https://lh6.googleusercontent.com/cgtH71OWpOG9b7JHSvtF4RuSEWrKmcmJnmZ5r0uCR3HUuBszuXvMbBD5Sdb80ZtkLm-qsbeQfBvTX_MEAXu0d7UxdMkLlGPu-KZ1ly-6zQmRd5i0PT9vJ_mpYh8U1xZiv3YycPfa)

Once the plugins installation has been completed, create an admin user and you will get your Jenkins server address.

![](https://lh4.googleusercontent.com/C5rXn7IwipF68kHXK24XeS8m6UBZPZ1pXBzGgInBZGDw2WwjGBG3R20q67Y5SLFnLa8dad8PiWfTedjnAcB0job0HyRNbhGy9HsxfXBPsXtrMbr602Jea-RlEJGngdTQghI6OK4P)

![](https://lh3.googleusercontent.com/FN29mJxgvQDvVfNoU6K5dIYU6PeeN57te7fYrzWKNg3ivfYBfF3sWOdi2Z-sU8Vx5csGmJgHl00lU2_O7PJ9UxqzTsXNn_FrLFJBSQJvvSZtXoLU14iu2sz2VoLUwln5n-OD9or5)

![](https://lh3.googleusercontent.com/_qY060vTZXG6rJforn6edPT69Z1Q8sAeoIqk1LZCBAQd9T13WjkU5eUzJtRy_fGWeAI3yoWAD_i0AadnivKt2kOc-G2lkCS7w_yDdiWaUJdZ4foZtssWb8bO4H89ML0A6FShq1kG)

The installation is completed!

Step 2 - Configure Jenkins to retrieve source codes from GitHub using Webhooks
------------------------------------------------------------------------------

In this part, you will learn how to configure a simple Jenkins job/project (these two terms can be used interchangeably). This job will be triggered by GitHub [webhooks](https://en.wikipedia.org/wiki/Webhook) and will execute a 'build' task to retrieve codes from GitHub and store it locally on Jenkins server.

STEPS

1.Enable webhooks in GitHub repository settings.

1.  Go into the tooling github repository:

https://github.com/BimsDevs/tooling

1.  Settings

2.  Click on Webhooks on the menu

3.  Add Webhooks

4.  Go to the Payload box and input the URL: http://jenkins_server_public_ip _address.github-webhook/

http://52.56.144.83:8080/github-webhook/

1.  Content type: Application/jason

2.  Select just the hook event

3.  Click on Add Webhook

Output

![](https://lh4.googleusercontent.com/Pi-lOM2o9WDmeSBoru_-3hvIidxDI05eRulLFZ-culjYYSinjPBqiWHCxJ1MW2IsPM-pslSkpBQPUhPU19x04QBoHS9wvUeI04HEK1T9gJZOhmWDceHphtdBtekcH73-m7ZlEWtz)

2\. Create a 'Freestyle project' 

Go to Jenkins web console, click "New Item" and create a "Freestyle project"

![](https://lh5.googleusercontent.com/wHwRrobKfOBQtv5eyOA7bkB1GgCYJzBwq48QS0KYx-IjRpSlJkA_PVoKckO4K4A79nEFDVJa7-twGcvVwAXCfCK9uu-XrhYFYQW7VhIP-cjZDmCRHj_Da8y3tnn00ErwCdhmB3GE)

![](https://lh6.googleusercontent.com/buDv5y6PBx3XusHQNFf3SbLx_C2l7H_ZCy8JiyUpNEuwC29fsomtLsolK6QyxgEa_qgm0IdTTnoFX4GXstiKCMGr8ztbbcPZEsER0dqsAmq6CQQb1P7ugbew8KMm5TAE0-IjIQiz)

3.Configure Jenkins Freestyle project to enable it access Github repository files.  

First, connect your Github repository to Jenkins Freestyle project by copying the Tooling Github repository URL and placing it in the Jenkins project along with the credentials (user/password) under the GIT repository section, so Jenkins could access files in the repository.

https://github.com/BimsDevs/tooling.git

![](https://lh6.googleusercontent.com/MtxtzTijCF2bog_5qXjouB1wXee781oYDJLbuEjEHaHtHBedIbKdt2Veb09l2GeD8ywMir0SfJ6REaC1GWoOgb9c18kV5ZvDqToQ5pmBpokwhX15AwZxSlFboXU1COxeUVaDp-Vw)

![](https://lh4.googleusercontent.com/pny925J0rO0QexQD_07XRYRGHPhJjTRLGzkWDfLmRO56li8FtqCudiHpg_USvGc4ETe9ST0CGv56jVhUR-JJ6CqTwYXZ2uU3asCduh4M9cqHl3ftk231qLQ-p4865A8QYF_7T7w9)

![](https://lh3.googleusercontent.com/CQJsE4xzqm8W0GmEZiVtzHBl6ARzBhhGwa8nHIWvFq7YNXPTxhjHUWPB1FwJoHmrv6yHLgbVLPhG8C2L1VMKK4-TliDN9_8psLE98mkT3jJn4Pj1OAwT5WEStcED2JxfMOzGxTjh)

Save the configuration and let us try to run the build. For now we can only do it manually. Click "Build Now" button, if you have configured everything correctly, the build will be successful and you will see it under #1

![](https://lh6.googleusercontent.com/JHBqO_hNCliDVd9VHIO32KdkrMKJl9fzE04W-mO8XihhNjWkg3RlYsn8dHvoLDRGnn_92ua6KvRBNopMf0vjSKf7MAIzqS57B6BUip3GemaIMDFzFzSIJZaBs1KewCi-88tgHNwl)

![](https://lh6.googleusercontent.com/3F44zlYl2NVbY7Y5-lE-UfEmX8bKj9LCha7W7U_U1gls-oE7hZo_RuzftjsPfnSkfjjrUjUxAIpGpjmT3MP4bp1Nw2K-Uz4bElw0SoYqp-6CdzieF5VZ6kDClv8mtSZUHPhQeu20)

Above output confirm that the Jenkins Build was successful

Verify that the Jenkins build was successful:

Open the build and check in "Console Output" if it has run successfully.

The above output confirms it is running. However, this still needs to be automated as it is currently only running when we trigger it manually. 

4\. Automate Jenkins job that receives files from GitHub by webhook trigger

Click "Configure" your job/project and add these two configurations

1.  Configure triggering the job from GitHub webhook:

![](https://lh6.googleusercontent.com/K_q98odZojWdgfZOpR5Faq4GCGUl-WNp2jkkkDmaKh2m_CYGNdaMIEi1mJNp789o5fH2HCykpEXqL8qI08NCsq7_omGSeZVR19HaELfvt3ZIXmQ6ITIRtUKiGK8NpgHn-yrfsRwp)

b.Configure "Post-build Actions" to archive all the files - files resulted from a build are called "artifacts".

*Click on Post-Build Action and select archive artifacts and save.

![](https://lh5.googleusercontent.com/u03WKOFTSxlQi7fKWEdz721L7s1-595qmQQ6dkfB0SIa_WyqpfvW3jPIuYxB5wLmoFlYkvpeUy246HYTLorPuB6XS4zX0hqfvaCnYEDyCtmbJ4V25AtYV_VQDV_1_tcU9Ai6Te_g)

Now, go ahead and make some changes in any file in your GitHub repository (e.g. README.MD file) and push the changes to the master branch.

You will see that a new build has been launched automatically (by webhook) and you can see its results - artifacts, saved on Jenkins server.

You have now configured an automated Jenkins job that receives files from GitHub by webhook trigger (this method is considered as 'push' because the changes are being 'pushed' and files transfer is initiated by GitHub). There are also other methods: trigger one job (downstream) from another (upstream), poll GitHub periodically and others.

By default, the artifacts are stored on Jenkins server locally

ls /var/lib/jenkins/jobs/tooling_github/builds/<build_number>/archive/

Step 3 - Configure Jenkins to copy files to NFS server via SSH
--------------------------------------------------------------

Now we have our artifacts saved locally on Jenkins server, the next step is to copy them to our NFS server to /mnt/apps directory.

Jenkins is a highly extendable application and there are 1400+ plugins available. We will need a plugin that is called ["Publish Over SSH"](https://plugins.jenkins.io/publish-over-ssh/).

1.  Install "Publish Over SSH" plugin.

On main dashboard select "Manage Jenkins" and choose "Manage Plugins" menu item.

On "Available" tab search for "Publish Over SSH" plugin and install it by clicking on 'install without restart'

![](https://lh4.googleusercontent.com/tqSse-qRZI8M3fcPrgVCiH7lWjkpbfldGsQMuK0YhLmm0WL1Z14vyE0bt_TARnopjnmUFCwiSIkZ5jEScw-IHZI9XONEEO50KOTLxLIFZcZ6Uh5MKEvC7yiZtZjm0cTxk1YUIHZi)

![](https://lh4.googleusercontent.com/da-iSdm3v4e2fM8cwIZXsigFxZFP-z3AS7B3azb1M6dS3_dU84qvFqCCW8YlitHL5EgDXdUIW5ZDEDV4QGFnLYcoFlXGvjrpM2O6SdK1ACpTbwJhraTlgG-DNhNTsWYvc5lcLXWk)

2\. Configure the job/project to copy artifacts over to NFS server.

On main dashboard select "Manage Jenkins" and choose "Configure System" menu item.

![](https://lh4.googleusercontent.com/_bnrcewJyrWXGj6NA3Gfm-aPpPHEdfrwPcSv5DphzRlLTZJ59lWA5ZaNhrvPj2t_hUiFLtnVa2IA2fsR2nNSki98iFUpqCWBmg5leI-IrjB_wPfdj53Eyv9zxXyLky68GV2LZC_5)

Scroll down to Publish over SSH plugin configuration section and configure it to be able to connect to your NFS server:

1.  Provide a private key (content of .pem file that you use to connect to NFS server via SSH/Putty) 

2.  Arbitrary name: 'credentials to connect to NFS server'

3.  Hostname - can be private IP address of your NFS server: I managed to use both the private and public IP address:'172.31.7.178'

4.  Username - ec2-user (since NFS server is based on EC2 with RHEL 8)

5.  Remote directory - /mnt/apps since our Web Servers use it as a mounting point to retrieve files from the NFS server

Test the configuration and make sure the connection returns Success. Remember, that TCP port 22 on NFS server must be open to receive SSH connections.

Grant permission so that Jenkins can access /mnt/apps in NFS server:

chown ec2-user:ec2-user /mnt/apps/

![](https://lh4.googleusercontent.com/tWPATMR9No0PS06JNSehFWffI6ZKV9tVe4I4NJfb1g0pYZ26W1io3-UE6i74kNU0oRWvyecVzDAl2D5mdcuTKqmnEb_-eurQUr2T2vq4uAMQtrOAU7Vf553p44U_PJ3B1cQ3Q4qu)

![](https://lh3.googleusercontent.com/0i4JlY7116HA0MlUXNNFAXf-nkX94uM1tyuy1X0wI5lwGWhtZcM0Ts6mvrgwCtrj_nfhmWAB_s0k4ywPcdQUkzoJeKz2FwcxN1_AlHKss8csC_z3IN3VWGy-W0UWw3TwXeUsu-k5)

Save the configuration, open your Jenkins job/project configuration page and add another "Post-build Action"

![](https://lh5.googleusercontent.com/SJcFVSGM6MM34IAcQGM_YlC3uGPYYLqTp7R_KUK6o3ZmLY_j_BRVpTF7sz6j7E1DMLC7V7g2RUHcmDyxWHl_fa-aaPe5NckLsMXL0P4tkAOerKcYW_VPBH7YSd73dVeGrF5VBY-C)

We need to configure Jenkins in order to send all files produced by the build into the previously defined remote directory. The task here is to copy all files and directories - so we use **. Otherwise, [this syntax](http://ant.apache.org/manual/dirtasks.html#patterns) can be used If you want to apply some particular pattern to define which files to send.

Save this configuration.

Test if configuration is working:

Change something in the README.MD file in the GitHub Tooling repository. Webhook will trigger a new job and in the "Console Output" of the job you will find something like this:

![](https://lh4.googleusercontent.com/6F-RsPm_U2kJJTlzHMwtH1aeyEax1rVdITGMiE9qRYJG4Ubas_xEx-hUP45_XvEH4qz8ahsoaSdTHbX67SjuODcW9OhEgpQefwrNfKsmZ5rvUjCqU14RLvAzsWLvvM1QBEEyjJf3)

Output below shows that a new job #3 has been created as a result of the push from Github with the Webhook. 

![](https://lh6.googleusercontent.com/2EyLauhQtDRBlGDbxej1hD8BxAZG1Lvft7tu02bE4M-ANxvQobwumBtKvzaZV-2Z3OQ2WIuTdHYyqjZP51E1cATCYoYmty9AH5HRPYQqHgGfTQKl7_bONBnnTIIbjHQWhx_i85qo)

![](https://lh6.googleusercontent.com/Y1S_jJAsk97KUITbBa3KpQPEv2V2SGL0tZS2OkAKAErCJeL2hc__PSzLTNtxPonPKLtI0q0l050o8YGYwSPC4msZwAMeqsopskzYrei9cjmMe6EA0Y9EdXUA6qHCj3FvYraJ4bke)

SSH: Transferred 25 file(s)

Finished: SUCCESS

The above confirms that the files have been successfully transferred over the SSH

To make sure that the files in /mnt/apps have been updated - connect via SSH/Putty to your NFS server and check README.MD file

cat /mnt/apps/README.md

![](https://lh6.googleusercontent.com/KT8iBWFf49H6tr-YJ_L1OrQQ-YQqui5mDWc7JfaktIPTWgG4-CGKZfS44jvRcm_5dxh_skvejaWRZIyTNbZBWzrGssUzkTFrIKzob14xLrp2WNHLfvEgEFj8vGT5zHXZS8O2JAz_)

Changes made in the README.md file is evidenced and this confirms that the Jenkins job is working fine. 

Credits

darey.io

Continuous integration/delivery/deployment: <https://circleci.com/continuous-integration/>

Jenkins: <https://www.digitalocean.com/community/tutorials/how-to-install-jenkins-on-ubuntu-20-04>
