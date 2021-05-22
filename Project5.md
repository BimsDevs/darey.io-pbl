

Client/Server Architecture Using A MySQL Relational Database Management System (MySQL RDBMS)
============================================================================================

Client/Server architecture is about dividing up application processing into two or more logically distinct pieces. The database makes up half of the client/server architecture. The database is the "server"; and any application that uses the data is a "client." In many cases, the client and server reside on separate machines; in most cases, the client application is some sort of user-friendly interface to the database. Below is a graphical representation of a simple client/server system.

|

![](https://lh5.googleusercontent.com/ikhvKVO0b3ZzjUwDov5yPS2sqoBb69OA2tYvnbv-dY8eKhwwE-b-4V4IiTex0FPLyXzKuebCqnQ02ogKIKAKA0a08nBnZvoL1xVJqkSwAN8ClkhD4E0zHHAyQCA_vMxTZLbDcajw)![](https://lh4.googleusercontent.com/U4CV-2TNDAZ5qujZ47y-ZKPJ9ZG-FAE-elSyfN4o4m9Q3Qu-EkIDmqVPQ9FZZwwDgb3o5LW2-bD7SO_47rmB4Nfc_Gv24RmTas6m-uXvL0ZlLvfI1rp-ph4ychhEPheYU_O081Zl)

SQL stands for Structured Query Language and its a central component that allows the database use to perform various database operations and manipulate the relational database. MySQL database is one of the in web data that provides database functionality to applications such as data storage on web servers, data retrieval from web servers, data queries, data reports, data security  and the other database functionality provided by RDBMS. 

![](https://lh4.googleusercontent.com/W4O0h_nRlaah4YXe2n-blRgprPqvQ6Epl_x0Zr5OTFR-jwj73zoomYr2Gum-UWuP-cdovr5vjXlJuzQFJLTHm02MJwekEsdPaCa5Mbcwsk5T-z1xTUFl1Mo96zm5IqEEyV9gF7Og)

The relational database management system is a database management system that allows the creation, management and administration of databases. It is based on a relational database model. The most popularly used RDBMS includes: MySQL, Oracle, MangoDB, MariaDB etc and MySQL is the most widely used RDBMS. RDBMSes store data in the form of tables, with most commercial relational database management systems using [Structured Query Language](https://searchsqlserver.techtarget.com/definition/SQL) (SQL) to access the database.

In this project, we will be Implementing a Client Server Architecture using a Database Management System (MySQL).

Install Curl: sudo apt install curl

![](https://lh5.googleusercontent.com/TCLtEEe04Z4XA7uWV38sUPRG7xrzPylPZAs4s9RefO56bbMCUBKhf9PZRP5GC5sYRri1uc_b3qcIXWmNahDZg4hCiPFPjC9j9PCxXjGjJKd5M9hmY5bWzspqTTU7yiqc4bebE0Nd)

Send a request from client (Linux Terminal) with the curl command

$ curl -Iv www.propitixhomes.com

![](https://lh5.googleusercontent.com/jBq6vnRxnhGrFVXaIPdqjy4dK0Ib9xI228DFn3qK6gDL2CXbLYGRt2KHFhzAgwpHLHHUT_pgyyleMBEblzRMZGD8sbTl2M0x_vUejscYJQLSuD0LVjqW9uSWmiVcuRdxOf8cL1Og)

 |

Steps to Implementing a Client Server Architecture using a Database Management System (MySQL).

1\. Configure 2 new Linux servers in Virtualbox

Server A - `mysql server`

Server B - `mysql client`

2.,Install the MySQL software.on both Linux Server and Client. 

sudo apt update

sudo apt install mysql-server

![](https://lh6.googleusercontent.com/OoV9XpDylyJyUKRuHpDgj-3pSKsXOqtCebWJv_aR-NMzG0_Iey3DNq3t5_hl9rCtQdvQTqY5R4kYaRV-RTPVF1ek7leJc_6YSpOjOa7G--0IgWR4w2S9GR4kT7XkYS_GOk6X9ZfZ)

![](https://lh4.googleusercontent.com/H3_wYghrT8OSWrdZtjmbFwnLK7JoddjSQkfzGCmE5n_G9ImDiagZEYJ4qRSSDPdQdAiKfsL8Uk7ffEw5PpSVhGxxWoJ1URUWQeI2qx5VOkFKMle7AUInuEuJ0LeSNAMzyEb6bJKj)

MySQL is an open-source relational database management system. Its name is a combination of "My", the name of co-founder Michael Widenius's daughter, and "SQL", the abbreviation for Structured Query Language.

1.  On mysql client Linux Server, using apt, install the MySQL client software

1.  Using the ip or ifconfig command, figure out the IP address of the mysql server Linux Server.

$ iifconfig;  IP is 192.168.0.36 (Server)

![](https://lh5.googleusercontent.com/JPBqVhg-I6Gem3RYHZlc4K3Ni6suWPwBfIeJGuQJiduSlRGuh-A3wLrrFnit3QxZRHonjwr0AqAo_h4NQkpz_CFSY1MOBKTOiLQoaQZGAb8ksq8b5GM6N13oICB1iEXw4hOHtzAo)

![](https://lh3.googleusercontent.com/bSiO5b5elJg_ygxrHspYRKTIo8Glw76VbkpD27old_3VuvGEzqMmmz8vGDCt1NOpH7DRlsybuWfgVjb2zO29AXdrrP1OXwc6QCDUxQEOn-wZt2AHc6F9-m9vKUbWjL1Z13tObq7H)

$ ifconfig IP is 192.168.0.35 (client)

![](https://lh4.googleusercontent.com/hIz5idDtmgBVHzIXhupVvh6t0mPJrQPn4FMUrMUJDsGX3k7usHcRhQPqWIquTqi_M9vsi1RrqNqNESqryZc6O3TB6UmKzCaD8hxv0G653ie3vdLxMLehjQn_S5I3ogFjpk76Cm7W)

1.  On mysql client Linux Server, connect remotely into the mysql server Database Engine without using SSH. You must use the mysql utility to perform this action.

Server's system:

1.  Login as root and go into the server's MySQL configuration file and change the 'Bind to 0.0.0.0 so as to allow any server to access.

nano /etc/mysql/mysql.conf.d/mysqld.cnf

![](https://lh3.googleusercontent.com/sj6dUm_CsKjbjp45Gzs0Mu9Toubm9YLRvKypU93PrCcBj8-VKphnKTZNUeuTZhQr1W4LpZbJEZ6TQ7GCdO7nS6wwis7mpwu-1UNzsVfJnpBJ2rVTjLCEpISOR8XHXpFXBZUU1o7a)

1.  Check that the server is running:

$ service mysql status

![](https://lh6.googleusercontent.com/_yGd4m7nF9R8hlh_-zBEtdefP2p98fPw4f6ntJCRVVeIWWh-e-vXY-OGKlBGpDQkrarRyb5Wr2cRTunx7yWtD3h1l-MK94yNpdMEOWVZWqepHlKdEgWE0aM2d8Ktku1u8KQ1ZvWI)

1.  Create Users:

Mysql

CREATE USER 'Bims'@'localhost' IDENTIFIED BY 'Gold1234';

1.  Grant access to MySQL

GRANT CREATE, ALTER, DROP, INSERT, UPDATE, DELETE, SELECT, REFERENCES, RELOAD on *.* TO 'james'@'192.168.0.35' WITH GRANT OPTION;

No need to create access for Bims as the user was created on localhost so would automatically have access to MySQL database.

1.  Confirm users in the MySQL database

mysql> select user, host from mysql.user;

![](https://lh5.googleusercontent.com/2jyD_LbDsGhljwG6WoIAF1X5QVetv73JHBituW7UhazKPIKItp4Zuy-AhKtuurr6seBZpopJJRu-SOc6Embt8HO8fb7X1wqexWguLDC7aNFev2uuIKAVXtYeE27NlpMUOIx1oO5m)

f. Create Database: project5

mysql> create database project5;

mysql> flush privileges;

mysql> Show databases;

![](https://lh4.googleusercontent.com/LEJg1mN29RoVRyyNYAYPkf-Ea_M-qsR_tKUceWEj-lAYgD38xyRtWMOU9WwneXMVHwpLmF-1RjRcTYJi5i5a3wp_jRsFDjNxh5p0ImqPBOyVVm3nY7fUQ--xc4wQQtbuvYriX249)

Created an additional user and granted access privilege:

CREATE USER 'John'@'localhost' IDENTIFIED BY 'Silver1234';

GRANT CREATE, ALTER, DROP, INSERT, UPDATE, DELETE, SELECT, REFERENCES, RELOAD on *.* TO 'john'@'192.168.0.35' WITH GRANT OPTION;

![](https://lh5.googleusercontent.com/Tc91gJl5OyL_XleXlxOe_ZS_SLefY-Inj2VyOgF8EAR5FLeGxg9_7zkkaDGYYmgt_ry_freVImfdKHbbrt58Vkd6AgrVTGFPOLwKDLekvnEGsukx6WkcRtNjTFReDHtao5GXiir0)

To confirm the users which have been set up in the Server's system:

mysql> select user, host from mysql.user;

![](https://lh3.googleusercontent.com/pVGueroUWo7TezOaxkUW9zfaZ39qePmjEfLFnW37sodCrt14DYGx4Ut8Vd8gg_dQFbh7cI3oYy45Xv6TfPp1flFPGMYsyEfPPfuHg9IJVapHFIU7eOHEchYzU6AJIs3a2jwhY_AG)

  g. Flush Privileges:

mysql> flush privileges;

  h  Exit from MySQL. Quit

   i   Allow ufw from server's IP to any port 3306.  3306 is the default MySQL port.

:~# sudo ufw allow from 192.168.0.36 to any port 3306

~# sudo ufw allow 3306

![](https://lh5.googleusercontent.com/0lMfWiCCiKYO0jRHmETs0_Tl80WojYCFpVxSvoGx0qJmMzbN8G0EAeZ3dwc2UtgI_OZPN7ETUcZ3fcwLHs8e0PXJrMwv3oFF-U0tu034GMvF0yjHODxBJ1BUee2kmG4YuyTlzVbj)

Client's server:

Check that the server can be remotely accessed by the client:

First check current users on mysql client database:

mysql> select user, host from mysql.user;

![](https://lh6.googleusercontent.com/hGuruArYUKBZA-Lx4QGk_IjjsIXOm-oWkijOerrnRN3ibkgqhwnHIPs8r0rop8iUnyHsp44ZynVQnCSq402VBnCWL5df9u2Ei1kyrR2J8rlz8TKy3CICvaMc2Q6suVhVvKQ3LGsI)

Allow ufw from server's IP to any port 3306.  3306 is the default MySQL port.

~# sudo ufw allow from 192.168.0.35 to any port 3306

~# sudo ufw allow 3306

Then check if the users can access the database from the server.

$ mysql -u james -h 192.168.0.36 -p

$ mysql -u john -h 192.168.0.36 -p

![](https://lh4.googleusercontent.com/nUnPb958FWKfMzIaM0uUUAwXyqUaE_w8f0IkvUZrDLLDRPJBdCtkZy7yMT_ihsIUUaGzJpjcpv4ukBzCPuwBqU2O11wDfDz1IIHKRXwnCmGmTAN5Ta0Ui9Au538hQQbffvT4fQp0)

Error: Cannot connect to MySQL server on 192.168.0.36

Back to the Server terminal to troubleshoot:

   Check the error log for error details:

 # sudo tail -f /var/log/mysql/error.log

![](https://lh6.googleusercontent.com/3DvlhBbnRKGKKFYbEwELPYvgulwtT0MlgNf4tQbuso8CAgTkA51ZKH39d33_WDaQYvb-uu96Vo5BrWBY5s6XD0xrWpBQhE24aYlxrWMS_WwknPxJ-g11VHHGYJbLfkMoFrdxjSBu)

Check firewall status: systemctl status firewallid.service

And  confirm that mysqld is running on the IP:port: netstat -an | grep 3306

![](https://lh4.googleusercontent.com/Xm6njZUFDse-Q5TTRRTsDqACVIedSrzYIOU2ow8vJB4X3blEbW36URBFVYh8Nm72d5VkcPqFMQlvyj6Z_6cr9eU8hK34ec7oejN0JkZwL_DjYaUshANoBwqC9mZOkBZ62aauWNt3)

Double checked the status of MySQL and Ufw and reconfirmed the IP address.

# sudo systemctl status mysql

# sudo ufw status

# hostname -I

![](https://lh5.googleusercontent.com/660dHUDd69-bUoM5sjY-7Yfx8NvcTzlaPHdfHoONgcbPtPK4mWqWPr7tVE9cyBHOEubvdbPYaZuSsVnDXU24NfAMZJNSHZ2JV8lvhs4VdXqHEGbd4trlidU04CCkD9QiMntfVpbJ)

From the screen above, everything seems to be working fine but Client is unable to access the server.

Error Solution: 

Restart MySQL after configuring the MySQL file. Do this on both terminals. 

# sudo systemctl restart mysql

 Back to Client terminal:

Check if users on the client's system can access the project5 database in the server system.

# sudo systemctl restart mysql

# sudo mysql -h 192.168.0.36 -u james -p

Mysql> Show Databases;

![](https://lh6.googleusercontent.com/kVrEKAjO2rf5ChAQ1CgDRM06rwblz_cbi86SgDQ4C7QWHuYNoigBckyB7L4bHUVYs-JNIE_jfP8lZHoNpRt0o9IGRf7tzYLHysE4fGmlISNrj57MhNsJqa1dql2ZRVHWB410pDcE)

# sudo mysql -h 192.168.0.36 -u John -p

Mysql> Show Databases;

![](https://lh4.googleusercontent.com/tTaMg1FVVEfzgahoy7ZzDjmngp6uCrWWo4pho2OQ81IiRVu9w9KJyAH0MoQFVpFD0MFFNY96IfAZCtrwNXL8YipetDy-XvMojDBpdgCbzhm52Oouco5Qn4Gv4vBopnOTDESonC6Z)

Above confirms that the two users - James and John are able to  access the project5 database which was installed  in the server system.

Credits: 

[www.darey.io](http://www.darey.io)

Client/ServerArchitecture:<https://www.oreilly.com/library/view/managing-using/0596002114/ch08s01s01.html>

Client/ServerArch:https://www.digitalocean.com/community/tutorials/ufw-essentials-common-firewall-rules-and-commands

RDBMS:<https://searchdatamanagement.techtarget.com/definition/RDBMS-relational-database-management-system>

RDBMS: <https://www.learncomputerscienceonline.com/mysql/>

MySQLinstallation:<https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-18-04>

RemoteAccess:<https://www.digitalocean.com/community/tutorials/how-to-allow-remote-access-to-mysql>
