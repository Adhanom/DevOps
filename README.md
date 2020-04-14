# DevOps
This repo has basic steps to setup with DevOps activities. 
You can add or modify on the steps and stages I have used in setting up DevOps environmeent. 

Automation Infrastructure and Design Configuraiton:
---------------------------------------------------
Lets say you have a server for your CI/CD configuration:
     Ubuntu Server:
            * 1TB storage with 16 GB RAM
            * Ubuntu Server with GNUM (versin 18.04 LTS)
            * Jenkins + Gitlab


 Installing your server with Ubuntu:
-----------------------------------
        * Step 1: Download Ubuntu Server LTS version (18.04 installed).
        * Step 2: Create bootable USB drive. Format USB , go to RUFUS webiste and downlaod the setup. Make bootable flash using the setup features by browsing the Ubuntu setup file from your PC local drive
        * Step 3: Insert the USB into the server and reboot the server.Press F12/F11 for booting options and choose "Boot from USB drive" 
        * Step 4: Enter profile information during the installation when promoted, such as name, username, server name and password details. Also, enter Network configuratin details: Address, host, IP and DNS etc. 
                      make sure the information s correct, check if the IP is not being used by another machine(ping before using).
        * Step 5: Finish installation
        * Step 6: Ubuntu will request login details if installatin was successful. Note: by default the Ubuntu Server does come with GUI/Interface. 
                      If you wish to have an interface like windows you need to install it using shell script. However, you must configure network durring the installation inorder to downlaod GNOME interface
        * Step 7: Update and upgrade your ubuntu package using;
                        -    sudo apt-get update              # Fetches the list of available updates
                        -    sudo apt-get upgrade   -y        # Installs some updates
                        -    sudo apt full-upgrade            # Installs full upgrade 
                        -    reboot server when completed
        * Step 8: When complted press Y when prompted "Do you want to continue?"
        * Step 9: To install GNOME use;
                       -   sudo apt-get install taskel -y
                       -   sudo taskel 
                       -   from the list of software packages, choose Ubuntu desktop by using arrow down key--press spacebar to selec it  then hit enter to to selecte OK.
                       -   run sudo taskel install ubuntu-desktop
        * Step 10: If failed after fetching files at 100% reinstall the package the same way( repeat step 9), untill you get Ubuntu login page
        * Step 11: The login screen may freeze because of graphic card driver issues. This is a bug on ubuntu. Using the following instruction to fix it.
                               - Reboot the server
                               - While rebooting keep holding the SHIFT key untill you see the Grub handler with interface--> press E then choose "Ubuntu" and hit enter
                               - Got to the line that starts with Linux ..and write NOMODESET command at the end of the word. Example, if the word is "maybe ubiquity" or "Nvidia graphics" add 
                                "Nvida graphics nomodeset" and save by pressing CTR +X
                               - Reboot the server
                               - Now you should be able to login, but you need to make a permannet change to a file inorder to login without the error for the next tme.
                               - Change the grub configuration so that the Linux kernel will not try to load the graphics driver before the display server.
                               - Open terminal (CTR + ALT + T).
                               - Write sudo gedit /etc/default/grub
                               - You’ll have to use your password to open this file. Once you have the text file opened, look for the line that contains: GRUB_CMDLINE_LINUX_DEFAULT="quiet splash".
                                 Change this line to: GRUB_CMDLINE_LINUX_DEFAULT="quiet splash nomodeset"
                               - Save the file and update the grub so that changes are taken into effect. Use this command: sudo update-grub2

Ip configration change :
-------------------------
You will need to configure the network:
      1. Write sudo /etc/netplan/50-cloud-init.yaml
      2. Got to the line that starts with address and change the IP address. Be careful not to indent anything on the page
      3. Exit using Ctrl + X and Press Y to savee when promoted.
      4. Wirte ifconfig to check Or reboot. 

Install Gitlab using the following steps:
=======================================

Step 1 - Update Repository and Upgrade Packages
	sudo apt update
	sudo apt upgrade -y
	
Step 2 - Install Gitlab Dependencies
	sudo apt install curl openssh-server ca-certificates postfix -y
		
Step 3 - Install GitLab
	wget --content-disposition https://packages.gitlab.com/gitlab/gitlab-ce/packages/ubuntu/bionic/gitlab-ce_12.3.0-ce.0_amd64.deb/download.deb
	
	sudo dpkg -i gitlab-ce_12.3.0-ce.0_amd64.deb //or the version you want, I wanted Gitlab 12.3.0.
		
Step 4 - Configure HTTPS for GitLab
	sudo nano /etc/gitlab/gitlab.rb
	
	Now change the 'external_url' value with your own domain name.
	
	external_url '172.21.10.10'
	
	Now run the 'gitlab-ctl' command below.
	
	sudo gitlab-ctl reconfigure
	
Step 5 - Write the IP on your browser
             Example: http://172.21.10.10
         Change you password when prompted , and login with admin previlege 
         If your gitlab is misbehaving check with sudo gitlab-ctl restart 
Step 14 -  login using Admin privilages, create username and project name on gitlab
Step 15 -  Configire Gtilab by using the Setting 
Step 16 -  Generate SSH on the SSH section of the gitlab browser



