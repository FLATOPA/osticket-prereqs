<p align="center">
<img src="https://i.imgur.com/Clzj7Xs.png" alt="osTicket logo"/>
</p>

<h1>osTicket - Prerequisites and Installation</h1>
This tutorial outlines the prerequisites and installation of the open-source help desk ticketing system osTicket.<br />


<h2>Video Demonstration</h2>

- ### [YouTube: How To Install osTicket with Prerequisites](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Internet Information Services (IIS)

<h2>Operating Systems Used </h2>

- Windows 10</b> (21H2)

<h2>List of Prerequisites</h2>

1. Set up an Azure Virtual Machine (VM) environment (Windows 10 4 vCPUs Recommended)
2. osTicket Installation Files (Download these files on your Azure Virtual Machine)
3. Enable IIS in I.S.S.
4. Install Web Platform Installer
5. Install MySQL and set up username and password
For this tutorial, I set up my username and password as such:
username: flatopa817
password: Promise02
6. Install C++ Redistributable
7. Configure permissions and install osTicket

<h2>Installation Steps</h2>

Install / Enable IIS in Windows WITH CGI and Common HTTP Features

In your VM, go to the Control Panel and head to Programs.
Under Program and Features click on Turn Windows features on or off
Navigate the list and check the box for Internet Information Services
Expand the list for Internet Information Services, navigate to World Wide Web Services then expand that to find Application Development Features, expand that and check the box for CGI.
Before closing, make sure the boxes under Common HTTP Features in World Wide Web Services are checked.
Check these boxes in Turn Windows Features on or off
https://user-images.githubusercontent.com/147654000/274989273-e770403c-5def-4c58-a2ad-24b61a859078.png![image](https://github.com/FLATOPA/osticket-prereqs/assets/122037583/4df77236-4422-48d6-b9ea-6f681daf34da)
To confirm everything is set accordingly, go to your browser in your VM and type in 127.0.0.1, it should load the page to Internet Information Services
https://user-images.githubusercontent.com/147654000/274992169-b6fdbb5f-73c6-4aaf-ac8c-3e9690303d7b.png![image](https://github.com/FLATOPA/osticket-prereqs/assets/122037583/8ec71eb5-0949-4521-b86c-317bddc2f4e1)

Installing Files for osTicket

NOTE: Depending on your VM CPU and Google Drive's virus scanning system, downloads will be rather slow
From the Installation Files, download PHP Manager (PHPManagerForIIS_V1.5.0.msi) and Rewrite Module (rewrite_amd64_en-US.msi)
Create a Folder in your VM's C Drive and name it PHP
https://user-images.githubusercontent.com/147654000/274994673-04098ba9-26d5-4291-9431-7d2fd3200fc4.png![image](https://github.com/FLATOPA/osticket-prereqs/assets/122037583/5e503fe9-2420-4aa5-b2e6-8cdad542bcf1)

From the Installation Files, download the zip file PHP 7.3.8 (php-7.3.8-nts-Win32-VC15-x86.zip) then unzip the contents into PHP folder we've made (C:\ PHP)
NOTE: If a warning sign appears in the downloading icon in your browser, it means the Microsoft Defender Smartscreen in your VM is preventing you from downloading the zip file. If this happens, navigate the file your downloads and click on Keep
https://user-images.githubusercontent.com/147654000/274998978-2be3abda-6e52-44df-b253-ab4006c199cc.png![image](https://github.com/FLATOPA/osticket-prereqs/assets/122037583/e9009b01-69a3-490d-a3c6-2140780f8d65)

From the Installation Files, download and install VC_redist.x86.exe
From the Installation Files, download and install MySQL 5.5.62 (mysql-5.5.62-win32.msi)
After installing, launch the Configuration Wizard
Check Install As Window Service, for this tutorial our Service Name will stay as MySQL
Check Modify Security Settings and for this tutorial we'll set the password as Promise02

Setting up IIS and PHP

Open Internet Information Services (IIS) Manager and run it as Administrator
PHP Manager and URL Rewrite should be found in our IIS Manager due to the PHP Manager and Rewrite Modules files we have downloaded initially
https://user-images.githubusercontent.com/147654000/275006594-8392f072-ba16-43db-898e-aa807c93d4f3.png![image](https://github.com/FLATOPA/osticket-prereqs/assets/122037583/aec14b2c-262d-4788-a9a3-15b69961e46b)

