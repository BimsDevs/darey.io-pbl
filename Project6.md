

Web Solution With WordPress

This project has to do with preparing storage infrastructures on two linux servers and implementing a web solution using wordPress.

WordPress is a free and open-source content management system written in PHP  and paired with MySQL or MariaDB as its backend Relational Database Management System (RDBMS).

Generally, web, or mobile solutions are implemented based on what is called the Three-tier Architecture. Three-tier Architecture is a client-server software architecture pattern that comprises 3 separate layers.

1.  Presentation Layer (PL): This is the user interface such as the client server or browser on your laptop.

2.  Business Layer (BL): This is the backend program that implements business logic. Application or Web Server

3.  Data Access or Management Layer (DAL): This is the layer for computer data storage and data access. [Database Server](https://www.computerhope.com/jargon/d/database-server.htm) or File System Server such as [FTP server](https://titanftp.com/2018/09/11/what-is-an-ftp-server/), or [NFS Server](https://searchenterprisedesktop.techtarget.com/definition/Network-File-System).

In this project, you will have the hands-on experience that showcases Three-tier Architecture while also ensuring that the disks used to store files on the Linux servers are adequately partitioned and managed through programs such as gdisk and LVM respectively.

A database server is a computer system that provides other computers with services related to accessing and retrieving data from a database. Access to the database server may occur via a "front end" running locally a user's machine (e.g., [phpMyAdmin](https://www.computerhope.com/jargon/p/phpmyadm.htm)), or "back end" running on the database server itself, accessed by remote shell. After the information in the database is retrieved, it is outputted to the user requesting the data.

FTP Server (which stands for File Transfer Protocol Server) is a software application which enables the transfer of files from one computer to another.

The Network File System (NFS) is a [client](https://searchenterprisedesktop.techtarget.com/definition/client)/[server](https://whatis.techtarget.com/definition/server)  [application](https://searchsoftwarequality.techtarget.com/definition/application) that lets a computer user view and optionally store and update [files](https://whatis.techtarget.com/definition/file) on a remote computer as though they were on the user's own computer.

Getting Started

Set up the 3-Tier Architecture using CentOS - Redhat:
-----------------------------------------------------

1.  A Laptop or PC to serve as the client

2.  A Linux Server to serve as the web server (This is where you will install Wordpress)

3.  A Linux server to serve as the database server

Step 1: Prepare the Web Server

Initial tasks include: 

a. Create a storage volume

b.Attach the storage to a VM

c. Format and access the volume

Add 3 disks from virtual box to the Linux server. Determine the size of each disk yourself

1.  Access AWS console: EC2 - Instances - Check the box of the instance to be used - select Volumes from the left menu - Create volume - select size (chose 10GB for each disk)

![](https://lh4.googleusercontent.com/yk0BTTeJZ0fhmbKIixDtQtQ7sM2IN48IpflNnSB8McePMOMRTswqJRf7JkfDsU22lViohGkGLq_tw3em_aszJ-de7AJbfHTFN2gjYyC6CvMy0SoQtpzn6BP2lA9GWG9gK6zAc_Nw)

![](https://lh5.googleusercontent.com/MkcfD5ikCYsBWHYk8VkOgKv58Q_Mphpo3fyCsKMYR3YfStF6HwapHX349lPhmDhCX0lhUpeE_WjwLKlbHpFubgpwpcRioeE-TRDOokjH1SdgaNp5POnBT1j3o6VBVYhcdlENfP-X)

Right click on the name of the volume and select attach volume

![](https://lh6.googleusercontent.com/P0aAwkfX8D2mKWG0mHy_M-3dX_6b1-RE5G7MfFFWVVGsx4vi0lmqN_WIt4Hwu1eKs_4p9f-R8lVOjEdkD5SmliLYLkXfviS_D9bnuYz29giGD6ZNJ_Zm_xJi5igBxSdZ3K-v0RHo)

![](https://lh3.googleusercontent.com/vSObFBHEhPT4kbEDkdXiPzoFQbP367Rn_Y57DD3xDw7uXqTbz8QWYGNQQbOHoH0wRZK_35kQiG0tuliJBUx9sOM4ImOYXBFUhVY20DscmQr6OOwHlMLRAnyk-Q-l-njGK0Ck4AWQ)     

b Attach volumes to instance in AWS 

c Format the Volume.

Access terminal on PC

Terminal - cd Downloads - paste the ssh file from AWS - Yes .

1.  Check the current storage in the terminal - lsblk

![](https://lh6.googleusercontent.com/iOj8yU3D-1fbo-NLr2VFSPKm7Jy_8vl6FpwgoMLxkyfMA8GNg9cIBTt_lPr5K9uAkMlxNc3QRQsXDNylEWdEzwYW-eURCqFW0XanVGc_8QNZUsXg9-2WPo6ZVrwPNWOfle7lmG1b)

1.  Use  df -h command to have a view and familiarise yourself with the disk configuration on the server: df -h

![](https://lh6.googleusercontent.com/algq0xlEj_4A1dngtt-Sy3f5A-BKve6ekaPZzT_uR3voQtZJCUeVVb554toCsPFyZ6chwWCNb8L2Lx1CLJW15448ArZ7lTvXIGp48EOtSbPCtUglJm0jRxKPVHb892fHd9zjuE4O)

1.  Use  gdisk  utility to create a single partition on each of the 3 disks

$ sudo gdisk /dev/xvdf   

? for help

Choose option 'n' to add a new partition

Partition number = 1

First sector = 2048  (default)

Last sector = 31457246  (default)

'W'  = Write table to disk and exit

![](https://lh6.googleusercontent.com/oRPPqZzuiKKBfobKu4wEnaKed0nCCZ6bY93yyqwZEnDWQbMxYwoZsM4GZ5HVJOjdzCc7uDMST6xsxlK2q_0i3mm-AIpZuu_egLLHolF1tlCszrFY7UgNyLDZQFw0AHXLJxQtA3Hy)

(Repeat for $ sudo gdisk /dev/xvdg and $ sudo gdisk /dev/xvdh)

1.  Use  lsblk utility to view the newly configured partition on each of the 3 disks.

Post-partitioning of dev/xvdg: lsblk

![](https://lh3.googleusercontent.com/O_OFrKGp2e4YxCEg-nGhKKwFVfajobkVwXdV_49QJN0GRteIkFeBqXGqVpoNKW99epBySm7Cfcks8_cWviBi5I4pHTNOgDrdf3riM4ev2-am1JmAN3QB-7LSQ0QNOXFUnmP3OCby)

Post-partitioning of dev/xvdf, dev/xvdg and xvdh:  lsblk

![](https://lh3.googleusercontent.com/JRGDW5Nmj1icOPYaqapm3zFdsyeW6ENQ43VSYHZeHNoKLHAAIUmpcnijtSOANBsx6dDuOGLwB698qgWSNgBWd7mQHNKSMYQ7PrpNWYOcZ0SeSeYDvEBYVd6DTTfYgi60ZYPxJKpN)

1.  Use lvmdiskscan utility to check for available storage for LVM. It shows the devices that are  suitable to be turned in physical volumes for LVM.

$ sudo lvmdiskscan

![](https://lh4.googleusercontent.com/JsoUY6Uwu-HpAgVO0LgfG543i0U1WY62fbjEwFm8wqrLcDHtmY8zdA7UUdk1y_krZUdldJez4DnE9IdOqj5MJ4TKpHjf7dissQInL-OYO336PJBniGCGGYQAHlKg5A5EQO3a0A7i)

Error: command not found

Tried: yum reinstall lvm2 but did not work

Solution: sudo yum install lvm2

![](https://lh4.googleusercontent.com/_0dYalQ3U5n82AKWZ5-IsH8h4cvgcuFXFX_lUD93dFxKt7CcvndQF8nWfiVV6tVXN7__RNfmEj2kQx0z1WvUD5YXR5Gkw3DsXmNsRr0Kyott1tABuZjjCbS0aDiiyAkMo6HRkzJP)

$ sudo lvmdiskscan

![](https://lh4.googleusercontent.com/7g7j3ZFNWXKspG4jP7pSzKr1WhfIuKfrDPAgwpOXrJL_r-BQktZ3_VTCkNAm50O7wj7T5Qdp-izqqtq8C16l_pM-_d83gOblfzW0VYtnMlclaihGU8MxCaOf27Gfv7X1RNYgXOZc)

1.  ### Creating Physical Volumes From Raw Storage Devices: In order to use storage devices with LVM, they must first be marked as a physical volume. This specifies that LVM can use the device within a volume group.

Use  pvcreate  utility to mark the disks as LVM physical volumes. 

sudo wipefs -a /dev/xvdf && sudo pvcreate /dev/xvdf

sudo wipefs -a /dev/xvdg && sudo pvcreate /dev/xvdg

sudo wipefs -a /dev/xvdh && sudo pvcreate /dev/xvdh

![](https://lh5.googleusercontent.com/lgUSpyTolowKhWoQO7KdyaRkyLNuga88F3s_sA4mIakz8UNHnE1W_YluQkmeyqqfJU5HSKFZkMdRRmijjY1k_yX5k1reguu7j9dEDggnLdMZaBQjjmO0qE-GsZH91wZy69etnxvq)

7\.    Creating new volume group from physical volume: Use vgcreate  utility to add the PVs to a 

       volume group from LVM physical volume. Name of new VG is Webdata-vg

sudo vgcreate volume_group_name /dev/xvdb1 /dev/xvdc1 /dev/xvdf1

$ sudo vgcreate webdata-vg /dev/xvdf /dev/xvdg /dev/xvdh

![](https://lh4.googleusercontent.com/pg_hM-kbH5P03H2x7ozz0hswM7ewx4QPDjJejI84yedcgtTXpSL67HCAmlfz-2JTMx_V7GTfDvwTaWNY2gmGEVwrmSddfT0tXLXfHdkZOaJhfy02eUvkGYVOkS01tVh0R0wa0zo1)

   Check the volume group: sudo vgs

   sudo vgdisplay

![](https://lh3.googleusercontent.com/O7RCVh4KRaUzYSScUgj_uTrPye0Ny9hk5Vm7gkJbZUkvDD8t2xneP80tBK2ukCLO08yXWGjkZMhrOjlDGtwwD3v0IQgDRXeqPlM6BvJFG7ApMQWuCf5V7i8UJ_clwm9xWyGkfbf9)

![](https://lh3.googleusercontent.com/ysy5OiuvcwzQY8EHpIiQLyTxz83gS74brhZTvEGTcUpTqIEADo0uwNkWjKFNrUXqXq1S8qYkgNIdq5klpYVFV1q3dKivOybQQWauvf1BULYWivv1eDC4SBXS4Q8kl_2RLSEKchri)

This shows a total vg size of 18.99GB

8\. Creating LV by specifying size: Use lvcreate utility to create 2 logical volumes. apps-lv (Use half of    the PV size), and logs-lv Use the remaining space of the PV size. NOTE: apps-lv will be used to store data for the Website while, logs-lv will be used to store data for logs.

      The command below creates a logical volume called apps-lv that uses    

                   50% of the total space in volume group webdata-vg and the remaining 50% 

                   for logs-lv

sudo lvcreate -n apps-lv -L 1.44g webdata-vg

sudo lvcreate -n logs-lv -L 1.44g webdata-vg

sudo lvs

![](https://lh4.googleusercontent.com/LVW4mU0NUMbAL3o81Avjl7ZLgj2C-S7HvztcyrxTm8TaBpC8pikI0IOne_TxV45-ZEveO2o7E0Zg1Dun44YkuXADVWeIzx__-dtXgOZHYt-EsTrImQiVyat6C3-HO0Me7wJ72Wfq)

9\. Confirm the entire setup and explore the below commands\
sudo vgdisplay -v #view complete setup - VG, PV, and LV

sudo lsblk

![](https://lh5.googleusercontent.com/est-3rght7h5s-JyyuCzrQNTlWCmX0x74M5y106EeyUJKAKyMVlpdwrBpEdvJ_ZkD-oRKwp294ahDXgcU36dDQPvnU-Ncd7OYR9BIU_ojYiMIJAXK_dUS6tTqMtQae1w7hmHINT6)

![](https://lh5.googleusercontent.com/bc6R56-U7vYZYBShX7h_r8xIgtwW6VIJoA3bXXdd41u3G_TtXmgApJ11HTR3pXfDeI6iGFtaYjBDa_JEb6y7v5ynxbDv6qkukk9rcxecu__voD1i0n9VGIyzAnOhpQRoZbsMSYda)

![](https://lh4.googleusercontent.com/123iM-470DB4wPyPbA0yYuPbVEQSjc3AXf-Z7bP-QQtuN_SmS8f9WbggdFByBhVNNLTIrMe3WTDS9Fp_Jt-VWrB6w_GDWYi4dodsEWPfV6-C-GTS38bn9yAYaeTHxfSJnoQYB02w)

sudo Lsblk

![](https://lh3.googleusercontent.com/jakyw0jAqip7DaUmbFH95s4KR-tyRzwPwC3CprrCPmUkhQkO4Ea3r1RTlOlYgPEUQjb-EmLC345UsteBHmh5kNg1w0TXZEWCtW2ZZrl81iaOB1LMLVcyaPKtdd4hz3jnDUNaAyJh)

sudo lvs #view logical volume summary

![](https://lh5.googleusercontent.com/1b-sNiMexPvkAAY2jg3KoscTYB1rclwJQlMVR325GF2PGgdpmFEq3uEcw0R3MynZPX-ML2E7Jusmj2w4g_5cwycjL67Ugy3N0lOb35Qk52qmCxPhwM-C4zuSdOwm2_gcZzBnpQtV)

10\. Use  mkfs.ext4 to format the logical volume with Ext4 filesystem

sudo mkfs -t ext4 /dev/webdata-vg/apps-lv

sudo mkfs -t ext4 /dev/webdata-vg/logs-lv

![](https://lh3.googleusercontent.com/p4_0niQnBQRDXww0VgQMB8z3m3mo2leWCfjQXo3s1lvQ1QygU9qMA9oLOI7WrxxJRQ6Sz9cfGi7RXk_ydCJZM8b7oCWWsBLje7UMS-ESxZIGIpyxQlepsbBqf1imWwAayJFJmsIG)

11\. Create /var/www/html directory to store website files

   $ sudo mkdir -p /var/www/html

12\. Create /home/recovery/logs to store backup of log data

$ sudo mkdir -p /home/recovery/logs

![](https://lh4.googleusercontent.com/K5DBZLiXvgjL5BwygZGHVV-xmxW4b6UDiPZUXeCcDtoZ4-060AQ1bHtnSrm8_WPe4lMaOSF3qGb1Jj2vu5ypl8ThCm_uSp0KaoBZobmr5FhQS8crmqoW-lbfqwxU-OC9tXnHF79x)

13\. Mount /var/www/html on apps-lv logical volume

sudo mount /dev/webdata-vg/apps-lv /var/www/html/

![](https://lh5.googleusercontent.com/aTVXofXUFS-RLwQwV3e_X4XKqqP-qYODwmxJoKipM8UDmRkrkik10XFSC-jENLqVJermAQVU_0cNhMZ5IKgmqrM3RO8bf0v7yi0ip0JBAvSAHiM-Klcm23FcNmOuiaV3WQc1jx--)

14.Use rsync  utility to backup all the files in the log directory /var/log into /home/recovery/logs (This is required before mounting the file system)

     Rsync can duplicate data between two directories whether the data is 

     collocated on the same com puter or over the network between two 

     Computers. The benefit of Rsync is the ability to update the mirrored 

     backup with computed changes from the data source with a minimal load to 

     system resources.

sudo rsync -av /var/log /home/recovery/logs

![](https://lh6.googleusercontent.com/YbussoGRNAbwXute4A_uCKj5GQghgR8KPqUp4XOkgRM_vLxU3fxOsYVXF1po_vJ-5E_o5eWxHcDaKLk3y6Y-J6slwvNvryosXEExqiGXuRxiw17Vc4_ev4dxVEkcCJElZ3CGS3t0)

15\. Mount /var/log on logs-lv logical volume. (Note that all the existing data on /var/log will be 

      deleted. That is why the rsync step above is very crucial)

sudo mount /dev/webdata-vg/logs-lv  /var/log  

![](https://lh4.googleusercontent.com/G-5-ccpphq6SijzPieLdPM0TR0pnSIj0GZIkg7d81WSfcbMKBQUncGHJV-wanb3LzQ3cGiSingp114OiIutqb5wEuD1HG7PSXb7XxHcQykPhB-Ojb3gooWmdRvpg2_lsf9O9BJye)

16\. Restore log files back into /var/log directory.

sudo rsync -av /home/recovery/logs/log/ /var/log

![](https://lh4.googleusercontent.com/OKYtFmtZGn7twb6mHNjNg6aUzYWpaUegg2RED_bnxk8vNBBBRpDbIrzGOw3l6MZ3J494SZ6xLgx-lqLCejGgVQbXaJCLfCaaMi-k9FXP-kO3gpvk_716Mw6OaaNjlfyjcQ3XV9Et)

17\. Update  /etc/fstab file so that the mount configuration will persist upon restart of the server.

sudo vi /etc/fstab

To check the content of the /etc/fstab file:  sudo cat /etc/fstab    

![](https://lh3.googleusercontent.com/ETFNpd1ZjrSlUON4rGvAWP7k204usifjne2Jc0Cbh2qjReMERttggm6ZWWYO4ehk4aCUh64alrIGURbyWju7eqLzWfUHyJPodJ4-hjvyHl9p2UM64eaK-MvcGX9EOqsVTPDpPvVd)

To list your drives and partitions run:  $ sudo fdisk -l  

![](https://lh3.googleusercontent.com/OiTzhUPUWS4J8dFf90Bl7OqrfdfTsTH5s9ksDU345cy4whXS3rKNmR2eqaF6I0bV_soXJ8YYcSoCmUOuq2FnkYQF_I8ye9a3eHjPHyk-UN-Woi4KbNChfr_etxaG3UAMdXpfnkPx)

18\. To check id (UUID) of the mounted logical volumes (LV) which will be used to 

update the /etc/fstab file, run: 

$ sudo blkid

![](https://lh5.googleusercontent.com/PuvLpIiCllGyXBHXw4AE45MB4NdtML4zIj7mG6xU20S2uZ1Y-eMRQZNKaFO_L_PQl0XnAXDGfePmtDfUPKV0WTNgp2Y2_qP-imGjL4nTlZLLCu0jPWmktKxRsnFzpH70HVHsfD-_)

19\. pdate the /etc/fstab file: sudo vi /etc/fstab

Post-Update:

Paste into the file to update:

UUID=c2cdbf21-2e83-4d60-8ad7-3deb12d217ce /var/www/html ext4 defaults,nofail 0 0 

UUID=2863d631-846c-44ee-9a6e-8811920a822a /var/log ext4 defaults,nofail 0 0 

![](https://lh5.googleusercontent.com/4uCu2M_0_NfMkyGnjP3uPZaabawVzcwwOrariSOVwmSnNJLGhn4pUyzKzTATtH9zovG6Rm8WK2iZs0PbmpBMIy4X7Dd2HL-oSjaxJHW4T73tQpaNABHochtSNFwN-IVYjyjALEaS)

Terms: 

UUID=71dea214-5656-4f1d-a3a7-a94277886adf = 

/var/log = Mount point

ext4 =  Type of file

Nofail = Bypass any mismatch. 

Update the system with the configuration: sudo systemctl daemon-reload 

To show the amount of free disk space on each mounted disk: sudo df -h

![](https://lh5.googleusercontent.com/fsGfjmeaZ0OD_kRitM_RROOpFmOqIMKnDiCn0wQf79M9Z6uQJBAdtbHTBaxuoVzFf3jglEejMD8NKo4c6O_rwakU5rw439v6eeUSnUPAM4bJDsPLbH0HQSPsI7VHBTrx08SBZMXD)

Step 2:  Prepare the Database Server

Initial tasks in Configuring a second server in Virtual Box include: 

a. Create a storage volume

b.Attach the storage to a VM

c. Format and access the volume

1.  Add 3 disks from virtual box to the Linux server. Determine the size of each disk yourself

![](https://lh3.googleusercontent.com/hiBVEwB9SS4SJu3mtiAEMGnmkjEGjdx6UVSid_lW-oSbZJRvvG5HTuRAY1rq2QSCCWTSzsqE37G4tPn7B8p9-3yAza0G2rRc-CrlQXWzl3hvs5bDIpg-N8XsEowUQZlCpQgS20uA)

1.  Open up the Linux terminal to begin configuration

2.  Use  df -h  command to have a view and familiarise yourself with the disk configuration on the server. Attach volume in AWS and check current storage in each disk

 sudo df -h,  and sudo lsblk

![](https://lh3.googleusercontent.com/xZZiKswk5wjyQsAp15c7NG2xtlznd_cnhnlM2XJlr-NxEQzenqlEa_oalXDQry_EgcWWRuvL8JBOnlxvzcQWEWSGg-1d-a5_u22q-3KN33lbD2Q5sOgq67jyh458HvzpNLPM0JgF)

1.  Use gdisk utility to create a single partition on each of the 3 disks

$ sudo gdisk /dev/xvdf

? for help

Choose option 'n' to add a new partition

Partition number = 1

First sector = 2048  (default)

Last sector = 31457246  (default)

'W'  = Write table to disk and exit

Repeat for: $ sudo gdisk /dev/xvdg  and 

$ sudo gdisk /dev/xvdh

![](https://lh6.googleusercontent.com/Bsise3ZnbIRYL54sga4wur1i38EQd_q9A5iO7nr4N59JBYTPmcHRv2fDKO_W9KIQWm_Alx4sfRLkECv1K1GPRzedT7egD4AJbf2IgsEL68yyT1lzhfLSjFkjqMuB0PaLc5JpTw8R)

1.  Use lsblk  utility to view the newly configured partition on each of the 3 disks.

Post-partitioning  of xvdf, xvdg and xvdh

![](https://lh3.googleusercontent.com/hie_9wR_Rmvw7FWb93HJmK7tA13_BsN1iG6GVPSNZ1XnOBa77lzUEdDXpwbwvQA6DTQD0Lk6EUbu1hqnOVgvKU4lG2lEb3088CbT5Qx5vrUmQaiVKFQdP9h4hftZXuKQerDQw5jQ)

1.  Use  lvmdiskscan  utility to check for available storage for LVM

$ sudo lvmdiskscan

(Requires an initial installation of:  sudo yum install lvm2)

![](https://lh6.googleusercontent.com/rj0VThnmlAIMfM2I61vK0am_enWvshGxZ6eWb3_JLD4TmdgdxTX-dnbH9MR4swDv13OtiNCJRZloGK-NlW-ZTRjtFPn6DrasCuGX_m7Z2q5p2PCzpZbZyC8RPjSOvk7StKjkQDQR)

1.  Use pvcreate utility to mark the disks as LVM physical volumes. In order to use storage devices with LVM, they must first be marked as a physical volume.

sudo wipefs -a /dev/xvdf && sudo pvcreate /dev/xvdf

sudo wipefs -a /dev/xvdg && sudo pvcreate /dev/xvdg

sudo wipefs -a /dev/xvdh && sudo pvcreate /dev/xvdh

![](https://lh4.googleusercontent.com/ZiHlARk3VjPpmMNmxsTHpLNXjzi_aqXwyymsIGqZKFs3S9ORcoiMUe9pnfSJx3NH275ezANxBEwSxK69AyaTs9Td25QaS2mNC45AIXmyhlAjD8ng_RXywG1anxlhzt3vQW6mEvcJ)

1.  Use vgcreate utility to add the PVs to a volume group. Name the VG dbdata-vg

$ sudo vgcreate dbdata-vg /dev/xvdf /dev/xvdg /dev/xvdh

![](https://lh6.googleusercontent.com/ZXeLUIVuuZRhtopw-uQF8ORol0ZIATk6WkaXpQoT2DkAlIgvSLoGYqHI4pDH94WVsAS0IYNLuheZUeQmMdJmWXxXoPjt73uRHeIi5MW_88jU2xk2zTLAs82ySPERIh0n7yvnEUk0)

Check VG:  sudo vgs

      Sudo vgdisplay

![](https://lh4.googleusercontent.com/uiyQ6x-Nr1b0yoUnidsohykiDbEevQryDHwEhtLZoHjj-Nz8fa1p0gnx6xkKzDkZe2_lzfsvnSYrpGTFhnm1FYGN3BOYdg49NE-bQZooxKt93MiDncdV5xbrvVU_V0umGoUQVHuj)

1.  Use lvcreate utility to create 2 logical volumes. db-lv (Use half of the PV size), and logs-lv  Use the remaining space of the PV size. NOTE: db-lv will be used to store database files while logs-lv will be used to store data for logs.

sudo lvcreate -n db-lv -L 1.44g dbdata-vg

sudo lvcreate -n logs-lv -L 1.44g dbdata-vg

sudo lvs

![](https://lh3.googleusercontent.com/DlQo4eaZq8nJSUSD9Z8ZIwt8pN0abFq5FDdq8y1fS2MhmzpSsheYyuDoVkNtnFJ-udLTB9eH59zIg4DAr8zvzc4YG5yqMc_uhniYb63ndnAvUzDzYssWxCLm1hXwDj5J5xuTDgf-)

10\. Confirm the entire setup and explore the below commands\
sudo vgdisplay -v #view complete setup - VG, PV, and LV

Sudo lsblk

![](https://lh4.googleusercontent.com/54RM2YeyjVJNbpGSQzdJOAvQ_w4RIEuvBtKKZv5OUknX-6ooSvOk-MXv1tMDtShLkZlQNHyzjsxXZ8qarYBkMVC6orEKdjF-m4Wts5xjGYK3OoCTr1_6d_O4JKkCbk189_31k-Zu)

![](https://lh6.googleusercontent.com/nQdIMMAQbv1fmHziNvqKVo27ajEF7HM9cIWibukufBGUD_d4n-xr7hHZUy5qtGb2MqgpNgDVHs2lN9jGMC637sr6FnrcacIil3yJiTyLAaqC5lF5lEsVjopCX5GueHLTu1h9jwq2)

![](https://lh6.googleusercontent.com/CdA5NeI8gMwEm8vEKzcCtYVYa05tyihtAw1syBKJXCqtWCNYQYuJVB5kHDpYX9qHMazA-PZcyG7Q08F5_Wfd3f0fbpvXdu1TKNG_Ms_Ps454I8LvkX4vL4bIXoCNR91ZGrQONnFD)

![](https://lh4.googleusercontent.com/AT6v7ojXVBRJb3rwlx8eQHu3nY5t7NcX8MHoGpJjMkxsbRarIKBR3xlp_npJQo9X4zxJ_pv8RMwx233kDZvqEMjp9EQx73hVi9rQIc3AxtexEZ1i4u6hLNhinKnwph__cvA2zrL3)

sudo lvs #view logical volume summary

![](https://lh5.googleusercontent.com/i83MIXXYL56-P8jS1SjjAkg7k7oFs7u3XnuU6qJURFOIow0Zc1eoqZ3W2hqa8lF3cIxpJLgDn7pFMPKGWjZjMX1yAb0cfmkZrvL0q8svlSA9kiflFzRvnobjJyn7hXk0YIjtYUAW)

11\.  Use mkfs.ext4 to format the logical volume with Ext4 filesystem

sudo mkfs -t ext4 /dev/dbdata-vg/db-lv

sudo mkfs -t ext4 /dev/dbdata-vg/logs-lv

![](https://lh3.googleusercontent.com/AAM6AvQ2aHPyCvuUP1r232Y7YzhovBgvEF-y8jGZRcvf6mKp3w__qzhQqmMN5An9KL-9WDqDXNZQsGto7JYmBoJYLCKvXazvcp8ru0aZ5fWeQiOvSUoDiGZlxaaEoenhK8MHYJ9Z)

12\. Create /db directory to store database files

sudo mkdir /db    

13\. Create /home/recovery/logs to store backup of log data

sudo mkdir -p /home/recovery/logs  

![](https://lh5.googleusercontent.com/mhOECCmJ4Br6ujbIpmsIvpIRUlAbY5KEDM8olQq_tCD5qr5WM8-e6Hs9uKhXjQ6EvWjj687GBRZqjwQ83suIQ3vGcNbAf7HiC-OtpVsCnL3cuBqM4jLx4u0ighYgQBNY6JZ7avSB)

14\. Mount /db on db-lv logical volume

sudo mount /dev/dbdata-vg/db-lv /db/

![](https://lh6.googleusercontent.com/fndwvEfxAuEEsoGPiOlHXNTAv-Srzy1YpRTzInKi-V8ywhBeQ9_J4gxzrbPHIA6KkBjnckwQraA4FgrjiebwBxFew1yQh1dIU712KnFt4JMioF4S203EiDKAiLQW_2PBqJOcvmDR)

15\. Use rsync  utility to backup all the files in the log directory /var/log into /home/recovery/logs (This    

      is required before mounting the file system)

     Rsync can duplicate data between two directories whether the data is 

     collocated on the same computer or over the network between two 

     Computers. The benefit of Rsync is the ability to update the mirrored 

     backup with computed changes from the data source with a minimal load to 

     system resources.

sudo rsync -av /var/log /home/recovery/logs

![](https://lh6.googleusercontent.com/YbussoGRNAbwXute4A_uCKj5GQghgR8KPqUp4XOkgRM_vLxU3fxOsYVXF1po_vJ-5E_o5eWxHcDaKLk3y6Y-J6slwvNvryosXEExqiGXuRxiw17Vc4_ev4dxVEkcCJElZ3CGS3t0)

16\. Mount /var/log on logs-lv logical volume. (Note that all the existing data on /var/log will be 

      deleted. That is why the rsync step above is very crucial)

sudo mount /dev/dbdata-vg/logs-lv  /var/log  

17\. Restore log files back into /var/log directory.

sudo rsync -av /home/recovery/logs/log/ /var/log  ![](https://lh3.googleusercontent.com/0LIfRRDQKyHRo8d1qpsZiGiU-Cr14klsToFPNMK_KP6qnTg4eJVRx04LgLQUrujCPbpan1xht9FNtK4rzNz3bu6cfuQSeHyZd6yPn9r2OMb1CIhXoEaG2bO-e8TQnDnO5ezzuqcX)

To list your drives and partitions run:  $ sudo fdisk -l 

15 . Update  /etc/fstab file so that the mount configuration will persist upon restart of the server.

sudo vi /etc/fstab

To check the content of the /etc/fstab file:  sudo cat /etc/fstab    

![](https://lh3.googleusercontent.com/ETFNpd1ZjrSlUON4rGvAWP7k204usifjne2Jc0Cbh2qjReMERttggm6ZWWYO4ehk4aCUh64alrIGURbyWju7eqLzWfUHyJPodJ4-hjvyHl9p2UM64eaK-MvcGX9EOqsVTPDpPvVd)

16\. To check id (UUID) of the mounted logical volumes (LV) which will be used to 

      update the /etc/fstab file, run: 

 $ sudo blkid

![](https://lh5.googleusercontent.com/_lCIN8h0k_DjnHlt9EUx3M-DwmDOc9Jq3t4nHFJXGOcwz1m-2mBvCGFi1ht1BbyiTgSTLqmH2c4VzHvZaPB2ZhfWRyvHMnmo9c-JT6h_dLzd67oKH94abzr4RSNMXmVJ-ACrQfse)

17\. update the /etc/fstab file:  vi /etc/fstab

Post-update: 

Paste the below inside the /etc/fstab file

UUID=74da8667-a175-43b4-a0ee-95bc378696c0 /db ext4 defaults,nofail 0 0

UUID=9ef7b7d1-74d7-4e94-b5b1-dc90e423fae7 /var/log ext4 defaults,nofail 0 0 

![](https://lh3.googleusercontent.com/D4KfNqZTF59hqGHNRghZpakACfBoUzAeHmZ9k2l_bHUlC4GwDS0vrfhnS8u3jjcFSep0ET0-HoBilka_c1m74mcrSMrQ_qHmTzomP_HL2QE4kyc8RLToz4nNtsVO2lxjmCUG2l0e)

18\. To update system, run: sudo systemctl daemon-reload

![](https://lh3.googleusercontent.com/YY9Afs0H_86ykmwfPcqlgAWHAzzHGWNs1s14tdXYmwa1A1Gtwx5OfRKXgwIfjV3odpenXkXvvSD_jCX-qfzZWRpUbfAh4Eq4h1wPd0CV0pC3NhJI4Ps3WkB8wlm8lOW5tuRXXX2K)

19\. Verify with df -h

![](https://lh5.googleusercontent.com/GpFMivNhNkWnT5x3IXf-g4DG2QCiCChr1svqf3Vb7GksKlBCUXvKAaYgkW4sZzJygKrtztSEX8xHG9bcstunOJf3uNcXpU579SuchctjX57aF3yfh3fSUJBkWM5ejHjYxTYjy6I1)

Step 3 --- Install MySQL on your DB Server EC2
--------------------------------------------

sudo yum update

sudo yum install mysql-server

Verify that the service is up and running by using sudo systemctl status mysqld, if it is not running, restart the service and enable it so it will be running even after reboot:

sudo systemctl restart mysqld

sudo systemctl enable mysqld

![](https://lh4.googleusercontent.com/fn75UlqGvR1GWJ-3Ijr5c8Fuaf18FpcVi-crfiUrDPbB6TTm2UPEk7wgXnnZDr3oKKs8-EfPF0D6MNZHQqzq1qrXYLryF0ti_-KAbIv0pxzFWTBaFceJ84PJohGCWz1lB8vA_nrw)

Step 4 --- Configure DB to work with WordPress

sudo mysql

![](https://lh4.googleusercontent.com/bGpBHIdf8-rfeLN_Yc7Phje5_kyJ7Ye1uPUW90pgeZu_4Ynw4hh8kO_3mut8vM7H23ogRITGERwkUqmnLfCkcFW8Re1AZfMeIejOW6T6yjq1d7d6Wa9ipV3dsLDbmpC6CtAV9PHB)

1.  Create a new database that WordPress can control

CREATE DATABASE wordpress;

1.  create a new MySQL user account that we will use exclusively to operate on WordPress's new database.

CREATE USER 'newuser'@'%' IDENTIFIED WITH mysql_native_password BY 'password';

CREATE USER 'Bims'@'%' IDENTIFIED WITH mysql_native_password BY 'Gold123';

GRANT ALL PRIVILEGES ON * . * TO 'Bims'@'%' ;

CREATE USER 'wordpress''@'%' IDENTIFIED WITH mysql_native_password BY 'wordpress';

GRANT ALL PRIVILEGES ON * . * TO 'wordpress'@'%' ;

![](https://lh6.googleusercontent.com/Mm-eC-giTSAXRuA0H6RGKzrPbxdQIRWX7frBucxhQUoebDqS9vOQTw2DqmKBRDbbgQ75nOUlw88olPfNFMEed1N6ZXyx25o6SA7FAQTT-4NVQ3m0ImiwB1NHEYHzSfRcApONa_-X)

1.  FLUSH PRIVILEGES;

2.  SHOW DATABASES;

3.  Confirm user:  select user, host from mysql.user;

4.  exit

![](https://lh6.googleusercontent.com/zqUijQZnBDI9pkCPQ_7WKaiPSAsQbJ75JET3QNKKb6FatspIn8QyzeokMBA-RNIVtM2k2va1cnFbsCb4ooQf3eWzb7svcJGy5cJLn8YDndVy1FNQ9Ih_kxIiI-DQeXcCTyk6uV_n)

Step 5: Open Firewall

Check firewall status: sudo systemctl status firewalld

![](https://lh5.googleusercontent.com/C2inlZWXLLYekzJjRvSfZyArClNLBMW4Yoc7W3yLEHB7Cd1W09xXyW3Y2suwWQR15O75yUUpEKjZQeDmpmegSxo43pSCiotg15cA8zAVQmnggubxCSpcQtx9vrFdFfiRFWFNebEU)

Could not find firewall service

Correction, install firewall: sudo yum install firewalld

Start the service for the meantime:    sudo systemctl start firewalld

enable the service to auto-start at boot time:    sudo systemctl enable firewalld

View service status:   sudo systemctl status firewalld

![](https://lh3.googleusercontent.com/AmUaa1cUKXnonuj3-6JAlZ0S2WQEjBUSScFodlLBJQ25VGCofsCLK3GXJCe-S9Ew-btHexCydY4N4430xvT9Hm-mBWSMXLC4CsB13b-KU1CnYGuzbQ-XKbMrHwPHhXi_7i2-Y0Nl)

![](https://lh4.googleusercontent.com/t_qNTqwQ8EFx1jzjrI10WpNa-VA6WY0X2xppPzJzhQ1WDRKaQeNIwPFJNtQMZhAwwKu-XqLTGcJhg4lVRhMDVpYKq5mMN8dhHKxw55axnd59ahO889TcKCWAxQRFjNc_rBXyWRIM)

check whether the daemon is running or not:  sudo firewall-cmd --state

![](https://lh3.googleusercontent.com/vV7YdTY8i692xUT6LLFBXFD912j1ckhjK-9m_I6d_yhErxPNAjko4Eeg-UGnW4Q_uLnz8-5c0NvjdQyCKDO4tn8PIk7Gq11adVltbRtnK8y318EduZ5knmAx4DUb2s1V5HDebyyj)

Set the zone and Ports

sudo firewall-cmd --get-zones

sudo firewall-cmd --zone=public --permanent --add-port=80/tcp --add-port=443/tcp 

sudo firewall-cmd --zone=public --add-port=3306/tcp --permanent

             sudo firewall-cmd --reload

![](https://lh5.googleusercontent.com/AeRtyRIGNVfUDXITCZfKtcaC1stutlGakGPXp7L5b0VJM2EtH3j2vMpUkgpffHgR60CJWcwe3jTaoYoBfZGHzgfCjf7oyR1fNKbiFmoAq7A8Rq0kwLfajS34JxSfibm-iu8rRH7H)

Note that adding the option --permanent sets the configuration permanently (or enables querying of information from the permanent configuration environment).

To find more information about a particular zone, i.e everything added or enabled in it, use one of these commands:

sudo firewall-cmd --info-zone public

![](https://lh6.googleusercontent.com/rTDL--SoUNQaxxzVz6W8bHnOL0MLqQtVcl4fT4wg9GXmGAnLFjSWOeE8FEdxO1q4LZDIBTg7kvbKxoimpZYDiqE4U_ZBil_nq8QYi5g6LvdUyNI9oK9VS4R5R3bdHLsnMFReVp4g)

Step 6: Configure MySQL File on the Database server

: 

First check the location of the mysql configuration file: mysql --help | grep "Default options" -A 1

![](https://lh4.googleusercontent.com/d8UbwyxlYn4KQwXHtBI-Fm4gYrOqUoJGyXN60pjo4zDAoxvOtLx3tFqAOM_hMhuaOzIr3puEqpd2MnNY_7J2de9cUENvjGo_fExNxbN3kDblvaBpmeAsv5wflD0hiY5KroUvivf7)

Then configure mysql file: sudo vi /etc/my.cnf

        Add into file: bind-address = 0.0.0.0

![](https://lh4.googleusercontent.com/LW7Vt_LdLDglVCs7ryBEXexj8QEEzgBNmmQL2-lCQ_VA-ajN36-C8Na6_rPQDJgDPSWPHPG-3jHsGrvJv5h10BXznsVzIbDwH3OQ0ep1FLJz6HZqBrcaq5jAwuXwR75ahNu_23ex)

To check current bind address:  Msql> SHOW GLOBAL VARIABLES like 'bind_address';

![](https://lh3.googleusercontent.com/TS622CROGm97bx30gYfNuCUgiQucVVOfhEMKl-Qefo2szDj2fpwg3gr7v_tQOZi9K1IHkFHG1M2ZAMiiByHEUp_jEvklhBvg5CvTbJS71hEfvb_khgK417TmbjFhBmBur1Kuo62w)

To verify the change:  netstat -nat | grep LISTEN

![](https://lh6.googleusercontent.com/X6clpXzD7PTEwhSJmqhEzr5Wn-OinVAAqhz46HcZaeqrGuzcl8Q7S3k76rPQSnTqwpCFB7sWu5okRdmp2D3JFt9dgi6-z5hiIsVfXEwENhxOzDuM0o11kRBBotQHRjtc5mjNCOxB)

Step 7 --- Install Wordpress on the webserver
-------------------------------------------

Pre-requisite: Install LAMP stack on CentOS 7. This will enable the ser to host dynamic websites and web apps. It can be installed using 'yum' - a CentOS package manager that allows installation of several softwares.

1\.  Install  Apache

1.  Install Apache: A webserver that host websites. Run:  $sudo yum install httpd  

2.  Start Apache service: sudo systemctl start httpd.service

3.  Verify Apache installation on your browser: [http://your_server_IP_address/](http://your_server_ip_address/).        

    http://18.133.33.28/

![](https://lh4.googleusercontent.com/5knZ6dn26FgYEmqg5dVa4Roy6gJT0FJowZTjBI6SCAAIRhiWSkbVfscoW1DqA8ZS5nSahSiszcXK70QCJNKmNaL0nYQMCFRjyJ6QwijE9V751gRIPu-BiGwW8vm6TjSGD0gTJF-d)          

1.  Enable Apache to start on boot:  sudo systemctl enable httpd.service

2\.  Install MySQL:  MySQL will organize and provide access to databases where our site can 

store information.

1.  Install MySQL:  sudo yum install mysql-server

2.  Start my MySQL:  sudo systemctl start mysqld

3.  Check mysql status:  sudo systemctl status mysqld   

![](https://lh6.googleusercontent.com/9CR6GtGYinW6GQXjD6wdMm3oa9F0yznpBwSKhlq9-Wwo1yGBUGNkn1XoqWxVoRdpuI-YAI-MN5A-wyDi6SdPjfgOwYtKcQy0dJwzmnIGBSfLVk3KfLWAKe2Co0triiKBgwacdf8W)

MySQL is running

1.  Show Databases: Show Databases;

![](https://lh6.googleusercontent.com/kzMAz_rQiw5fqcgiJsnUHo6O6uiYKljXUtwc-EhNa1Xx66BfDNU0QTzM-CJOodX38Bm2Yt0XwBe8kHHeJvDiGPDjwude_jKFZt-0UodItKpdx1f8H8lBGfu5yAtCI5ICdc64Cprg)

1.  Run a security script to remove some dangerous defaults: sudo mysql_secure_installation

![](https://lh4.googleusercontent.com/fjZ2WzISIRMQFZBG884QkveVXWjS_IxQ2eCVApzUdlE4YOMQPo-HfIcJVzbyZ3XxG7BuNmjzeWn_mhnGpBwh9WPnN_4JHAsKjkfkDvivWWQBSXyG5tZuls52R83ovvziZT_hqVEg)

1.  Establish connection to database server with IP address - 172.31.38.26 

sudo mysql -h 172.31.38.26 -u Bims -p

![](https://lh5.googleusercontent.com/1ga4r4bx051yNt-RsK3YfnMrbyO18UgNXAcqWZGY_EGGS1alDz_mKCu9RiRPQmcPbGPa-8aAGKsCWJu6339B2rgN4sJ0R0p7tsNQC5wTttvxpdsbsE7BCXjeN4ACs5hii9NSsUMU)

1.  Establish connection to database server with IP address - 172.31.38.26 

sudo mysql -h 172.31.38.26 -u wordpress -p

![](https://lh5.googleusercontent.com/6tySwPBObF2oZUfI1ug8Qr5KvcJAVYy-QRx_QsF5Gvs5x0xP07e74Q3NUBldkhZimdvkK4PXxA7pk-u2XoMKu6yNFe89_enlR_iBW14xKb2gEIkJRBlYfrL2MkM-vfq-RH1JFAkP)

3\.  Install PHP

 Install PHP:  PHP is a scripting web programming language used for developing dynamic web pages.

         This is the component that will process code to display dynamic content. It can 

                      run scripts, connect to our MySQL databases to get information, and hand the 

                       processed content over to our web server to display.

I initially installed the old version of PHP which did not allow the wordpress site function well. So I ran the error log: sudo tail -f /var/log/httpd/error_logand sudo tail -f /var/log/httpd/access_log commands to identify that the issue was with the PHP version.

The latest version is the PHP 7.4.

1.  Install the EPEL repository: 

sudo dnf install <https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm>

1.  Install [yum utils](https://www.tecmint.com/linux-yum-package-management-with-yum-utils/) and enable remi-repository

sudo dnf install dnf-utils <http://rpms.remirepo.net/enterprise/remi-release-8.rpm>

1.  search for the PHP modules which are available for download eg - the module, stream and installation profile.

sudo dnf module list php

1.  The output indicates that the currently installed version of PHP is PHP 7.2. To install the newer release, PHP 7.4, reset the PHP modules.

sudo dnf module reset php

1.  Having reset the PHP modules, enable the PHP 7.4 module by running

sudo dnf module enable php:remi-7.4

1.  Finally, install PHP, PHP-FPM (FastCGI Process Manager) and associated PHP modules

sudo dnf install php php-opcache php-gd php-curl php-mysqlnd

1.  Start and enable PHP-FPM on boot-up: 

sudo systemctl start php-fpm

             sudo systemctl enable php-fpm

1.  Check the status: sudo systemctl status php-fpm

To instruct SELinux to allow Apache to execute the PHP code via PHP-FPM run.

setsebool -P httpd_execmem 1

Finally, restart Apache web server for PHP to work with Apache web server.

sudo systemctl restart httpd

Install php: sudo yum install php-mysqlnd 

![](https://lh6.googleusercontent.com/Xv8fk86G9PdSyGNG5AeBjh8zRN_CtIsLuesM5PWtu8gup_pdZhzYkrqOGM3_Cx0W92zwwG0BfTx_WzMmuvAArSGBoPEvtKULy9yoaVJXUR1JmN_FOmKWMIOZCM11kvUY6fFGH4cj)

4\.  Test PHP Processing on your web browser: 

In order to test that our system is configured properly for PHP, we can create a very basic PHP script.

We will call this script info.php. In order for Apache to find the file and serve it correctly, it must be saved to a

 specific directory, which is called the "web root".

To test PHP with the web server, you'll have to create an info.php file to the document root directory. In CentOS 7, this directory is located at /var/www/html/. We can create the file at that location by typing:

vi /var/www/html/info.php

This will open a blank file. We want to put the following text, which is valid PHP code, inside the file. Insert the PHP code below and save the file.

<?php

 phpinfo ();

?>

Test whether our web server can correctly display content generated by a PHP script by typing below in the browser.

http://your_server_IP_address/info.php

http://3.8.188.219/info.php

![](https://lh4.googleusercontent.com/44nhc8cdfdn2f4jGcrvPXUbs1-6GOfcTvD65dLOSxgnfToKtrmIQ-RFf8ZKTuYxUgFpZOMjuZNMdhhpb_Xbu82PgWRen9RdEuQbUHHJzGnvd3QY7he6XO-y8Hws-hp0Zfob6QLRD)

Remove this file for security reasons: sudo rm /var/www/html/info.php

LAMP stack has been installed successfully. This will allow installation of several websites and web 

software on our server.

LAMP is now installed on Webserver.

Install additional PHP extension as WordPress and many of its plugins leverage additional 

PHP extensions.

sudo yum install php-curl php-gd php-mbstring php-xml php-xmlrpc php-soap php-intl php-zip

Double check the connection to the database server: with IP address - 172.31.38.26

Log in to mysql:  sudo mysql

sudo mysql -h 172.31.38.26 -u Bims -p

![](https://lh6.googleusercontent.com/IssK_xZ_XrFl5ubmqZ7lxlmJvCg4CDHHh5wvZ2c0XKPzBsv_qkDS0aU-ZPcg9fSJY1BDUd3Mh8asOMTDkOWvaL-5ix7wjalFLnla_sqrC-__08ky2bB0YZ7l7wzqQ6YwukzjdmTz)

sudo mysql -h 172.31.38.26 -u wordpress -p

![](https://lh5.googleusercontent.com/6tySwPBObF2oZUfI1ug8Qr5KvcJAVYy-QRx_QsF5Gvs5x0xP07e74Q3NUBldkhZimdvkK4PXxA7pk-u2XoMKu6yNFe89_enlR_iBW14xKb2gEIkJRBlYfrL2MkM-vfq-RH1JFAkP)

Show databases;

![](https://lh5.googleusercontent.com/GEpcZ_CRcPLuNpBXQKFrJw75iTJeDeDLnhv9ZFX21jU9DC_Nspqc0tn5dStYYs_9sDea0scQPCEBMoAI_5ewLkFpyM80wKMrnZWu2ZWjP20oVFsyY5_L-Ho9lN5r2tVQ62YM7mtp)

Setting up the Webserver:

When installing our LAMP stack we only installed a minimal version of PHP that allows basic communication between MySQL, Apache and PHP. Before we proceed we will install an additional extension of PHP that will enable wordpress to function because wordpress itself was built on php.

sudo yum update

sudo yum install php-curl php-gd php-mbstring php-xml php-xmlrpc php-soap php-intl 

Php-zip php-mysql

![](https://lh3.googleusercontent.com/4nVwwh9WMeU00q8S1N9xF_qzsYi9XaRKXodGhztuuoFUXIMZ1nxCZ5x8wKOFfXQVMkrcKb106mMBgkD2giLmOHpexKtU9zGmPMfko2MiVGDdx5Nv931-o3Ky-q5vL9rAuO6bqetK)

Error: Unable to find a match: php-mysql

Correction:  used php-mysqlnd

sudo yum install php-curl php-gd php-mbstring php-xml php-xmlrpc php-soap php-intl 

Php-zip php-mysqlnd

![](https://lh4.googleusercontent.com/FNgTVv9okz8C4OTKqAaWSp3-P5KD7rSnLvQee3CHRLAGoL8flBX6amD2T1efBce1X6z1C-G5ULFkGVYnKZVEpq7fGpHB8sezqCBj9BIpCc-yd2Mf3W5SzZ9r0ZIDDkPY1tKGfL1Q)

For the new extension to be used we need to restart httpd

sudo systemctl restart httpd

 Open Firewall

Install firewall: sudo yum install firewalld

Start the service for the meantime:    sudo systemctl start firewalld

enable the service to auto-start at boot time:    sudo systemctl enable firewalld

View service status:   sudo systemctl status firewalld

Allow httpd

sudo firewall-cmd --permanent --add-service=http

sudo firewall-cmd --permanent --add-service=https

sudo systemctl restart firewalld

![](https://lh5.googleusercontent.com/rjzIhSEKn9tzANVv0Pbl-dQwwrRJH4jvDR5B4SB4_9Egx2ivnmlSQFnEr45oxrS_nHjn5Rxpm0lJ-5sunoEJRrKblY81SxF3ckoqD5o96mCjI50S93El5jHf-LSkQc_Ct1tqoItX)

![](https://lh3.googleusercontent.com/a204KU_H2eFt27fuK698wPMhMjesldGLcE342_dCtU8XGLvzQjJ73n1gesRDYyU39ZwhRVnI0m4Fn2SwUC52E30NEMnN4-FZfv4Xu6GNYtshpfx2enxnkzIfRTIznoGgUyVNYGBO)

Set the zone and Ports

sudo firewall-cmd --get-zones

sudo firewall-cmd --zone=public --permanent --add-port=80/tcp --add-port=443/tcp 

sudo firewall-cmd --zone=public --add-port=3306/tcp --permanent

             sudo firewall-cmd --reload

![](https://lh4.googleusercontent.com/Paf43viDBr23I4n1_RYilI53gv8RPVw86U9hBo37XEswi_aH18u-yC66wvrZzS-yU2hr_99PmnfO1Hq3pKkfn1LpkSQstqqoEkvZ4RLPxw3NBz34TkvIQvri19_QivEmu2HcSqSN)

![](https://lh5.googleusercontent.com/rjzIhSEKn9tzANVv0Pbl-dQwwrRJH4jvDR5B4SB4_9Egx2ivnmlSQFnEr45oxrS_nHjn5Rxpm0lJ-5sunoEJRrKblY81SxF3ckoqD5o96mCjI50S93El5jHf-LSkQc_Ct1tqoItX)

Adjust security in AWS

All traffic  all all anywhere

ssh tcp 22 anywhere

mysql Aurora TCP 3306  anywhere

![](https://lh5.googleusercontent.com/UKCtoXPfNoqPlyk0Y2_MJo6ws_Yvj9h31tqn03R0LbLk9-AWalzUiIKCUTunGarcSUiCJiO_hYMkq5LsaaCLsU4WRpvwnVqsMXGlnOsN98HO1ORdkwdzz4vdF1bWABsPL1LEOwwA)

Setting up the Environment:

Create a separate directory for the wordpress website

sudo mkdir /var/www/wordpress

sudo chown -R $USER:$USER  /var/www/wordpress

sudo chmod -R 755 /var/www

![](https://lh6.googleusercontent.com/9PMIEfAipSoDyAOyEW5-58ZkenpITGUKEJkoowKj0_BQFlUCfSXQBucCjjcZkQd9EeNkRWrSDyhodYLyqPEvwrpc24ako6L76fBR-aPs-fDAnn-1ayLJV9i9giG0Rx9TScktl_vF)

sudo mkdir /etc/httpd/sites-available /etc/httpd/sites-enabled

![](https://lh4.googleusercontent.com/9AufZ3BnoGR2XpyATiBMMpEpz2Esfqq2HfpSFfUpEmGcfi8Td4lLcl_fdLymGwLTHxFUMeLfCWMdAhvFehd2Tq5M4m688taRO-yZ1QHNLQj2Aegc_3Jd6x8UYsUVrrY6J9zzmx3Z)

![](https://lh3.googleusercontent.com/Ltz0-qONcJQOi4BnEb5YrqAC_XeFpgFMfLRqgYu3_oq0kQqTbsoilYZFU9i3EIh5A5n8hX3HTxe0RXtaNhRxIJvytRhLBW46GQ2iNNqBSesZAZX8H-RC-yrHhZPPHLMieLMzmCGo)

Next, you will tell Apache to look for virtual hosts in the sites-enabled directory. To accomplish this, edit Apache's main configuration file and add a line declaring an optional directory for additional configuration files:

sudo vi /etc/httpd/conf/httpd.conf

IncludeOptional sites-enabled/*.conf

![](https://lh3.googleusercontent.com/Xwh0jndS8By9irTKxW5KLlXSF-cfBxGsjjaeaAUKq4PQXwerc9UW20273xL3HExV7HD2buD_fvt3UmPwaI52t6AGEl1_VlQqc6-W75dTOIuFgZgp12YRxWIfI-0vytSAQsPX6Zpl)

Creating Configuration file for wordpress in /etc/httpd/sites-available/

Note: in our configuration file we will enable .htaccess file, WordPress and many WordPress plugins use these files extensively for in-directory tweaks to the web server's behavior.

sudo vi  /etc/httpd/sites-available/html.conf

<VirtualHost *:80>

    ServerName wordpress

    ServerAlias www.wordpress

    ServerAdmin webmaster@localhost

    DocumentRoot /var/www/wordpress

    ErrorLog ${APACHE_LOG_DIR}/error.log

    CustomLog ${APACHE_LOG_DIR}/access.log combined

    <Directory /var/www/wordpress>

    AllowOverride All

    </Directory>

</VirtualHost>

![](https://lh6.googleusercontent.com/zQi2oTE0CgvPx7d3gy1bD03va4-fvu40AicM_ZXuDupps8u3-yxEnzkKzZCZdlzm73U2MBTjRgo4wZ9bBhKIN8FBhDsxupIunsTYexOE5YTFhOq95iIWBQss2FKs2SmyDLjrL9KX)

Link sites-available with sites-enabled for wordpress

sudo ln -s /etc/httpd/sites-available/wordpress.conf /etc/httpd/sites-enabled/wordpress.conf

If you want httpd to determine your server name, you can add your servername in the main httpd configuration file

sudo vi /etc/httpd/conf/httpd.conf

Add wordpress

Restart apache to update all the configuration added

sudo systemctl restart httpd

Downloading and  installing  wordpress

Now, the apache web server is setup and configured, we will download the latest wordpress from their website for security reasons. The wordpress zipped file will be downloaded in the /wordpress folder, this is because we don't want to keep the file for long after installation.

Change into directory wordpress and download

cd /wordpress

curl -O <https://wordpress.org/latest.tar.gz>

The WordPress installation above downloads a compressed archive file that contains all of the WordPress files that we need.

We can extract the archived files to rebuild the WordPress directory with  tar:

Extract the downloaded file: sudo tar xzvf latest.tar.gz

Create a temporary .htaccess

sudo touch /home/ec2-user/wordpress.htaccess

Also copy over the sample configuration file to the filename that WordPress actually reads

cp /home/ec2-user/wordpress/wp-config-sample.php /home/ec2-user/wordpress/wp-config.php

![](https://lh6.googleusercontent.com/sY3nSfB7v72FJjDPuL3rKPSQZM8qz_1tZVPEwHUL6NrhBW2bh6uUJcKF_NdlrcoJTe3qt1KYJDvY4-oD_C5RnKJ0wPfP-VqjQpq1IOwXzXOU0TQ3cCdy2-h2pX7rlIYahzXw46e5)

Move wordpress: cd wordpress

We need to add a folder for WordPress to store uploaded files. Create the upgrade directory

sudo mkdir wp-content/upgrade

cd ..

ls -l

![](https://lh5.googleusercontent.com/Hqn-LoWL3J8VMgiQJf1jpGL7e4XCm1njFhFi-KI_cG4HMOBdizquu0o_WzxO2p4llslENl9BhVHX3RmKtojlbtMGleNp2vMJuiq9HCOoYvFGbneTYs04ho0ThET096BJvwN_IOXy)

Finish the installation by transferring the unpacked files to Apache's document root, where it can be served to 

visitors on the website. This can be done by transferring  our WordPress files there with rsync, which will preserve

the files' default permissions:

Copy the entire wordpress file into the root directory /var/www/html

sudo cp -R  wordpress/. /var/www/html

cd into /var/www/html and double check the file content:

![](https://lh6.googleusercontent.com/G55_3-SnN3YmLSYLT_kCsw7VFcdXhvX2okxGjI8YVqm8vM_KIeyx26sv7YaaoOMU7QFX3sLsQkxkYeJid4biGao7kMeBlLWlFC0QusIY0yJSUIp0kcSdKDYlGIeNIhRm_TBRVXTQ)

Disable the default apache site:

sudo mv /etc/httpd/conf.d/welcome.conf /etc/httpd/conf.d/welcome.conf_backup

Restart httpd: sudo systemctl restart httpd

![](https://lh3.googleusercontent.com/kOwHfaMYg2R6m_Hs9LgcCfWp8lYNkg8y7GhyIvyMIEiGzs5Dq4RoqtsV5HeJebkZmXQ_raw0qy_K6zuBGMejo5GccYjw0upseOKaDAgHV4KZOQauuQbVgEANoWKC5iBkuoZJQA3Y)

Assign the correct ownership and permissions to our WordPress files and folders. This will increase security while still allowing WordPress to function as intended. To do this, we'll use chown to grant ownership to Apache's user and group:

          sudo chown -R apache:apache /var/www/html

With this change, the web server will be able to create and modify WordPress files, and will also allow us to upload content to the server.

Adjusting selinux policies

The SELinux Policy is the set of rules that guide the SELinux security engine. It defines types of file objects and domains for processes. It uses roles to limit the domains that can be entered, and has user identities to specify the roles that can be attained. In essence, types and domains are equivalent, the difference being that types apply to objects while domains apply to processes.

There are different ways to set policies based on the environment's needs, as SELinux allows you to customize your security level, we can set policy universally and we can set policy on specified file

This will set a universal policy on httpd files

sudo setsebool -P httpd_can_network_connect 1

The setsebool command changes SELinux boolean values. The -P flag will update the boot-time value, making this change persist across reboots. httpd_unified is the boolean that will tell SELinux to treat all Apache processes as the same type, so you enabled it with a value of 1.

Setting  permission on apache directories

sudo semanage fcontext -a -t httpd_sys_content_t "/var/www/wordpress(/.*)?"

sudo restorecon -R -v /var/www/wordpress

sudo semanage fcontext -a -t httpd_log_t "/var/log/httpd/logs(/.*)?"

sudo restorecon -R -v /var/log/httpd/error_log

![](https://lh3.googleusercontent.com/nXpZ_rCmR09T-_qBWbg3Dq-CK3GGiC3wqBfEkZ9SmkJu3xhlPdloJBDVhdxygL2ZSdtx_Mskh5-rawHhoIhM8ptOjI13KJ4lBl44U2ascQROlXKRvseTJujBZknGHZIhGcQ1i3Y6)

Setting up the WordPress Configuration File

There is need to obtain secure key values from wordpress, this adds more security to our installation, to get this keys wordpress has provide a secure generator so we don't have to come up with our own keys

Move into the /var/www/html folder

cd  /var/www/html

To obtain secure values from the WordPress secret key generator, type:

sudo curl -s <https://api.wordpress.org/secret-key/1.1/salt/>

![](https://lh3.googleusercontent.com/ryexlT05OEOqxjXW32KBXms_WUO8K8ESVcDLKf6ibXQ6O2VRpLAr7VXEgydE1MwmhI-skxPPTe9zTmkVhrrysMS_x5aDv515fAob3oZVdrn4jeLIPUpK0wC_lJ9ZZrsCFNl7Hf7O)

Copy the key values into the wordpress config file: sudo vi wp-config.php

define('AUTH_KEY',                             '|(v)2@NM?$n={Z{&$GU`:NanS31tjdle,@uB)+B!5OPS7fV[#Q<b<c.fq!irSG[j');

define('SECURE_AUTH_KEY',        'T>u}XoI87mK%aQz&F:e}J629]./R dLUNq,O&o1P~:-<gs(A*u0P?TJ|lH+/*) 9');

define('LOGGED_IN_KEY',                    '1h#of1N<d%A>^uL:D-Fcy.|-tftQKi_yPCJV=;-SMd9XyP;!(u;f=k+]1m88!:0@');

define('NONCE_KEY',                         'EP-u;?T$$kRQZg3gsa5+ecU>0??|HIJdfa*-NF>&5aj>igQa:PxX75/T+.|-sm<=');

define('AUTH_SALT',                        'UnaQ2X36rY^8vD&/bggX8WL/~#JQ3(-s)dEmFd5-E}e9eWA:|sAijiH`&b-Jc`J^');

define('SECURE_AUTH_SALT',        '_5-.=v]t4CgNx8R=?14J]:N|o;uQT7FEA+H,<solrn4:Gj(H8=x4%A8zI?)LA6Y+');

define('LOGGED_IN_SALT',    '}-A1<;QRr~M6N@~Fn~o6SR~YD[zw[hbUYI0$Qi[ZTJ)Ls)23abZCdLQ? FgU9b3t');

define('NONCE_SALT',                 '%AJ(Hk$I_L7jP_s@t%THgSlCZT|7@e%,khe>n*Jf1j+Y@Y?D6^xzb~pKLny{+o($');

Update database connection string: sudo vi wp-config.php

 The ip address of the databases (172.31.38.26) must be added here, including the username, dbname and password

. . .

define('DB_NAME', 'wordpress');

/** MySQL database username */

define('DB_USER', 'wordpressuser');

/** MySQL database password */

define('DB_PASSWORD', 'wordpress');

And add public IP address of the database

....

define('FS_METHOD', 'direct');

![](https://lh4.googleusercontent.com/WKwyyjUx3wC4bfJ9TiaJUdyl31uAordSGJ5KF-cG19aiWFFH2fqBT2bfYt3Oi4KRbM6fs997g9g42EcMx-Ub6DAvcVP_mJ-m3dV4DjvUyGqm2g7jHxYtq1Ix0FFaFlxeIMAFAp3V)

Save and close the file

Step 4: Complete installation through the web interface

At this stage, the files are all in place and software configured, so the WordPress installation can be completed through the web interface.

In your web browser, navigate to your server's domain name or public IP address:

[http://server_domain_name_or_IP](http://server_domain_name_or_ip)

![](https://lh4.googleusercontent.com/kigV6XlNA2OjQGWl-8et0CgmrrQPnYI9HYr97MrrASdLmyIzisMZkjpbo36tQauuWgJBDPolPAJviRfZrhn54bOc5IaE7_Y55NyVKB8thIK_7smhwXiqupB_t2i2IfdA5dXo1_oZ)

![](https://lh6.googleusercontent.com/7FKJcmTDe56Sc1-9uzthBUkfhJFCU1uzkB1gvIyZ7KtdDTkHc75ZAwlix6U07gOkX55hF4VW4lgQNJRX645EY7nBijPxV0ijgsUZENYR9mNt_Tmf9QrAQ9bRVWpGhY9bTMv03jpM)

Credits

[www.darey.io](http://www.darey.io)

Database Server: <https://www.computerhope.com/jargon/d/database-server.htm>

FTP Server: <https://titanftp.com/2018/09/11/what-is-an-ftp-server/>

NFS Server: <https://searchenterprisedesktop.techtarget.com/definition/Network-File-System>

LogicalVolumeMgt:<https://www.digitalocean.com/community/tutorials/how-to-use-lvm-to-manage-storage-devices-on-ubuntu-16-04> (LVM)

LVM:<https://stuff.mit.edu/afs/athena/project/rhel-doc/5/RHEL-5-manual/Cluster_Logical_Volume_Manager/LV_create.html>

Rsync Restcode:<https://unix.stackexchange.com/questions/330664/restore-directory-to-previous-state> 

Rsync Restore Code:<http://www.yolinux.com/TUTORIALS/Rsync.html>

LVM: <https://linuxhint.com/install_lvm_centos7/>

Rsync: <https://averagelinuxuser.com/backup-and-restore-your-linux-system-with-rsync/>

MySQL: <https://tecadmin.net/install-mysql-on-centos-redhat-and-fedora/>

LAMP:<https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-centos-7#step-three-%E2%80%94-install-php>

Centos Firewall: <https://laptrinhx.com/how-to-fix-firewall-cmd-command-not-found-centos-7-3143063578/>

Firewall: <https://www.tecmint.com/install-configure-firewalld-in-centos-ubuntu/>

<https://www.digitalocean.com/community/tutorials/how-to-install-wordpress-with-lamp-on-ubuntu-18-04>

MariaDB Configuration: <https://codingbee.net/rhce/mariadb-install-and-configure-a-mariadb-server-on-centos-rhel-7>

PHP on CentOS: <https://www.tecmint.com/install-lamp-on-centos-8/>

<https://www.cyberciti.biz/faq/linux-mount-an-lvm-volume-partition-command/>

<https://linuxconfig.org/how-to-move-var-directory-to-another-partition>

<https://www.youtube.com/watch?v=kp-8g0YF0Hw>

<https://www.youtube.com/watch?v=4ZyOadWAE3s>