UninstallGitlab:
-----------------------------------------
Step 1 - sudo gitlab-ctl stop ->stops gitlab services
Step 2 - sudo gitlab-ctl uninstall -> uninstalls gitlab from host
Step 3 - sudo gitlab-ctl cleanse -> delets all local config associated with gitlab
Step 4 - sudo gitlab-ctl remove-account -> removes accounts
Step 5 - sudo dpkg -r gitlab -> removes debian package 

Changing gitlab IP address:
------------------------------------------
Step 1: Check the Ip address by wrting Ifconfig
Step 2: Write sudo nano /etc/gitlab/gitlab.rb 
Step 3: Change the IP address on the line that starts with "external_url"
Step 4: save the file using Ctrl + X and exit using Ctrl + M 

Changing gitlab password:
-----------------------------------------
 Step 1 - open your terminal or Putty
 Step 2 - $ sudo gitlab-rails console -e production
 Step 3 - $ user = User.where(id: 1).first
 Step 4 - $ user.password = 'secret_pass' // write the new password under the quotes
 Step 5 - $ user.password_confirmation = 'secret_pass' //confirm the new password under the quotes
 Step 6 - $ user.save!

Creating members and projects on Gitlab:
-----------------------------------------
Step 1 - Login with Admin password 
Step 2 - Add SSH public keygen on gitlab's SSH section
        1.Go to "Git Bash" just like cmd. Right click and "Run as Administrator".
	2.Type ssh-keygen
	3.Press enter.
	4.It will ask you to save the key to the specific directory.
	5.Press enter. It will prompt you to type password or enter without password.
	6.The public key will be created to the specific directory.
	7.Now go to the directory and open .ssh folder.
	8.You'll see a file id_rsa.pub. Open it on notepad. Copy all text from it.
	9.Go to https://gitlab.com/profile/keys .
	10.Paste here in the "key" textfield.
	11.Now click on the "Title" below. It will automatically get filled.
	12.Then click "Add key".
Step 3 - Add the key on the box provided and press the Add button
Step 4 - Create a new project, give it a name, then add ignor file from add file button. 
Step 5 - Member should register on the system by themselves  using the login on the home page.
Step 6 - Assign users to thier repective project using the add users feature
Step 7 - If the user forgets his/her password, the Admin should reset the password using the edit details on each page of the user. 
Step 8 - Users logs with a new password created by the Admin, Up on login the user will be promoted to chnage the password on their home page. 
Step 9 - change the password and login with a new password
Step 10 - Search for a project assigned and start working on by clonning, pushing and pulling using git bach from local machine.(See git command for working with project) 


IP is occupied by Appache Command:
------------------------------------
If the port on the Ubuntu server locking Gitlab from operating use the comman below. 

STep 1: type systemctl stop apache2.service (to stop)
Step 2: type systemctl restart apache2.service (to start again)
             


Intalling Jenkins:
==================
Open the link below for more information:
https://wiki.jenkins.io/display/JENKINS/Installing+Jenkins+on+Ubuntu

Jenkins Installation:
------------------------------
            Step 1: Download Jenkins for Ubuntu Server 18.04 
            Step 2: Install Jenkins
                        ** Open the installing Jenkins on Ubuntu 18.04 link below**
                        https://linuxize.com/post/how-to-install-jenkins-on-ubuntu-18-04/ (guided steps for jenkis installtion)
If Jenkins is not working after installation, use the following command to change the port 8080
            Step 3: write sudo nano /etc/default/jenkins on you terminal 
            Step 4: go to the line with HTTP:8080 and change it to 8081 then save it using Ctr +X then close the terminal(ctrl +m)
            Step 5: restart jenkins using, sudo service jenkins restart
            Step 6: go on broweser and type http://ip addree:8081, example 172.21.10.10:8081, 
Durring installation Jenkins generates a password. Use the following command to generate it:
           Step 7: sudo cat /var/lib/jenkins.secrets/initialAdminPassword OR got to the file location on your computer. 
           Step 8: copy and past the password on the Jenkins web. 
Jenkins will ask you if you want to install suggested plugins. Click on the link and start installing the plugins.
          Step 9: give account details, username, password, full name and email.
                    Example
		Username: DevOps_Admin
		Password: Jenkins_2019
		Welcome to Jenkins :) Melkam Sera! 
         Step 10: Configure Jenkins and install required plugin from the Jenkins drop down on the right top of the jenkins page. 
To create a project;
         Step 11: click on the New Item link choose the type of your project, and give it a name. Click OK to finish. 
         Step 12: configure your project accourding to the requirments of your project.  
         Step 13: start building and testing your project by clicking on the build now link on the lift side of the window, if error occured correct them and rebuild the project. 
         

More details: 

https://www.ostechnix.com/how-to-install-microsoft-net-core-sdk-on-linux/ (installing .Net Core on Ubuntu)
https://www.linode.com/docs/development/ci/automate-builds-with-jenkins-on-ubuntu/ (gitlab + Jenkins integration)






  
            

                       


      
                      

                          
       
       

    