Go to PHP Manager and click on Register new PHP Version, set the directory to the php-cgi file found in the PHP folder we've set in C Drive (C:\PHP)
https://user-images.githubusercontent.com/147654000/275007659-640439ed-5a37-470d-9451-836176f74ff8.png![image](https://github.com/FLATOPA/osticket-prereqs/assets/122037583/26eaee69-e3f2-4ff3-b07a-471c69f927ea)
Optional but Recommened: Refresh the IIS Manager Server by going to Actions and under Manage Server click on Restart

Installing osTicket

From the Installation Files, download and install osTicket v1.15.8.zip
Extract the upload folder from the zip file and copy the folder into the directory C:\inetpub\wwwroot in your VM
https://user-images.githubusercontent.com/147654000/275010103-0f1b83b3-86df-450c-bd22-e9369fcacf0b.png![image](https://github.com/FLATOPA/osticket-prereqs/assets/122037583/7707d44a-8a0e-49b0-9b81-567aed196544)
Rename the upload folder we've copied into wwwroot to osTicket, then reload IIS Manager
In IIS Manager, expand the connection Sites then Default Web Site to click and highlight osTicket. Then, navigate to Browse Folder and click on Browser*.80 (http)
The page for osTicket Installer should now pop up, if it does not, check your directories of your files and folders
https://user-images.githubusercontent.com/147654000/275012197-0e213a5c-e82c-4e1c-b187-b6e53aeb9af2.png![image](https://github.com/FLATOPA/osticket-prereqs/assets/122037583/63f982c0-2055-4cfd-9621-970e7f7a9385)

In IIS Manager, go to osTicket and click on PHP Manager and click on Enable or disable extensions and enable the following extensions
php_imap.dll
php_intl.dll
php_opcache.dll
Refresh osTicket Installer on your browser to see PHP IMAP Extension and Intl Extensions are checked signifying the extensions are installed
After configurations locate the php file ost-sampleconfig.php inside the directory C:\inetpub\wwwroot\osTicket\include\ and rename it to ost-config.php because osTicket Installer needs to interact with this file
Now go to the Properties of the ost-config file and go to the Advanced settings in Security and Disable Inheritance to remove all inheritance permissions from the file (essentially making a "clean" object)
Now add a new Permission, then click on Select a principal and for a new object type "everyone" the click on Check Names to set the Group and click OK. Then check all the boxes on Basic Permissions and then click OK. Now everyone using osTicket should have full permission to use it
Head to osTicket on your browser and click on Continue then set your System Settings and Admin User until you get to Database Settings
For login information, you can set it to a fake email such as "yourname@helper.com", again it is recommended to have a notepad to keep your usernames and passwords on the standby
From the Installation Files, download and install HeidiSQL
Go through basic setup then launch HeidiSQL and create a New Session using the username "flatopa817" and password "Promise02"
https://user-images.githubusercontent.com/147654000/275033089-e4be70d7-c458-4274-95bb-6f53f8ecde98.png![image](https://github.com/FLATOPA/osticket-prereqs/assets/122037583/35b0be2e-5461-4ffb-8c52-c7d03d1b7c13)
You should see this once connected
https://user-images.githubusercontent.com/147654000/275033778-e04f8a37-4c5c-49da-b16b-c882470f2cdb.png![image](https://github.com/FLATOPA/osticket-prereqs/assets/122037583/08780622-9a35-4c41-ad87-cdda56192783)

Create a Database and name it osTicket
Once connected, go back to osTicket Installer type in our username and password into the respected fields in Database Settings
MySQL Database: osTicket
MySQL Username: root
MySQL Password: Password1
Click Install Now, osTicket should now be fully installed on your VM!
https://user-images.githubusercontent.com/147654000/275036584-f48259ba-ba8a-4bce-9cbe-a300903de8b2.png![image](https://github.com/FLATOPA/osticket-prereqs/assets/122037583/1310fbc5-d9d2-480d-a946-6760ec60309e)

Clean Up

Delete the setup folder inside your osTicket folder inside wwwroot (C:\inetpub\wwwroot\osTicket\setup)
Set the permissions of ost-config.php to "read only" (have only the Read and Read and Execute boxes checked)




