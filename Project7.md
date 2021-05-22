

Ansible Configuration Management - Automate Project 7 to 10

-----------------------------------------------------------

===========================================================

Ansible is a modern configuration management tool that facilitates the task of setting up and maintaining remote servers, with a minimalist design intended to get users up and running quickly. Ansible uses an inventory file to keep track of which hosts are part of your infrastructure, and how to reach them for running commands and playbooks.

Most of the routine tasks are automated with [Ansible](https://en.wikipedia.org/wiki/Ansible_(software)) [Configuration Management](https://www.redhat.com/en/topics/automation/what-is-configuration-management#:~:text=Configuration%20management%20is%20a%20process,in%20a%20desired%2C%20consistent%20state.&text=Managing%20IT%20system%20configurations%20involves,building%20and%20maintaining%20those%20systems.). It also involves writing code using declarative language such as [YAML](https://en.wikipedia.org/wiki/YAML).

[](https://github.com/BimsDevs/ansible-config-mgt/blob/master/Ansible%20Configuration%20Management.md#ansible-client-as-a-jump-server-bastion-host)Ansible Client as a Jump Server (Bastion Host)

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

A [Jump Server](https://en.wikipedia.org/wiki/Jump_server) (sometimes also referred as [Bastion Host](https://en.wikipedia.org/wiki/Bastion_host)) is an intermediary server through which access to the internal network can be provided. If you think about the current architecture you are working on, ideally, the webservers would be inside a secured network which cannot be reached directly from the Internet. That means, even DevOps engineers cannot SSH into the Web servers directly and can only access it through a Jump Server - it provide better security and reduces [attack surface](https://en.wikipedia.org/wiki/Attack_surface).

On the diagram below the Virtual Private Network (VPC) is divided into [two subnets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html) - Public subnet has public IP addresses and Private subnet is only reachable by private IP addresses.

[![](https://camo.githubusercontent.com/172276a65d3dcc11ae6cf575c2318486eff183f9bdf16dffb1dd02d42f119f3b/68747470733a2f2f6c68342e676f6f676c6575736572636f6e74656e742e636f6d2f335245433450535248666d6a6365646e7154485241475949755f6d37795f56475f4150357065647445506a617277454742546b77434644325255374e62617275514b51686e557543774f53357646627757497a6c616e434336444868417a4a595569335a385944663563454d3351436b71516249636a374578506e64454c6d6c423271663832774a)](https://camo.githubusercontent.com/172276a65d3dcc11ae6cf575c2318486eff183f9bdf16dffb1dd02d42f119f3b/68747470733a2f2f6c68342e676f6f676c6575736572636f6e74656e742e636f6d2f335245433450535248666d6a6365646e7154485241475949755f6d37795f56475f4150357065647445506a617277454742546b77434644325255374e62617275514b51686e557543774f53357646627757497a6c616e434336444868417a4a595569335a385944663563454d3351436b71516249636a374578506e64454c6d6c423271663832774a)

When you reach [Project 15](https://dareyio-pbl-expert.readthedocs-hosted.com/en/latest/project15.html), you will see a Bastion host in proper action. But for now, we will develop Ansible scripts to simulate the use of a Jump box/Bastion host to access our Web Servers.

[](https://github.com/BimsDevs/ansible-config-mgt/blob/master/Ansible%20Configuration%20Management.md#task)Task

---------------------------------------------------------------------------------------------------------------

-   Install and configure Ansible client to act as a Jump Server/Bastion Host

-   Create a simple Ansible playbook to automate servers configuration

[](https://github.com/BimsDevs/ansible-config-mgt/blob/master/Ansible%20Configuration%20Management.md#step-1---install-and-configure-ansible-on-ec2-instance)Step 1 - Install and configure Ansible on EC2 Instance

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

1\.  Update Name tag on your Jenkins EC2 Instance to Jenkins-Ansible. We will use this server to run playbooks.

2\.  In your GitHub account create a new repository and name it ansible-config-mgt.

3\.  Install Ansible

sudo apt update

sudo apt install ansible

Check your Ansible version by running ansible --version

[![](https://camo.githubusercontent.com/0cd540ec9bd46c6126d45a5492231c8ead2fc644d95e80f424b0385236e0e799/68747470733a2f2f6c68362e676f6f676c6575736572636f6e74656e742e636f6d2f644a6558622d48583842366e723739484b6f7379315255434b6968574c4f622d754e5549774a445862316a7331652d33676c49466d5531623162787556584738754c67547a76554d71385a696d795166627369535650364d6143302d666c3756415974416236515644496f454656534b5a41334e5952556d524659744c77396d34786c7a46645638)](https://camo.githubusercontent.com/0cd540ec9bd46c6126d45a5492231c8ead2fc644d95e80f424b0385236e0e799/68747470733a2f2f6c68362e676f6f676c6575736572636f6e74656e742e636f6d2f644a6558622d48583842366e723739484b6f7379315255434b6968574c4f622d754e5549774a445862316a7331652d33676c49466d5531623162787556584738754c67547a76554d71385a696d795166627369535650364d6143302d666c3756415974416236515644496f454656534b5a41334e5952556d524659744c77396d34786c7a46645638)

[](https://github.com/BimsDevs/ansible-config-mgt/blob/master/Ansible%20Configuration%20Management.md#configure-jenkins-build-job-to-save-your-repository-content-every-time-you-change-it---this-will-solidify-your-jenkins-configuration-skills-acquired-in-project-9)Configure Jenkins build job to save your repository content every time you change it - this will solidify your Jenkins configuration skills acquired in [Project 9](https://professional-pbl.darey.io/en/latest/project9.html).

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

-   Create a new Freestyle project ansible in Jenkins and point it to your 'ansible-config-mgt' repository.

[![](https://camo.githubusercontent.com/20855e0f232b9bf9e068f10855609bc4fcffcf41e238d1c85c5ba7a90f2e1079/68747470733a2f2f6c68352e676f6f676c6575736572636f6e74656e742e636f6d2f486d444d70594a785f464364317a5163332d64526a32544d4b6478716f703457595935566f4532726f556d4b753655695732326d3339706476614d35756d7042355943346d764e513761507a743079706d5a496c576e566b31692d64577337722d7432427a7a494c324b6362795573386d4272375777645a4c49344868634b5a7553564941337438)](https://camo.githubusercontent.com/20855e0f232b9bf9e068f10855609bc4fcffcf41e238d1c85c5ba7a90f2e1079/68747470733a2f2f6c68352e676f6f676c6575736572636f6e74656e742e636f6d2f486d444d70594a785f464364317a5163332d64526a32544d4b6478716f703457595935566f4532726f556d4b753655695732326d3339706476614d35756d7042355943346d764e513761507a743079706d5a496c576e566b31692d64577337722d7432427a7a494c324b6362795573386d4272375777645a4c49344868634b5a7553564941337438)

[![](https://camo.githubusercontent.com/06b0eb632e19787c14c9fac957de53125e9980997a718d71133d09b702c9d111/68747470733a2f2f6c68362e676f6f676c6575736572636f6e74656e742e636f6d2f7a44476e52627249486d6a37786f4433453343705f39694b386179717349303959436a7130327870655749766a4a4531374f7368766575496d34706a4538536a6851383162774a69456a43534e6778467753696f375348594535704469774d6e77576e6b6d4c766b3762674975647255757772557873626964333844386d6a584e68576669643747)](https://camo.githubusercontent.com/06b0eb632e19787c14c9fac957de53125e9980997a718d71133d09b702c9d111/68747470733a2f2f6c68362e676f6f676c6575736572636f6e74656e742e636f6d2f7a44476e52627249486d6a37786f4433453343705f39694b386179717349303959436a7130327870655749766a4a4531374f7368766575496d34706a4538536a6851383162774a69456a43534e6778467753696f375348594535704469774d6e77576e6b6d4c766b3762674975647255757772557873626964333844386d6a584e68576669643747)

[![](https://camo.githubusercontent.com/b65e9713283c3eadf6c441b469ba4e51b6822a9f64a48ba2907b8bdc95ba6cb7/68747470733a2f2f6c68362e676f6f676c6575736572636f6e74656e742e636f6d2f6571614c5949425f31596f2d53784d5f59556375566f73383654527a5736792d6d346c726267735a76425f34507356675359456f384263625f50467545536163696579515f323839414a5f516a524346727231684674376b597436466c75566f4c7550375541685030685357642d39366f737a5341303575704f52466f3370306758443246355047)](https://camo.githubusercontent.com/b65e9713283c3eadf6c441b469ba4e51b6822a9f64a48ba2907b8bdc95ba6cb7/68747470733a2f2f6c68362e676f6f676c6575736572636f6e74656e742e636f6d2f6571614c5949425f31596f2d53784d5f59556375566f73383654527a5736792d6d346c726267735a76425f34507356675359456f384263625f50467545536163696579515f323839414a5f516a524346727231684674376b597436466c75566f4c7550375541685030685357642d39366f737a5341303575704f52466f3370306758443246355047)

[![](https://camo.githubusercontent.com/629458743085d6ee4c698072432390102229cbe3c58555204e453d775fa4bf4a/68747470733a2f2f6c68362e676f6f676c6575736572636f6e74656e742e636f6d2f77575f58595664454e797768395a4b55795f596656376941316c67685f646c30317455584c4451763267684958716c334e5a784a727761626e6e43345a486e6e374f305f326430657370437857765a6e51532d58657370586f463549705141566c726e4b76686d46487750664d52466b75414c61715271734248434e77365268655a2d4d31684150)](https://camo.githubusercontent.com/629458743085d6ee4c698072432390102229cbe3c58555204e453d775fa4bf4a/68747470733a2f2f6c68362e676f6f676c6575736572636f6e74656e742e636f6d2f77575f58595664454e797768395a4b55795f596656376941316c67685f646c30317455584c4451763267684958716c334e5a784a727761626e6e43345a486e6e374f305f326430657370437857765a6e51532d58657370586f463549705141566c726e4b76686d46487750664d52466b75414c61715271734248434e77365268655a2d4d31684150)

-   Configure Webhook in GitHub and set webhook to trigger ansible build.

[](https://github.com/BimsDevs/ansible-config-mgt/blob/master/Ansible%20Configuration%20Management.md#enable-webhooks-in-github-repository-settings)Enable webhooks in GitHub repository settings.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

-   Go into the tooling github repository:

<https://github.com/BimsDevs/tooling>

-   Go to webhook in settings and add webhook:

-   Go to the Payload box and input the URL: http://jenkins_server_public_ip _address.github-webhook/

<http://35.178.239.175:8080/github-webhook/>

[![](https://camo.githubusercontent.com/9032068af68176548d88b0494420e96e47bdd6dbb3bb4ea86bc57798d75a4879/68747470733a2f2f6c68342e676f6f676c6575736572636f6e74656e742e636f6d2f5f614578355854644168586a706a376a5054505f377263396f4e4a5a504d4149704b494648364e314442486c6c5637692d426b7072583078684142477056307644536d64425837746c766c6243337a4864334b6c464c5f32375465705546416d393961535476727075716865667077767a5046654536377548415f69526b794a335450475656464e)](https://camo.githubusercontent.com/9032068af68176548d88b0494420e96e47bdd6dbb3bb4ea86bc57798d75a4879/68747470733a2f2f6c68342e676f6f676c6575736572636f6e74656e742e636f6d2f5f614578355854644168586a706a376a5054505f377263396f4e4a5a504d4149704b494648364e314442486c6c5637692d426b7072583078684142477056307644536d64425837746c766c6243337a4864334b6c464c5f32375465705546416d393961535476727075716865667077767a5046654536377548415f69526b794a335450475656464e)

-   Configure a Post-build job to save all (**) files, like you did it in Project 9.

[![](https://camo.githubusercontent.com/0bab408b46234c0d0d4d09e325aa48f5d11039df3969a0e62a638abff3bc5765/68747470733a2f2f6c68352e676f6f676c6575736572636f6e74656e742e636f6d2f7271646b617436713164716e53374765384e766c314d584a414e4639444e57685f654b386b6b4f57777a5f4f69547334613366574956727465585242357a2d597a4c56366c734f554e30554e4742457550396d41386d39676d7834455a64313355687069784757584b643143344a4c686b5143513541574a4d4b566b794479324141547654453649)](https://camo.githubusercontent.com/0bab408b46234c0d0d4d09e325aa48f5d11039df3969a0e62a638abff3bc5765/68747470733a2f2f6c68352e676f6f676c6575736572636f6e74656e742e636f6d2f7271646b617436713164716e53374765384e766c314d584a414e4639444e57685f654b386b6b4f57777a5f4f69547334613366574956727465585242357a2d597a4c56366c734f554e30554e4742457550396d41386d39676d7834455a64313355687069784757584b643143344a4c686b5143513541574a4d4b566b794479324141547654453649)

[](https://github.com/BimsDevs/ansible-config-mgt/blob/master/Ansible%20Configuration%20Management.md#blocker)Blocker:

----------------------------------------------------------------------------------------------------------------------

Unable to successfully build the job.

 Solution: Updated Jenkins URL with the new public IP address under Jenkins

              Location. Also ensured that the file in Github was under the Master's record.

[![](https://camo.githubusercontent.com/3702a535029afcdb119dbca1da372c744ec01f23c614eea815dfd5472347d056/68747470733a2f2f6c68342e676f6f676c6575736572636f6e74656e742e636f6d2f43626446596d38336c4f707157507857786f50675a4c4443784c4177505f57753932515a767032776f6c306f6f5131504b4f4872756a73312d45684c4248756c576a736e367363575f43504265527745776a56525a476d37514c67784255536b464a7550496c4858624a4e704a4e366362554c473450356e393549613551695857474c4f4a737545)](https://camo.githubusercontent.com/3702a535029afcdb119dbca1da372c744ec01f23c614eea815dfd5472347d056/68747470733a2f2f6c68342e676f6f676c6575736572636f6e74656e742e636f6d2f43626446596d38336c4f707157507857786f50675a4c4443784c4177505f57753932515a767032776f6c306f6f5131504b4f4872756a73312d45684c4248756c576a736e367363575f43504265527745776a56525a476d37514c67784255536b464a7550496c4858624a4e704a4e366362554c473450356e393549613551695857474c4f4a737545)

Test your setup by making some change in README.MD file in master branch and make sure that builds starts automatically and Jenkins saves the files (build artifacts) in following folder

ls /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/

[![](https://camo.githubusercontent.com/57ca138440f663744dc1060652f95f7cd4471181fe3df73d295afb08f1723012/68747470733a2f2f6c68362e676f6f676c6575736572636f6e74656e742e636f6d2f4c4677647847716d623042476f5f3973397447306571764e5776734a49486965795030377367506368343859316734574e6857453955424b484d7a64666c6a636d314b6566706e524f3630564b7545514c515f32565434384769556d78425565647277525f6d465378416c6f527357345553694550546a49776f346a2d4c466f79696e616f4b6431)](https://camo.githubusercontent.com/57ca138440f663744dc1060652f95f7cd4471181fe3df73d295afb08f1723012/68747470733a2f2f6c68362e676f6f676c6575736572636f6e74656e742e636f6d2f4c4677647847716d623042476f5f3973397447306571764e5776734a49486965795030377367506368343859316734574e6857453955424b484d7a64666c6a636d314b6566706e524f3630564b7545514c515f32565434384769556d78425565647277525f6d465378416c6f527357345553694550546a49776f346a2d4c466f79696e616f4b6431)

[![](https://camo.githubusercontent.com/7b5aa56c87e2c832df9a95498a2b723e4445862588769725fe468733e358a865/68747470733a2f2f6c68342e676f6f676c6575736572636f6e74656e742e636f6d2f7a3938304c37616d6674534e732d46783538665a722d6b346f34684549707542614559576f6e4e77576e4b7a316265384f657272547a6970324f662d6a454a5077486375584c4f7a5f757041426f306a35303553516f6642456b554977544c4d455a6c5f76416b43304e7038416b466674556741575f7575474e41754b6367453850304d496a6d76)](https://camo.githubusercontent.com/7b5aa56c87e2c832df9a95498a2b723e4445862588769725fe468733e358a865/68747470733a2f2f6c68342e676f6f676c6575736572636f6e74656e742e636f6d2f7a3938304c37616d6674534e732d46783538665a722d6b346f34684549707542614559576f6e4e77576e4b7a316265384f657272547a6970324f662d6a454a5077486375584c4f7a5f757041426f306a35303553516f6642456b554977544c4d455a6c5f76416b43304e7038416b466674556741575f7575474e41754b6367453850304d496a6d76)

[![](https://camo.githubusercontent.com/892eac528f5ee4d74dc077818784c53dc906c23f4f045233e121180633e16438/68747470733a2f2f6c68352e676f6f676c6575736572636f6e74656e742e636f6d2f502d44737454756c7a5450486e4e6832316e57734b50616e537959747637656c714b317a5a41475675344b565a446a715855576f4655677270773347615935756b4f35675a61653369592d4a6a573141514c6945715034774478393336667a666f68376a34355976673752467648356845356b71666e384165754c2d434f69654c442d697235524e)](https://camo.githubusercontent.com/892eac528f5ee4d74dc077818784c53dc906c23f4f045233e121180633e16438/68747470733a2f2f6c68352e676f6f676c6575736572636f6e74656e742e636f6d2f502d44737454756c7a5450486e4e6832316e57734b50616e537959747637656c714b317a5a41475675344b565a446a715855576f4655677270773347615935756b4f35675a61653369592d4a6a573141514c6945715034774478393336667a666f68376a34355976673752467648356845356b71666e384165754c2d434f69654c442d697235524e)

Note: Trigger Jenkins project execution only for /main (master) branch.

Now your setup will look like this:

[![](https://camo.githubusercontent.com/d7f9206f1fa9a60bd8c56a806c233a9999b79f6635419253d4973c77647af513/68747470733a2f2f6c68332e676f6f676c6575736572636f6e74656e742e636f6d2f376f373253692d31547850424d4c344852436b366e705f35507455796433666f7a586433476a3871534b306f507266776a4f63717767796732715a4479364b70506a48536330417456636e463565565563373733695f3249385958783474336678673844475657495766746f54314d6773436e732d4b416a54704e6c503357366768563679564956)](https://camo.githubusercontent.com/d7f9206f1fa9a60bd8c56a806c233a9999b79f6635419253d4973c77647af513/68747470733a2f2f6c68332e676f6f676c6575736572636f6e74656e742e636f6d2f376f373253692d31547850424d4c344852436b366e705f35507455796433666f7a586433476a3871534b306f507266776a4f63717767796732715a4479364b70506a48536330417456636e463565565563373733695f3249385958783474336678673844475657495766746f54314d6773436e732d4b416a54704e6c503357366768563679564956)

[](https://github.com/BimsDevs/ansible-config-mgt/blob/master/Ansible%20Configuration%20Management.md#allocate-elastic-ip-address-to-jenkins---ansible-server)Allocate Elastic IP address to Jenkins - Ansible server

---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Tip Every time you stop/start your Jenkins-Ansible server - you have to reconfigure GitHub webhook to a new IP address, in order to avoid it, it makes sense to allocate an [Elastic IP](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html) to your Jenkins-Ansible server (you have done it before to your LB server in [Project 10](https://professional-pbl.darey.io/en/latest/project10.html)). Note that Elastic IP is free only when it is being allocated to an EC2 Instance, so do not forget to release Elastic IP once you terminate your EC2 Instance.

Steps:

1\.  Open the Amazon EC2 console at <https://console.aws.amazon.com/ec2/>.

2\.  In the navigation pane, choose Network & Security, Elastic IPs.

3\.  Choose Allocate Elastic IP address.

4\.  I have chosen Amazon's pool of IPv4 addresses, since the IPv4 address is to be allocated from Amazon's pool of IPv4 addresses.

5\.  (Optional) Add or remove a tag.

6\.  Choose Allocate.

7\.  Click Allocate new address in the Elastic IPs page.

8\.  Click on the row of the newly created elastic IP, select Actions and click on Associate elastic address.

9\.  Choose the EC2 instance you are integrating.

10\. Click on Associate

[![](https://camo.githubusercontent.com/a8d6c01d80027d080b21038f8fb2236236a8dc7832c3b903a22dad8bd2682a10/68747470733a2f2f6c68332e676f6f676c6575736572636f6e74656e742e636f6d2f3953386b697166637654373861724f4161374b6b39595578564d4f425453586274306d476244535f3736486a326c4c784f6b6c7976674a706b5157623864442d75796475714c4a4d725f64574c4155763369646f7547626b336a4270717253634a494c7646726147697944706c6539587539784e7a7669684877476e78674531597a4e7676625239)](https://camo.githubusercontent.com/a8d6c01d80027d080b21038f8fb2236236a8dc7832c3b903a22dad8bd2682a10/68747470733a2f2f6c68332e676f6f676c6575736572636f6e74656e742e636f6d2f3953386b697166637654373861724f4161374b6b39595578564d4f425453586274306d476244535f3736486a326c4c784f6b6c7976674a706b5157623864442d75796475714c4a4d725f64574c4155763369646f7547626b336a4270717253634a494c7646726147697944706c6539587539784e7a7669684877476e78674531597a4e7676625239)

[](https://github.com/BimsDevs/ansible-config-mgt/blob/master/Ansible%20Configuration%20Management.md#elastic-ip-address-1813221941)Elastic IP address: [18.132.219.41](https://eu-west-2.console.aws.amazon.com/ec2/v2/home?region=eu-west-2#ElasticIpDetails:AllocationId=eipalloc-047271799d471a966)

=======================================================================================================================================================================================================================================================================================================

[](https://github.com/BimsDevs/ansible-config-mgt/blob/master/Ansible%20Configuration%20Management.md#step-2---prepare-the-development-environment-using-visual-studio-code)Step 2 - Prepare the development environment using Visual Studio Code

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Downloaded Visual Studio code as the [Integrated development environment (IDE)](https://en.wikipedia.org/wiki/Integrated_development_environment) or [Source-code Editor](https://en.wikipedia.org/wiki/Source-code_editor) that can be used to write some codes. Configured it to connect to Github with the steps below:

Steps to link Visual Studio Code to Github (<https://code.visualstudio.com/docs/editor/github>)

-   Open the remote window

-   On the bar space, select remote SSH - Open configuration file

-   Select file path to configuration file

-   Configure the host, host name, user and identity file

-   Ctrl+S to save. Then launch new terminal

[![](https://camo.githubusercontent.com/0e12c7b9f6ace81d4c7d9b95f9a903d7bd3fb41f69470c1a7c5a08c9a128c220/68747470733a2f2f6c68332e676f6f676c6575736572636f6e74656e742e636f6d2f4b4a6356647a427465615343714c47797679566e463646657662576545386b6b56666a493157313454483655346839664f4c397744364f4648454d5f41484f5f664b7574696a426d6e3259326c6e713757455761522d7253715a72715f7135416a6c626a67393364716952736263644867306939513932612d3770705a734f4d554f4e4b326c5073)](https://camo.githubusercontent.com/0e12c7b9f6ace81d4c7d9b95f9a903d7bd3fb41f69470c1a7c5a08c9a128c220/68747470733a2f2f6c68332e676f6f676c6575736572636f6e74656e742e636f6d2f4b4a6356647a427465615343714c47797679566e463646657662576545386b6b56666a493157313454483655346839664f4c397744364f4648454d5f41484f5f664b7574696a426d6e3259326c6e713757455761522d7253715a72715f7135416a6c626a67393364716952736263644867306939513932612d3770705a734f4d554f4e4b326c5073)

[![](https://camo.githubusercontent.com/bd79f1ed692fb35e2e7d823b379bd10213228ebc68dccfadb6a0c254503bc204/68747470733a2f2f6c68352e676f6f676c6575736572636f6e74656e742e636f6d2f5a7153342d4d6f434273395070783133455950534f364c7948616b6f76666a766449772d6b59584b6f3759566267396d4549443566343839466b457838307448347a466e616455774b5462474342354b4865775535673078522d76657575346c4d4f646663775a6e5f4463337337494c5652454156474453397075386c3938546355787264775439)](https://camo.githubusercontent.com/bd79f1ed692fb35e2e7d823b379bd10213228ebc68dccfadb6a0c254503bc204/68747470733a2f2f6c68352e676f6f676c6575736572636f6e74656e742e636f6d2f5a7153342d4d6f434273395070783133455950534f364c7948616b6f76666a766449772d6b59584b6f3759566267396d4549443566343839466b457838307448347a466e616455774b5462474342354b4865775535673078522d76657575346c4d4f646663775a6e5f4463337337494c5652454156474453397075386c3938546355787264775439)

[![](https://camo.githubusercontent.com/eaaa90aeb37a9d3565835a23c4d97d799b1c596cae918626294b7fe37c2d225f/68747470733a2f2f6c68362e676f6f676c6575736572636f6e74656e742e636f6d2f6e44524a496356444130564272796d376c736230534a645f5475644a4942564269684b537877343065795958414a5a4b466a69496b614b73585754384a4a694a75586837724f6c71354a6c52334b5035504b6a6c7764454f357a46696746787261414d6952712d6d77557468747267765742553746475161515a345f5f4f415049447062704e4d39)](https://camo.githubusercontent.com/eaaa90aeb37a9d3565835a23c4d97d799b1c596cae918626294b7fe37c2d225f/68747470733a2f2f6c68362e676f6f676c6575736572636f6e74656e742e636f6d2f6e44524a496356444130564272796d376c736230534a645f5475644a4942564269684b537877343065795958414a5a4b466a69496b614b73585754384a4a694a75586837724f6c71354a6c52334b5035504b6a6c7764454f357a46696746787261414d6952712d6d77557468747267765742553746475161515a345f5f4f415049447062704e4d39)

Start by connecting the remote servers to the Ansible-Jenkins server:

To get started with ansible, you must configure the Ansible-Jenkins master controller to connect to your remote host servers, for best practice and security this is achieved using ssh.

All remote host servers would be stored in an inventory file with the below structure in different environments in this instance.

Note: Ansible uses TCP port 22 by default, which means it needs to ssh into target remote/slave node servers from the Jenkins-Ansible host.

Two ways are:

1\.  Copy your private (.pem) key and provide a path to it so Ansible could use it to connect. Do not forget to change permissions to your private key chmod 400 key.pem, otherwise EC2 will not accept the key.

2\.  Generate a new ssh key, then print the output of the public key by running the below command and copy the public key of the Ansible-Jenkins into the authorization key file of each of the remote servers

Ansible-Jenkins Server:

sudo ssh-keygen

sudo cat .ssh/id_rsa.pub

or

cd .ssh

sudo cat id_rsa.pub

Copy the public authorisation key file and paste in the authorisation key file of the connecting servers.

[![](https://camo.githubusercontent.com/66730298465b8cd8a5e069e7f80e0200573e330044dd04632cbdd6d4122c77e6/68747470733a2f2f6c68362e676f6f676c6575736572636f6e74656e742e636f6d2f5a62544e35536974427072635a7172655745786c48466569656d3148384e506d4a7769584c566f4e5456372d4e4e4e4d7a5f71436758534c5f41764f3456454674703467643841364970504837765a586333523562345975373069793748302d4b396f43427753576155303856564f3264797257676b6a6c69733932574c6a4a43474e5353555778)](https://camo.githubusercontent.com/66730298465b8cd8a5e069e7f80e0200573e330044dd04632cbdd6d4122c77e6/68747470733a2f2f6c68362e676f6f676c6575736572636f6e74656e742e636f6d2f5a62544e35536974427072635a7172655745786c48466569656d3148384e506d4a7769584c566f4e5456372d4e4e4e4d7a5f71436758534c5f41764f3456454674703467643841364970504837765a586333523562345975373069793748302d4b396f43427753576155303856564f3264797257676b6a6c69733932574c6a4a43474e5353555778)

[![](https://camo.githubusercontent.com/528581fcd9b9eb7907bbfcdae9329fa82ba8fb8c8f112015d36cc5ae13839185/68747470733a2f2f6c68362e676f6f676c6575736572636f6e74656e742e636f6d2f43442d766c68786d3146626331354a557733437073457457797677667a6767777575783556446a565f5a547138584d773270625269657775452d657174574b6c6866356d3176634a6a394b555075596f79644d7034596b76544f596745376e3674365f6b31346b2d386e334c726e3556353377624272525076495968674d7a31375f356c4742696c)](https://camo.githubusercontent.com/528581fcd9b9eb7907bbfcdae9329fa82ba8fb8c8f112015d36cc5ae13839185/68747470733a2f2f6c68362e676f6f676c6575736572636f6e74656e742e636f6d2f43442d766c68786d3146626331354a557733437073457457797677667a6767777575783556446a565f5a547138584d773270625269657775452d657174574b6c6866356d3176634a6a394b555075596f79644d7034596b76544f596745376e3674365f6b31346b2d386e334c726e3556353377624272525076495968674d7a31375f356c4742696c)

Connecting server and repeat for the other servers :  sudo vi .ssh/authorized_key

[![](https://camo.githubusercontent.com/8ed09bf9f9af86d33fe2c6db2aed2ba724e55bb206bca096416dfc9cee2f6a74/68747470733a2f2f6c68332e676f6f676c6575736572636f6e74656e742e636f6d2f586361644b63736138504e526a627a654d4561496e30435446384672416d5542645971476851623245365f776b31794b4b4d613864574c365f496866766b343062684a4a6d7756695f6a4b786c59506f63356a725a723730467034746669634d685653425f596656456c725f6751746e5a33655354574f634b4945522d6f75536679363967757a46)](https://camo.githubusercontent.com/8ed09bf9f9af86d33fe2c6db2aed2ba724e55bb206bca096416dfc9cee2f6a74/68747470733a2f2f6c68332e676f6f676c6575736572636f6e74656e742e636f6d2f586361644b63736138504e526a627a654d4561496e30435446384672416d5542645971476851623245365f776b31794b4b4d613864574c365f496866766b343062684a4a6d7756695f6a4b786c59506f63356a725a723730467034746669634d685653425f596656456c725f6751746e5a33655354574f634b4945522d6f75536679363967757a46)

[](https://github.com/BimsDevs/ansible-config-mgt/blob/master/Ansible%20Configuration%20Management.md#step-3---begin-ansible-development)Step 3 - Begin Ansible Development

---------------------------------------------------------------------------------------------------------------------------------------------------------------------------

1\.  In your ansible-config-mgt GitHub repository, a new branch was created. This will be used for the development of a new feature. (ansible-5511-feature01)

[![](https://camo.githubusercontent.com/de12ca5eb6d33ce92b1c5448811053b3251d45e5e80e7bd5c18fc522428d6bb4/68747470733a2f2f6c68352e676f6f676c6575736572636f6e74656e742e636f6d2f7a784e7a4857577a65683332694539335949637a384a7977425577436973554f5863657a727066746f6d476843534438757368456e694671757344516b337979486374666732325175526e4c48414950796f396f38315177586b70712d50314c64514e41745654316d367262507535687273304c366171446775374642394838307459784f6f712d)](https://camo.githubusercontent.com/de12ca5eb6d33ce92b1c5448811053b3251d45e5e80e7bd5c18fc522428d6bb4/68747470733a2f2f6c68352e676f6f676c6575736572636f6e74656e742e636f6d2f7a784e7a4857577a65683332694539335949637a384a7977425577436973554f5863657a727066746f6d476843534438757368456e694671757344516b337979486374666732325175526e4c48414950796f396f38315177586b70712d50314c64514e41745654316d367262507535687273304c366171446775374642394838307459784f6f712d)

 In the VS Code Integrated Terminal:

1\.  Create a folder and name it ansible: mkdir ansible

2\.  cd  into ansible: cd ansible

3\.  Clone your repository into the server:

git clone <https://github.com/BimsDevs/ansible-config-mgt.git>

[![](https://camo.githubusercontent.com/53092be6b459f5f9780be6596029398cfae9cb2d7c5d4e517a24a818861e0ac8/68747470733a2f2f6c68352e676f6f676c6575736572636f6e74656e742e636f6d2f763142534357385830543750586d78687966394f5a6964777845307441646b75723256334e783737625255457535545377585649754168395f4b62507356663435586d6637472d4446376a4d61786f6458334f53564c3632686671636b6f4b46724d676c637676705874756b6c744d595654467344664b7a654f583036746f71424164745a36696a)](https://camo.githubusercontent.com/53092be6b459f5f9780be6596029398cfae9cb2d7c5d4e517a24a818861e0ac8/68747470733a2f2f6c68352e676f6f676c6575736572636f6e74656e742e636f6d2f763142534357385830543750586d78687966394f5a6964777845307441646b75723256334e783737625255457535545377585649754168395f4b62507356663435586d6637472d4446376a4d61786f6458334f53564c3632686671636b6f4b46724d676c637676705874756b6c744d595654467344664b7a654f583036746f71424164745a36696a)

1\.  Checkout the newly created feature branch to your local machine and start building your code and directory structure

git checkout ansible-5511-feature01

[![](https://camo.githubusercontent.com/d8a54b89984e51af4149ceb93552f191c45b96e479e9948b4878d7c41129868d/68747470733a2f2f6c68342e676f6f676c6575736572636f6e74656e742e636f6d2f4250734d61373258526a71634b736a336d736c325733452d6b396f573269646f5170333246417574424c644976444731686538304c4a75386b526a36325a56706c72695166617973666e4378794d4b52324e4a347641476e7a64766f67715032694364316c4e2d3134456e4d5364564957616d336c4f5a4e364b6c7a41654674356f664a51685066)](https://camo.githubusercontent.com/d8a54b89984e51af4149ceb93552f191c45b96e479e9948b4878d7c41129868d/68747470733a2f2f6c68342e676f6f676c6575736572636f6e74656e742e636f6d2f4250734d61373258526a71634b736a336d736c325733452d6b396f573269646f5170333246417574424c644976444731686538304c4a75386b526a36325a56706c72695166617973666e4378794d4b52324e4a347641476e7a64766f67715032694364316c4e2d3134456e4d5364564957616d336c4f5a4e364b6c7a41654674356f664a51685066)

1\.  Create a directory and name it playbooks - it will be used to store all the playbook files.

mkdir playbooks

1\.  Within the playbooks folder, create your first playbook, and name it common.yml

cd playbooks

touch common.yml

1\.  Create a directory and name it inventory - it will be used to keep the hosts organised.

mkdir inventory

1\.  Within the inventory folder, create an inventory file (.yml) for each environment (Development, Staging Testing and Production) dev, staging, uat, and prod respectively.

cd inventory

touch dev.yml staging.yml uat.yml prod.yml

[![](https://camo.githubusercontent.com/7362149878774e80a78c1024fa677d5eaa4520bc536f486d17ec75cf3539077c/68747470733a2f2f6c68352e676f6f676c6575736572636f6e74656e742e636f6d2f424578476d4343546c324d483577666e50376f576d517a614943324f3961316a4f55565a74774f2d695534795a6b4652365f64524a394b5945464a566d5952757a30587931542d696c4e3045595653537174647435356e6e7577525350434251464f744d35783464786a755f434648644669323364392d4552796965706a726d2d55547135795756)](https://camo.githubusercontent.com/7362149878774e80a78c1024fa677d5eaa4520bc536f486d17ec75cf3539077c/68747470733a2f2f6c68352e676f6f676c6575736572636f6e74656e742e636f6d2f424578476d4343546c324d483577666e50376f576d517a614943324f3961316a4f55565a74774f2d695534795a6b4652365f64524a394b5945464a566d5952757a30587931542d696c4e3045595653537174647435356e6e7577525350434251464f744d35783464786a755f434648644669323364392d4552796965706a726d2d55547135795756)

### [](https://github.com/BimsDevs/ansible-config-mgt/blob/master/Ansible%20Configuration%20Management.md#step-4---set-up-an-ansible-inventory)Step 4 - Set up an Ansible Inventory

An Ansible inventory file defines the hosts and groups of hosts upon which commands, modules, and tasks in a playbook operate. Since our intention is to execute Linux commands on remote hosts, and ensure that it is the intended configuration on a particular server that occurs. It is important to have a way to organize our hosts in such an Inventory.

Save below inventory structure in the inventory/dev file to start configuring your development servers. Ensure to replace the IP addresses according to your own setup.

Note: Ansible uses TCP port 22 by default, which means it needs to ssh into target servers from Jenkins-Ansible host - for this you need to copy your private (.pem) key and provide a path to it so Ansible could use it to connect. Do not forget to change permissions to your private key chmod 400 key.pem, otherwise EC2 will not accept the key. Also notice, that your Load Balancer user is ubuntu and user for RHEL-based servers is ec2-user.

[](https://github.com/BimsDevs/ansible-config-mgt/blob/master/Ansible%20Configuration%20Management.md#ip-addresses-private)IP Addresses (private)

-------------------------------------------------------------------------------------------------------------------------------------------------

NFS : 172.31.7.178

Webserver 1: 172.31.16.162

Webserver 2: 172.31.16.41

Database: 172.31.18.63

Load balancer: 172.31.46.227

[](https://github.com/BimsDevs/ansible-config-mgt/blob/master/Ansible%20Configuration%20Management.md#update-the-inventorydevyml-file-with-this-snippet-of-code-and-save-ctrls)Update the inventory/dev.yml file with this snippet of code and save (ctrl+s):

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

[nfs]

ansible_ssh_user='ec2-user' ansible_ssh_private_key_file=<path-to-.pem-private-key>

[webservers]

ansible_ssh_user='ec2-user' ansible_ssh_private_key_file=<path-to-.pem-private-key>

ansible_ssh_user='ec2-user' ansible_ssh_private_key_file=<path-to-.pem-private-key>

[db]

ansible_ssh_user='ec2-user' ansible_ssh_private_key_file=<path-to-.pem-private-key>

[lb]

ansible_ssh_user='ubuntu' ansible_ssh_private_key_file=<path-to-.pem-private-key>

[](https://github.com/BimsDevs/ansible-config-mgt/blob/master/Ansible%20Configuration%20Management.md#updated-version)Updated version

-------------------------------------------------------------------------------------------------------------------------------------

[nfs]

172.31.7.178 ansible_ssh_user='ec2-user'

[webservers]

172.31.16.162 ansible_ssh_user='ec2-user'

172.31.16.41 ansible_ssh_user='ec2-user'

[db]

172.31.18.63 ansible_ssh_user='ubuntu'

[lb]

172.31.46.227 ansible_ssh_user='ubuntu'

[](https://github.com/BimsDevs/ansible-config-mgt/blob/master/Ansible%20Configuration%20Management.md#output)Output

-------------------------------------------------------------------------------------------------------------------

[![](https://camo.githubusercontent.com/6bb6aa5ddc9f48b4c61b6e6610190e5edfa5d01387f7d1fb2699f90677d176e7/68747470733a2f2f6c68342e676f6f676c6575736572636f6e74656e742e636f6d2f5235495f366633636d4b675f46384d5732386561454e68445f39304e777a4f4f57593663636c4f54343876774c4d6d3358615f6567526e426e443873756d32456a727974374f30514b4e66776b526977346c4977532d3931785f4b4a533131526c6c4a7977724745585143646e55664e4c6b6c33386241674c2d75586c34316a3039643779774c45)](https://camo.githubusercontent.com/6bb6aa5ddc9f48b4c61b6e6610190e5edfa5d01387f7d1fb2699f90677d176e7/68747470733a2f2f6c68342e676f6f676c6575736572636f6e74656e742e636f6d2f5235495f366633636d4b675f46384d5732386561454e68445f39304e777a4f4f57593663636c4f54343876774c4d6d3358615f6567526e426e443873756d32456a727974374f30514b4e66776b526977346c4977532d3931785f4b4a533131526c6c4a7977724745585143646e55664e4c6b6c33386241674c2d75586c34316a3039643779774c45)

[![](https://camo.githubusercontent.com/53af6db7ea814bc9686359ee6f3456e306d4392a6de717808c266fae174e32f6/68747470733a2f2f6c68332e676f6f676c6575736572636f6e74656e742e636f6d2f6935392d52734168504453366330374e71346a4862326842375f6e396a47714b78717569636a65797a6f512d68383969527559686c6b4f613671356574325175524565546e67707366784d6d766e71393437425255674f555a49706246586767313969715230436a7a5050366d54516a597231454b3844346945306c6658793679756a472d4a744a)](https://camo.githubusercontent.com/53af6db7ea814bc9686359ee6f3456e306d4392a6de717808c266fae174e32f6/68747470733a2f2f6c68332e676f6f676c6575736572636f6e74656e742e636f6d2f6935392d52734168504453366330374e71346a4862326842375f6e396a47714b78717569636a65797a6f512d68383969527559686c6b4f613671356574325175524565546e67707366784d6d766e71393437425255674f555a49706246586767313969715230436a7a5050366d54516a597231454b3844346945306c6658793679756a472d4a744a)

### [](https://github.com/BimsDevs/ansible-config-mgt/blob/master/Ansible%20Configuration%20Management.md#step-5---create-a-common-playbook)Step 5 - Create a Common Playbook

It is time to start giving Ansible the instructions on what needs to be performed on all servers listed in inventory/dev.

In the common.yml playbook you will write configuration for repeatable, re-usable, and multi-machine tasks that is common to systems within the infrastructure.

[](https://github.com/BimsDevs/ansible-config-mgt/blob/master/Ansible%20Configuration%20Management.md#update-your-playbookscommonyml-file-with-following-code)Update your playbooks/common.yml file with following code:

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

* * * * *

-   name: update web, nfs and db servers

  hosts: webservers, nfs, db

  remote_user: ec2-user

  become: yes

  become_user: root

  tasks:

  - name: ensure wireshark is at the latest version

    yum:

      name: wireshark

      state: latest

-   name: update LB server

  hosts: lb

  remote_user: ubuntu

  become: yes

  become_user: root

  tasks:

  - name: ensure wireshark is at the latest version

    apt:

      name: wireshark

      state: latest

[![](https://camo.githubusercontent.com/12aabb26dd87ba1c5815d0469f10f36bff8b367efa2e547f94a297782f73f4c1/68747470733a2f2f6c68332e676f6f676c6575736572636f6e74656e742e636f6d2f494562676f5f7575326f64576c32574c4458646c4d72795a5734696f465a3150613774696b6344496b414a695f3850434346327351676c6736697a534d6171656379724e2d5f39304c65473546596c746b777336694a3268756c3441494672644167374c6b5f4a396e4a716f4a344663667276746470524c6558464e36323947497370694d4e5449)](https://camo.githubusercontent.com/12aabb26dd87ba1c5815d0469f10f36bff8b367efa2e547f94a297782f73f4c1/68747470733a2f2f6c68332e676f6f676c6575736572636f6e74656e742e636f6d2f494562676f5f7575326f64576c32574c4458646c4d72795a5734696f465a3150613774696b6344496b414a695f3850434346327351676c6736697a534d6171656379724e2d5f39304c65473546596c746b777336694a3268756c3441494672644167374c6b5f4a396e4a716f4a344663667276746470524c6558464e36323947497370694d4e5449)

[![](https://camo.githubusercontent.com/5a7974117433997dc57926a9dc77d992eee673eab779f72494943873e1395795/68747470733a2f2f6c68352e676f6f676c6575736572636f6e74656e742e636f6d2f57594a477959654f6a6639507458737937503741426f436958796c427a52324b355842344f51716647794d763343354649485a714d384757615579504d437448636b354b42555f62346338797958396e4143365f78774f6b6a70697547412d5157455f61466662386d5f767142516d6333764957477678546f68483455473539794865776a735230)](https://camo.githubusercontent.com/5a7974117433997dc57926a9dc77d992eee673eab779f72494943873e1395795/68747470733a2f2f6c68352e676f6f676c6575736572636f6e74656e742e636f6d2f57594a477959654f6a6639507458737937503741426f436958796c427a52324b355842344f51716647794d763343354649485a714d384757615579504d437448636b354b42555f62346338797958396e4143365f78774f6b6a70697547412d5157455f61466662386d5f767142516d6333764957477678546f68483455473539794865776a735230)

Examine the code above and try to make sense out of it. This playbook is divided into two parts, each of them is intended to perform the same task: install [wireshark](https://en.wikipedia.org/wiki/Wireshark) utility (or make sure it is updated to the latest version) on your RHEL 8 and Ubuntu servers. It uses root user to perform this task and respective package manager: yum for RHEL 8 and apt for Ubuntu.

Run ansible:

ansible-playbook -i inventory/dev.yml playbooks/common.yml

[![](https://camo.githubusercontent.com/c8dac85e889d7818b29490886de5dc03bda34c62b061fd1924df687f9eae9af1/68747470733a2f2f6c68362e676f6f676c6575736572636f6e74656e742e636f6d2f53385f6169656235394e387575484c5f706255395475797a6a314f6d5f326c5832732d375a4975795175616f316a74373241456d30775546766153724f3442545f304a5750537877374f41447532566f5f4a6c64717933556861765955517364793173694b344e737838636662494a306147666c307577546c54466f684e3075696c667649486b6b)](https://camo.githubusercontent.com/c8dac85e889d7818b29490886de5dc03bda34c62b061fd1924df687f9eae9af1/68747470733a2f2f6c68362e676f6f676c6575736572636f6e74656e742e636f6d2f53385f6169656235394e387575484c5f706255395475797a6a314f6d5f326c5832732d375a4975795175616f316a74373241456d30775546766153724f3442545f304a5750537877374f41447532566f5f4a6c64717933556861765955517364793173694b344e737838636662494a306147666c307577546c54466f684e3075696c667649486b6b)

Feel free to update this playbook with following tasks:

-   Create a directory and a file inside it

-   Change timezone on all servers

[](https://github.com/BimsDevs/ansible-config-mgt/blob/master/Ansible%20Configuration%20Management.md#sample-syntax)Sample syntax:

==================================================================================================================================

-   name: Set timezone to Asia/Tokyo

Hosts: ubuntu

  timezone:

    name: Asia/Tokyo

-   Run shell Script etc. ..

-   hosts: all

  tasks:

  - name: Creating a new directory and set time zone with a simple script

    file:

      path: "/your path"

      state: directory

[](https://github.com/BimsDevs/ansible-config-mgt/blob/master/Ansible%20Configuration%20Management.md#updated-version-1)Updated version:

----------------------------------------------------------------------------------------------------------------------------------------

-   name: Creating a directory and set time zone with a simple script

  hosts: all

  become: true

  tasks:

  - name: Creating a directory

    file:

      path: /playbooks/new-folder

      state: directory

-  name: Creating a new file inside the new directory

   file:

     path: /playbooks/new-folder/new-file

     state: touch

-   name: Set timezone to Europe/London

   timezone:

    name: Europe/London

-   name: Status

  command: echo "All tasks completed"

[![](https://camo.githubusercontent.com/4e58460266cd86e992929a36aec9b406bd0ffbcb40551ddb5c0b5d6c41f784e5/68747470733a2f2f6c68332e676f6f676c6575736572636f6e74656e742e636f6d2f7569367148324e726c704368504e69586b372d7a3775377843443152667647416e384d2d723664455845787471515970386257564f6b6e44567254533054324d7a70644d6c74617463705a567978333871797a32734a655a6f6a4c71553035437a734f4d3242355845586a367764594f32657476696f3352644a4e68463145326e425a4137426951)](https://camo.githubusercontent.com/4e58460266cd86e992929a36aec9b406bd0ffbcb40551ddb5c0b5d6c41f784e5/68747470733a2f2f6c68332e676f6f676c6575736572636f6e74656e742e636f6d2f7569367148324e726c704368504e69586b372d7a3775377843443152667647416e384d2d723664455845787471515970386257564f6b6e44567254533054324d7a70644d6c74617463705a567978333871797a32734a655a6f6a4c71553035437a734f4d3242355845586a367764594f32657476696f3352644a4e68463145326e425a4137426951)

[![](https://camo.githubusercontent.com/09b5358ecbdf0336b042f352c2e6c4925e506d2df241d275198f37c1988b082a/68747470733a2f2f6c68352e676f6f676c6575736572636f6e74656e742e636f6d2f6934546a414a6a3670674d61426f5175366b7a546669396f5a2d2d5136567063515356635639513237395a5a5f335f6d345a7955386d6c6c354a4f354744495256696c756747486462435f6d426a326253396d776b76476a547164334a5f78357433584d5650513231536554796f63346c75565f3463514e456d48657a70554f726d586339727776)](https://camo.githubusercontent.com/09b5358ecbdf0336b042f352c2e6c4925e506d2df241d275198f37c1988b082a/68747470733a2f2f6c68352e676f6f676c6575736572636f6e74656e742e636f6d2f6934546a414a6a3670674d61426f5175366b7a546669396f5a2d2d5136567063515356635639513237395a5a5f335f6d345a7955386d6c6c354a4f354744495256696c756747486462435f6d426a326253396d776b76476a547164334a5f78357433584d5650513231536554796f63346c75565f3463514e456d48657a70554f726d586339727776)

Overall Outputs:

[![](https://camo.githubusercontent.com/4703dd780bb1476257b58be4fa5111fcd83d21be956bd3434f8ddebd31bf6401/68747470733a2f2f6c68342e676f6f676c6575736572636f6e74656e742e636f6d2f4f6545734f626c35775230744e4a306d36645a654568746c41354b6858314d4a6a62454345355f5633384a5a4432385a7a62657a77533552396a624b79376a574a55536c33794c51567453585f6566794c59793434766347516b6a71517a367137633234474c6f6d742d467431645152506c65637769696a49642d334c667344334d457178633571)](https://camo.githubusercontent.com/4703dd780bb1476257b58be4fa5111fcd83d21be956bd3434f8ddebd31bf6401/68747470733a2f2f6c68342e676f6f676c6575736572636f6e74656e742e636f6d2f4f6545734f626c35775230744e4a306d36645a654568746c41354b6858314d4a6a62454345355f5633384a5a4432385a7a62657a77533552396a624b79376a574a55536c33794c51567453585f6566794c59793434766347516b6a71517a367137633234474c6f6d742d467431645152506c65637769696a49642d334c667344334d457178633571)

[![](https://camo.githubusercontent.com/e1d9545695b907bd0f2f66ace2553237e4e5a656d5cf07e8915305257e54f57c/68747470733a2f2f6c68342e676f6f676c6575736572636f6e74656e742e636f6d2f48546864664f42697956333256516e654a726175443679764a4c6257323678547435755567387264315a6f4c724e4633583741725552776f424f3372356e63655637375a6a6a44397948556730644f6f483330563772425250434a7265325f355f54745a63336c5f6e3839624c5770537772666545484832464333397178316845366969354a5563)](https://camo.githubusercontent.com/e1d9545695b907bd0f2f66ace2553237e4e5a656d5cf07e8915305257e54f57c/68747470733a2f2f6c68342e676f6f676c6575736572636f6e74656e742e636f6d2f48546864664f42697956333256516e654a726175443679764a4c6257323678547435755567387264315a6f4c724e4633583741725552776f424f3372356e63655637375a6a6a44397948556730644f6f483330563772425250434a7265325f355f54745a63336c5f6e3839624c5770537772666545484832464333397178316845366969354a5563)

[![](https://camo.githubusercontent.com/1ab32661535d1a4e0959413b03cd17d57aada2a93830a08768a61e6c7974c4b1/68747470733a2f2f6c68362e676f6f676c6575736572636f6e74656e742e636f6d2f6b2d684f315753556e5a41307a4c6b4b524c7733412d6751636259726b6d30637054454b6779305848542d413231315f47505a6667706433496e43346f747a55336b54436b3868666c3944677a6b326b79716e53356853675a754e4842524f692d50356564566b317a7352775f5958656d5f7263446e66357679446f777a56536c486a737661676a)](https://camo.githubusercontent.com/1ab32661535d1a4e0959413b03cd17d57aada2a93830a08768a61e6c7974c4b1/68747470733a2f2f6c68362e676f6f676c6575736572636f6e74656e742e636f6d2f6b2d684f315753556e5a41307a4c6b4b524c7733412d6751636259726b6d30637054454b6779305848542d413231315f47505a6667706433496e43346f747a55336b54436b3868666c3944677a6b326b79716e53356853675a754e4842524f692d50356564566b317a7352775f5958656d5f7263446e66357679446f777a56536c486a737661676a)

[![](https://camo.githubusercontent.com/091fb9fd190f238d7f19e2d71a2bf04b41ccfa9656e06c1a0d248d7e023a6a96/68747470733a2f2f6c68342e676f6f676c6575736572636f6e74656e742e636f6d2f4770362d31516f4e45676a4765623147436f72656f64793343705967716a58797077576f7174313570584b786c3238585754774f39714666787958734a3170397a62316c31325451315846325732416d5163345f393071694762774a30393463723941324a4f6474707842566f59443331765f527845494775706f79524a78334c75654c6a64536f)](https://camo.githubusercontent.com/091fb9fd190f238d7f19e2d71a2bf04b41ccfa9656e06c1a0d248d7e023a6a96/68747470733a2f2f6c68342e676f6f676c6575736572636f6e74656e742e636f6d2f4770362d31516f4e45676a4765623147436f72656f64793343705967716a58797077576f7174313570584b786c3238585754774f39714666787958734a3170397a62316c31325451315846325732416d5163345f393071694762774a30393463723941324a4f6474707842566f59443331765f527845494775706f79524a78334c75654c6a64536f)

[![](https://camo.githubusercontent.com/150b7aebb0ba57bb9faa6646eab2472f2c3dba199e54e1448484e437153e2ac0/68747470733a2f2f6c68352e676f6f676c6575736572636f6e74656e742e636f6d2f3879373451445f44775341484a526c39305a796542474530666833475976337174546e724a37304152423735586541656b7a514b545279646c793835674a53766e34497664795a494c3758723135336f72726e39494e456163304b764577746956737072557a74314d65464834524d62644b494f6668674739517a767464726e367a573243447071)](https://camo.githubusercontent.com/150b7aebb0ba57bb9faa6646eab2472f2c3dba199e54e1448484e437153e2ac0/68747470733a2f2f6c68352e676f6f676c6575736572636f6e74656e742e636f6d2f3879373451445f44775341484a526c39305a796542474530666833475976337174546e724a37304152423735586541656b7a514b545279646c793835674a53766e34497664795a494c3758723135336f72726e39494e456163304b764577746956737072557a74314d65464834524d62644b494f6668674739517a767464726e367a573243447071)

For a better understanding of Ansible playbooks - [watch this video from RedHat](https://youtu.be/ZAdJ7CdN7DY) and read [this article](https://www.redhat.com/en/topics/automation/what-is-an-ansible-playbook).

### [](https://github.com/BimsDevs/ansible-config-mgt/blob/master/Ansible%20Configuration%20Management.md#step-6---update-git-with-the-latest-code)Step 6 - Update GIT with the latest code

Now all of your directories and files live on your machine and you need to push changes made locally to GitHub.

In the real world, you will be working within a team of other DevOps engineers and developers. It is important to learn how to collaborate with the help of GIT. In many organisations there is a development rule that do not allow you to deploy any code before it has been reviewed by an extra pair of eyes - it is also called "Four eyes principle".

Now you have a separate branch, you will need to know how to raise a [Pull Request (PR)](https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests/about-pull-requests), get your branch peer reviewed and merged to the master branch.

Commit your code into GitHub:

[](https://github.com/BimsDevs/ansible-config-mgt/blob/master/Ansible%20Configuration%20Management.md#use-git-commands-to-add-commit-and-push-your-branch-to-github)Use git commands to add, commit and push your branch to GitHub.

===================================================================================================================================================================================================================================

git status

git add

           git add inventory/

           git add playbooks/

Or 'git add . ' to add all files / changes

git commit -m "commit message"

git commit -m "Updated playbooks with new timezone and created new directory and a file inside it"

[![](https://camo.githubusercontent.com/dc13a6df3a10b75f2431b41e2f91e44c7c88200d4df14f4224688cdc30c21de4/68747470733a2f2f6c68342e676f6f676c6575736572636f6e74656e742e636f6d2f646176315f355266733659566a4d4265377275797970794a3672576a3532436255596b634f6a432d6768496457464235546f353961737139532d4a50715f4d3156597a5877425a485177306c42497057487942414e425056322d4d54326d63784743432d49383548475149464e4f5944776a6e6d456e36316d434b4c524e2d433947595369726e49)](https://camo.githubusercontent.com/dc13a6df3a10b75f2431b41e2f91e44c7c88200d4df14f4224688cdc30c21de4/68747470733a2f2f6c68342e676f6f676c6575736572636f6e74656e742e636f6d2f646176315f355266733659566a4d4265377275797970794a3672576a3532436255596b634f6a432d6768496457464235546f353961737139532d4a50715f4d3156597a5877425a485177306c42497057487942414e425056322d4d54326d63784743432d49383548475149464e4f5944776a6e6d456e36316d434b4c524e2d433947595369726e49)

1\.  Create a Pull request (PR) from terminal to github repository

git status

git push

    [![](https://camo.githubusercontent.com/a123648d0bbe20517b24cadd22a257cb083c150a8153fa53da4a30c3d41d1a27/68747470733a2f2f6c68362e676f6f676c6575736572636f6e74656e742e636f6d2f44334f4d7738326b537a71694e425a3269747937554c4e6e61373747536b7476747931417470327064643244426d3477435f67306c4750337637457455794878374f764452303071645f31614e444d6b41435464706a454a7855597a324a316b6c5858784a7a792d476e6d716d434a4b6a566733317442414d53467a7a4565556348555034464657)](https://camo.githubusercontent.com/a123648d0bbe20517b24cadd22a257cb083c150a8153fa53da4a30c3d41d1a27/68747470733a2f2f6c68362e676f6f676c6575736572636f6e74656e742e636f6d2f44334f4d7738326b537a71694e425a3269747937554c4e6e61373747536b7476747931417470327064643244426d3477435f67306c4750337637457455794878374f764452303071645f31614e444d6b41435464706a454a7855597a324a316b6c5858784a7a792d476e6d716d434a4b6a566733317442414d53467a7a4565556348555034464657)

. Wear a hat of another developer for a second, and act as a reviewer.

. If the reviewer is happy with your new feature development, merge the code to the master branch.

[](https://github.com/BimsDevs/ansible-config-mgt/blob/master/Ansible%20Configuration%20Management.md#steps)Steps:

==================================================================================================================

Go into the branch which is to be merged to the master in github

Click on Compare and Pull request tab (green)

Click on Merge Pull Request

Confirm Merge

[![](https://camo.githubusercontent.com/329314a8e541d94ad47a15cbe552753b2d6325a759f8ca0c4cdc85276abeeeed/68747470733a2f2f6c68342e676f6f676c6575736572636f6e74656e742e636f6d2f536978317245446f45724c6a7a4b704f53493539544b48344e476d474152585a4931474a6b4b39315475513366504f306d4d3062617a352d435067635f6f795744706e6f4231582d59454f6855303943764b57674c69704a7a4c4a705459436d42534635664447425457326f704b59455f437144694e5972684f30656546526b4577524433365441)](https://camo.githubusercontent.com/329314a8e541d94ad47a15cbe552753b2d6325a759f8ca0c4cdc85276abeeeed/68747470733a2f2f6c68342e676f6f676c6575736572636f6e74656e742e636f6d2f536978317245446f45724c6a7a4b704f53493539544b48344e476d474152585a4931474a6b4b39315475513366504f306d4d3062617a352d435067635f6f795744706e6f4231582d59454f6855303943764b57674c69704a7a4c4a705459436d42534635664447425457326f704b59455f437144694e5972684f30656546526b4577524433365441)

[![](https://camo.githubusercontent.com/7f364084067880811d95b8b10d78a53d92539c47ee000c90a6ecb6982a9336ba/68747470733a2f2f6c68362e676f6f676c6575736572636f6e74656e742e636f6d2f587943716c2d6d38487a546469525052306b6142306446794b4278494937694a52656c4c465831445653704b45615370576c36584254337866364d714a7a6b6b785661584e686e6561754352753243314f63614877754574646e3866734c5447687043313667795579615231484e52527334346645565f396e704f5f42794378587a70486f474c6f)](https://camo.githubusercontent.com/7f364084067880811d95b8b10d78a53d92539c47ee000c90a6ecb6982a9336ba/68747470733a2f2f6c68362e676f6f676c6575736572636f6e74656e742e636f6d2f587943716c2d6d38487a546469525052306b6142306446794b4278494937694a52656c4c465831445653704b45615370576c36584254337866364d714a7a6b6b785661584e686e6561754352753243314f63614877754574646e3866734c5447687043313667795579615231484e52527334346645565f396e704f5f42794378587a70486f474c6f)

[![](https://camo.githubusercontent.com/546ab90c2dc9d6f565934c209f05eebeb970880dd2d2c655acfe8b8a78e3fabb/68747470733a2f2f6c68352e676f6f676c6575736572636f6e74656e742e636f6d2f6976535942504f39426a55696f3777546a5f314a2d5951787854354b4c717567444178306345687a31357231356e4c765861564c4671333233595761436e61786e4a5067706168612d4b6f3465313432454c6948795f7351516832735456386d6f4d424437465231544233682d4e6656506854654f634167737075535a77743653667337514c4c55)](https://camo.githubusercontent.com/546ab90c2dc9d6f565934c209f05eebeb970880dd2d2c655acfe8b8a78e3fabb/68747470733a2f2f6c68352e676f6f676c6575736572636f6e74656e742e636f6d2f6976535942504f39426a55696f3777546a5f314a2d5951787854354b4c717567444178306345687a31357231356e4c765861564c4671333233595761436e61786e4a5067706168612d4b6f3465313432454c6948795f7351516832735456386d6f4d424437465231544233682d4e6656506854654f634167737075535a77743653667337514c4c55)

[![](https://camo.githubusercontent.com/59f831283dcb14a2890e3ea3be6df52b8ce8213524c44be5a5c1bf1800ee2f32/68747470733a2f2f6c68362e676f6f676c6575736572636f6e74656e742e636f6d2f6f39686d5332586e72456c44484f5f616e6a466a4365667668504f5671507467397a4476512d784a30346939325f426730796f325935342d6870365a72415333373437376c6d65327377786530567132726931335879706e63584b574f394e45554254387462654161686e3847636e6f3836317332594237674f68494633467a5f627a6b3243514b)](https://camo.githubusercontent.com/59f831283dcb14a2890e3ea3be6df52b8ce8213524c44be5a5c1bf1800ee2f32/68747470733a2f2f6c68362e676f6f676c6575736572636f6e74656e742e636f6d2f6f39686d5332586e72456c44484f5f616e6a466a4365667668504f5671507467397a4476512d784a30346939325f426730796f325935342d6870365a72415333373437376c6d65327377786530567132726931335879706e63584b574f394e45554254387462654161686e3847636e6f3836317332594237674f68494633467a5f627a6b3243514b)

1\.  Head back on your terminal, checkout from the feature branch into the master, and pull down the latest changes.

git checkout master

git pull

[![](https://camo.githubusercontent.com/f1eb7ffd08e0b835935786e618eebb1c30c1a9c32a7f098e2fa286a7467432c9/68747470733a2f2f6c68362e676f6f676c6575736572636f6e74656e742e636f6d2f4f63612d336261786f565064464c65464a55635043615f4131534349426839733958326571656d4138596c6970486679316b6176634249566f546a70744d6c343235786769756c6931506f3655574b384a416f7555484548674a377a524d4251677a75395659597673564f6a4254526247417a3879395447664852556a7a316d514c2d3567695653)](https://camo.githubusercontent.com/f1eb7ffd08e0b835935786e618eebb1c30c1a9c32a7f098e2fa286a7467432c9/68747470733a2f2f6c68362e676f6f676c6575736572636f6e74656e742e636f6d2f4f63612d336261786f565064464c65464a55635043615f4131534349426839733958326571656d4138596c6970486679316b6176634249566f546a70744d6c343235786769756c6931506f3655574b384a416f7555484548674a377a524d4251677a75395659597673564f6a4254526247417a3879395447664852556a7a316d514c2d3567695653)

Once your code changes appear in master branch - Jenkins will do its job and save all the files (build artifacts) to /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/ directory on Jenkins-Ansible server.

[![](https://camo.githubusercontent.com/86c79d5bf57bea38457fbc01841b66ca62ded6c160709f44e70fcd072d79fab9/68747470733a2f2f6c68332e676f6f676c6575736572636f6e74656e742e636f6d2f534545554a4c346151555a6967456b58583469305944702d415078325f54504d4f415054483447745676467139366a61614255366f546c706858336934424353344c33635076444a4336316762615f56684f6f7533787a3158312d766d67356f44636a34414e3839512d396f4357686143596e4943394161546f7171357049517876445a33387a37)](https://camo.githubusercontent.com/86c79d5bf57bea38457fbc01841b66ca62ded6c160709f44e70fcd072d79fab9/68747470733a2f2f6c68332e676f6f676c6575736572636f6e74656e742e636f6d2f534545554a4c346151555a6967456b58583469305944702d415078325f54504d4f415054483447745676467139366a61614255366f546c706858336934424353344c33635076444a4336316762615f56684f6f7533787a3158312d766d67356f44636a34414e3839512d396f4357686143596e4943394161546f7171357049517876445a33387a37)

[](https://github.com/BimsDevs/ansible-config-mgt/blob/master/Ansible%20Configuration%20Management.md#step-7---run-first-ansible-test)Step 7 - Run first Ansible test

---------------------------------------------------------------------------------------------------------------------------------------------------------------------

Now, it is time to execute ansible-playbook command and verify if your playbook actually works:

sudo ansible-playbook -i /var/lib/jenkins/jobs/ansible/builds//archive/inventory/dev.yml /var/lib/jenkins/jobs/ansible/builds//archive/playbooks/common.yml

sudo ansible-playbook -i /var/lib/jenkins/jobs/ansible/builds/11/archive/inventory/dev.yml /var/lib/jenkins/jobs/ansible/builds/11/archive/playbooks/common.yml

Output

[![](https://camo.githubusercontent.com/ae65cb3ded0286a9cfbf36216e813250cd7768b244df26ece0e6e591e363b04a/68747470733a2f2f6c68362e676f6f676c6575736572636f6e74656e742e636f6d2f7a384f695435574a31716b50554f6a483666594861694451347a3352316c6779422d574a4c695068622d6e48724f6841686b346b63784f4148763371543049534176547a532d484f455562316742485953794f56504b7a4a736230353232587162396b794b497335443731654235536141396b64694d34746f384552574963443955464f715a315a)](https://camo.githubusercontent.com/ae65cb3ded0286a9cfbf36216e813250cd7768b244df26ece0e6e591e363b04a/68747470733a2f2f6c68362e676f6f676c6575736572636f6e74656e742e636f6d2f7a384f695435574a31716b50554f6a483666594861694451347a3352316c6779422d574a4c695068622d6e48724f6841686b346b63784f4148763371543049534176547a532d484f455562316742485953794f56504b7a4a736230353232587162396b794b497335443731654235536141396b64694d34746f384552574963443955464f715a315a)

[![](https://camo.githubusercontent.com/d8988325e4a468560ca1e481fe600f36c04a1e30b7bc588ec6ce9a7c089523cf/68747470733a2f2f6c68342e676f6f676c6575736572636f6e74656e742e636f6d2f553153384138774c754232556f424e5375793677485049696d795f3174443733345153315148506167334a592d79376e587a367969465658386b583359465832617a684563682d32426e52676c6249377031462d49534e517656446e776b774f3565586878353157722d79537077585931392d5858544e4c69416b6e3030334e664e4a3267564861)](https://camo.githubusercontent.com/d8988325e4a468560ca1e481fe600f36c04a1e30b7bc588ec6ce9a7c089523cf/68747470733a2f2f6c68342e676f6f676c6575736572636f6e74656e742e636f6d2f553153384138774c754232556f424e5375793677485049696d795f3174443733345153315148506167334a592d79376e587a367969465658386b583359465832617a684563682d32426e52676c6249377031462d49534e517656446e776b774f3565586878353157722d79537077585931392d5858544e4c69416b6e3030334e664e4a3267564861)

[](https://github.com/BimsDevs/ansible-config-mgt/blob/master/Ansible%20Configuration%20Management.md#blocker-1)Blocker:

------------------------------------------------------------------------------------------------------------------------

The below ansible playbook syntax was run but gave an error.

sudo ansible-playbook -i /var/lib/jenkins/jobs/ansible/builds/11/archive/inventory/dev.yml /var/lib/jenkins/jobs/ansible/builds/11/archive/playbooks/common.yml

Solution: 'sudo' was removed and it worked as per above output. This is because the parameter 'become' has been marked as 'yes' in the playbook which implies the sudo privilege already exists and does not need to be duplicated.

ansible-playbook -i /var/lib/jenkins/jobs/ansible/builds/11/archive/inventory/dev.yml /var/lib/jenkins/jobs/ansible/builds/11/archive/playbooks/common.yml

[![](https://camo.githubusercontent.com/4f87fb75a9f3003b02b54c00b67dc327486ccbf224dc463495063d36ed81c415/68747470733a2f2f6c68362e676f6f676c6575736572636f6e74656e742e636f6d2f526146632d59596a653668354c767076444c31584930316b446e524b755a485634375855327a614e4e6d56397646724170776848566d616746684a414c6b613647352d714551654b49473942773641747331457643366d466a6574456556545343515473755a69734170614a4833373235396f43776b7073384375655930794b7648766f5249744b)](https://camo.githubusercontent.com/4f87fb75a9f3003b02b54c00b67dc327486ccbf224dc463495063d36ed81c415/68747470733a2f2f6c68362e676f6f676c6575736572636f6e74656e742e636f6d2f526146632d59596a653668354c767076444c31584930316b446e524b755a485634375855327a614e4e6d56397646724170776848566d616746684a414c6b613647352d714551654b49473942773641747331457643366d466a6574456556545343515473755a69734170614a4833373235396f43776b7073384375655930794b7648766f5249744b)

You can go to each of the servers and check if wireshark has been installed by running which wireshark or wireshark --version

[![](https://camo.githubusercontent.com/f700e71d0a2597f0c609e3567a71c4757dbc00b061761d39fda7b76a076df853/68747470733a2f2f6c68332e676f6f676c6575736572636f6e74656e742e636f6d2f69456e7653517737586151627162694d6a684f7645624c496e4c385f5436706366367850727850564b306c486970677577507070334766736a3871334768595777744d534767365a6449625f423570724e6145356d516e34555f756b50797a53656d5545376d38796c5930744a78517447366e4352576d754a32614e6d556f4c5148774f36786878)](https://camo.githubusercontent.com/f700e71d0a2597f0c609e3567a71c4757dbc00b061761d39fda7b76a076df853/68747470733a2f2f6c68332e676f6f676c6575736572636f6e74656e742e636f6d2f69456e7653517737586151627162694d6a684f7645624c496e4c385f5436706366367850727850564b306c486970677577507070334766736a3871334768595777744d534767365a6449625f423570724e6145356d516e34555f756b50797a53656d5545376d38796c5930744a78517447366e4352576d754a32614e6d556f4c5148774f36786878)

Your updated with Ansible architecture now looks like this:

[![](https://camo.githubusercontent.com/25be2947cf1edff36d413748f7767bf99c228be0dc3d2cbb9db144b4ad8ac64a/68747470733a2f2f6c68342e676f6f676c6575736572636f6e74656e742e636f6d2f5465626f5a755559316c374d716d56676d48393976674150536a6c76686933726f344559736f4e46614f424e6e4365313731335165446f307871505f3533685964424b6b323348594a6b49317739734150694e7a436c484163394831694673653075556e3979574f3774706b6c764e784a4b64535a504f38783832673367624b43654e4d5a2d7563)](https://camo.githubusercontent.com/25be2947cf1edff36d413748f7767bf99c228be0dc3d2cbb9db144b4ad8ac64a/68747470733a2f2f6c68342e676f6f676c6575736572636f6e74656e742e636f6d2f5465626f5a755559316c374d716d56676d48393976674150536a6c76686933726f344559736f4e46614f424e6e4365313731335165446f307871505f3533685964424b6b323348594a6b49317739734150694e7a436c484163394831694673653075556e3979574f3774706b6c764e784a4b64535a504f38783832673367624b43654e4d5a2d7563)

[](https://github.com/BimsDevs/ansible-config-mgt/blob/master/Ansible%20Configuration%20Management.md#repeat-once-again)Repeat once again

-----------------------------------------------------------------------------------------------------------------------------------------

Update your ansible playbook with some new Ansible tasks and go through the full checkout -> change codes -> commit -> PR -> merge -> build -> ansible-playbook cycle again to see how easily you can manage a servers fleet of any size with just one command!

-   Checkout into branch: git checkout ansible-5511-feature01

-   Create a new file: touch test

-   Push to github:

-   Add random comments into the test file in github

-   Commit change (github)

-   Compare and Pull request (github)

-   Merge and confirm merge

-   Ensure that the (elastic) IP address in the webhook is correct.

-   Confirm new build and changes are success in Jenkins (Jenkins)

-   Execute ansible:

ansible-playbook -i /var/lib/jenkins/jobs/ansible/builds/11/archive/inventory/dev.yml /var/lib/jenkins/jobs/ansible/builds/11/archive/playbooks/common.yml

-   Checkout into branch: git checkout ansible-5511-feature01

Created a new file: touch test

[![](https://camo.githubusercontent.com/42614d4b07d61a75b0692a24086e9ed9c25d653f566948cac4517b6c1f9d22d9/68747470733a2f2f6c68342e676f6f676c6575736572636f6e74656e742e636f6d2f445a537133766556324465424956576a42767a7a6571794d714370354956476e5a345f3971493749684b36344e4370304941573979646d3766683668487670345848664a6635386e3662523166394446735f4369776d6a4b614b644f386e566d62426c32313363565a5463716e4348466367523234585f41775f456a44497738624f3232466a6342)](https://camo.githubusercontent.com/42614d4b07d61a75b0692a24086e9ed9c25d653f566948cac4517b6c1f9d22d9/68747470733a2f2f6c68342e676f6f676c6575736572636f6e74656e742e636f6d2f445a537133766556324465424956576a42767a7a6571794d714370354956476e5a345f3971493749684b36344e4370304941573979646d3766683668487670345848664a6635386e3662523166394446735f4369776d6a4b614b644f386e566d62426c32313363565a5463716e4348466367523234585f41775f456a44497738624f3232466a6342)

[![](https://camo.githubusercontent.com/ea08ccba25cd060accaa1a5b8b06354b4dd3a3c6b3c18b41e6846a47dc154198/68747470733a2f2f6c68362e676f6f676c6575736572636f6e74656e742e636f6d2f34753576343775614f4d5a74526566767138657a45337059516335313341526151653736676b6d58585a416948714d674665765976393952376461592d64504e7177614971675075664f58726176576c465330754132796535383477586f2d596c506f526c5338696e684255374c527a6761754f596e696e42754a2d35704a3457306653664f4c43)](https://camo.githubusercontent.com/ea08ccba25cd060accaa1a5b8b06354b4dd3a3c6b3c18b41e6846a47dc154198/68747470733a2f2f6c68362e676f6f676c6575736572636f6e74656e742e636f6d2f34753576343775614f4d5a74526566767138657a45337059516335313341526151653736676b6d58585a416948714d674665765976393952376461592d64504e7177614971675075664f58726176576c465330754132796535383477586f2d596c506f526c5338696e684255374c527a6761754f596e696e42754a2d35704a3457306653664f4c43)

-   Push to github: git push

[![](https://camo.githubusercontent.com/2454a0998405a55e746eb344024f0676ad8c337530c1e43532f474953ea0f634/68747470733a2f2f6c68352e676f6f676c6575736572636f6e74656e742e636f6d2f4a6b56526f4e683833516170546c6a4c664f496a554c3067354d3638367a6c373739485a56446c75464d2d6f4d64516e654966644f6e52517a30736c4767357a364c4242444a356b3646714946354b4137716e6f542d4674663559654559616e5a4d764579445978594c4e616545676f6d4d4a54744d646c694f74726e30485267346b5758696c69)](https://camo.githubusercontent.com/2454a0998405a55e746eb344024f0676ad8c337530c1e43532f474953ea0f634/68747470733a2f2f6c68352e676f6f676c6575736572636f6e74656e742e636f6d2f4a6b56526f4e683833516170546c6a4c664f496a554c3067354d3638367a6c373739485a56446c75464d2d6f4d64516e654966644f6e52517a30736c4767357a364c4242444a356b3646714946354b4137716e6f542d4674663559654559616e5a4d764579445978594c4e616545676f6d4d4a54744d646c694f74726e30485267346b5758696c69)

-   Added random comments into the test file in github

Commit change (github)

Compare and Pull request (github)

Merge and confirm merge

[![](https://camo.githubusercontent.com/f70255fb5ac690e38ccbb4dfa8b92dc1ee10d2862d5630da717c176967d90e70/68747470733a2f2f6c68362e676f6f676c6575736572636f6e74656e742e636f6d2f7456584659675856673038384a496f32614b513349776e384a6b2d6d624d33386b5566787252333853544b36484e4332484e3255747a502d78506153536f717a6c6a59694d36496646447142396f497162636b615a7142476c52697831414b444270734f753744376d6565353839356f68634747445a6d31614c58344a7a553736794b717651696d)](https://camo.githubusercontent.com/f70255fb5ac690e38ccbb4dfa8b92dc1ee10d2862d5630da717c176967d90e70/68747470733a2f2f6c68362e676f6f676c6575736572636f6e74656e742e636f6d2f7456584659675856673038384a496f32614b513349776e384a6b2d6d624d33386b5566787252333853544b36484e4332484e3255747a502d78506153536f717a6c6a59694d36496646447142396f497162636b615a7142476c52697831414b444270734f753744376d6565353839356f68634747445a6d31614c58344a7a553736794b717651696d)

-   Ensure that the (elastic) IP address in the webhook is correct.

[![](https://camo.githubusercontent.com/a92170c6f3ab5818a39fda15e41f54da0c387441d18ffb02732fbaf439c5a883/68747470733a2f2f6c68332e676f6f676c6575736572636f6e74656e742e636f6d2f766f72497a6b38655a6e517136517a5568666a6854796f4c326f44566d4b5a355757344d6c3852425f394e65364632793976314a4679525141374830774a4967774a476731636e62744b335568464a595a6832415f6566317250487a626f38677867394370494a366932526a71484f674344346f42544b35614266467745717255346734302d6e51)](https://camo.githubusercontent.com/a92170c6f3ab5818a39fda15e41f54da0c387441d18ffb02732fbaf439c5a883/68747470733a2f2f6c68332e676f6f676c6575736572636f6e74656e742e636f6d2f766f72497a6b38655a6e517136517a5568666a6854796f4c326f44566d4b5a355757344d6c3852425f394e65364632793976314a4679525141374830774a4967774a476731636e62744b335568464a595a6832415f6566317250487a626f38677867394370494a366932526a71484f674344346f42544b35614266467745717255346734302d6e51)

-   Ensure that the (elastic) IP address in the webhook is correct.

  Confirm new build and changes are success in Jenkins (Jenkins)

[![](https://camo.githubusercontent.com/45b9c756d19208f93f80f47a2f35048ac4dc8b4f3ce46e04152f2a18a2ec7773/68747470733a2f2f6c68332e676f6f676c6575736572636f6e74656e742e636f6d2f345052496c43514a477171516157346c5f61686c45387a7174583831395f3134454c376c44584764576454474b73485a76644b4e66445637556570505633557a4e4c615631637751364e7677756f736d6535586d4155643475724f78314446596c303468636b36565a475f766837644f6a6354435075474c424d6a4e6b536e706572646d79684862)](https://camo.githubusercontent.com/45b9c756d19208f93f80f47a2f35048ac4dc8b4f3ce46e04152f2a18a2ec7773/68747470733a2f2f6c68332e676f6f676c6575736572636f6e74656e742e636f6d2f345052496c43514a477171516157346c5f61686c45387a7174583831395f3134454c376c44584764576454474b73485a76644b4e66445637556570505633557a4e4c615631637751364e7677756f736d6535586d4155643475724f78314446596c303468636b36565a475f766837644f6a6354435075474c424d6a4e6b536e706572646d79684862)

[![](https://camo.githubusercontent.com/32b3785ac71ac2cb0d486b1fac41c50d4a29cedbbd1f7bbfa7fb2c2cd557b708/68747470733a2f2f6c68332e676f6f676c6575736572636f6e74656e742e636f6d2f6c425274437478743242413662556e4b557a5445417965655a5657663979422d77663470396c5a6e466b66554b30676532646e365a7a7a4f4d45496d2d5a773057524d67502d4c5164323850314b4c44376b764f716a365a6f5239455f676f63452d456d4e676e576b6a686d6e36474d64416165374c4235723365627667415a5064486a62615f54)](https://camo.githubusercontent.com/32b3785ac71ac2cb0d486b1fac41c50d4a29cedbbd1f7bbfa7fb2c2cd557b708/68747470733a2f2f6c68332e676f6f676c6575736572636f6e74656e742e636f6d2f6c425274437478743242413662556e4b557a5445417965655a5657663979422d77663470396c5a6e466b66554b30676532646e365a7a7a4f4d45496d2d5a773057524d67502d4c5164323850314b4c44376b764f716a365a6f5239455f676f63452d456d4e676e576b6a686d6e36474d64416165374c4235723365627667415a5064486a62615f54)

-   Execute ansible:

ansible-playbook -i /var/lib/jenkins/jobs/ansible/builds/11/archive/inventory/dev.yml /var/lib/jenkins/jobs/ansible/builds/11/archive/playbooks/common.yml

[![](https://camo.githubusercontent.com/c9a970d2be5cb823ce5dc2a52ecd64b88f9a2c7f4836a63dd3a699e06657d75f/68747470733a2f2f6c68352e676f6f676c6575736572636f6e74656e742e636f6d2f65643454794e4564776b42564c7a627a587870502d36655339456a57444b454a32775652574b55694b525f34653058376e334c36736c66374e486549686f2d705f6536716972674377705573556d3246657379547230585855515847755f444e4757426c307576734f5a72744e5878745a6c33436978785039316668786b6e4f384a374d35392d71)](https://camo.githubusercontent.com/c9a970d2be5cb823ce5dc2a52ecd64b88f9a2c7f4836a63dd3a699e06657d75f/68747470733a2f2f6c68352e676f6f676c6575736572636f6e74656e742e636f6d2f65643454794e4564776b42564c7a627a587870502d36655339456a57444b454a32775652574b55694b525f34653058376e334c36736c66374e486549686f2d705f6536716972674377705573556d3246657379547230585855515847755f444e4757426c307576734f5a72744e5878745a6c33436978785039316668786b6e4f384a374d35392d71)

[![](https://camo.githubusercontent.com/e089a3d93b2ef715d3e3f6bee678e3505dd9a72fa401d270efc9e0b4b7a3d16a/68747470733a2f2f6c68362e676f6f676c6575736572636f6e74656e742e636f6d2f4e355538644251566f415761726676325f4e374c2d4a555077322d3431555a556d5945506462704553576f636f637a4e7372532d557341633958566434714f472d50313333716456784938454b64496f637459646d4e352d7844777934435355666d576930494f4f774d48694c5f5a74323050526d7535726475756e5f65546174636b6a76436c63)](https://camo.githubusercontent.com/e089a3d93b2ef715d3e3f6bee678e3505dd9a72fa401d270efc9e0b4b7a3d16a/68747470733a2f2f6c68362e676f6f676c6575736572636f6e74656e742e636f6d2f4e355538644251566f415761726676325f4e374c2d4a555077322d3431555a556d5945506462704553576f636f637a4e7372532d557341633958566434714f472d50313333716456784938454b64496f637459646d4e352d7844777934435355666d576930494f4f774d48694c5f5a74323050526d7535726475756e5f65546174636b6a76436c63)

The above shows the execution of automated routine tasks by implementing this Ansible project.

[](https://github.com/BimsDevs/ansible-config-mgt/blob/master/Ansible%20Configuration%20Management.md#credits)Credits:

----------------------------------------------------------------------------------------------------------------------

darey.io

<https://phoenixnap.com/kb/ansible-create-file>

<https://docs.ansible.com/ansible/latest/collections/community/general/timezone_module.html>

<https://www.mydailytutorials.com/ansible-create-files/>

<https://linuxize.com/post/how-to-set-or-change-timezone-in-linux/>
