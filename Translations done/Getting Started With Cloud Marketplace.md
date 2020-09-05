# Lab: Google Cloud Fundamentals: Getting Started with Cloud Marketplace

---

## Overview
In this lab, you will replicate the operation of using Cloud Marketplace on the Cloud Shell to deploy a LAMP stack on a Compute Engine instance.

| Component | Role |
| ----------- | ----------- |
| Linux | Operating system |
| Apache HTTP Server | Web server |
| MySQL | Relational database |
| PHP | Web application framework |

## Objectives
In this lab, you will learn how to deploy the LAMP stack on your created instance.

## Steps
1. Sign in to Google Cloud Platform using your qwiklabs credentials.

2. Click on the Cloud shell icon to activate the web command line.

![Select the Cloud Shell Icon](https://storage.googleapis.com/practise-test-cloud-shell-icon/personal%20shell%20stuff.png)
- Click Continue

![Continue](https://storage.googleapis.com/practise-test-cloud-shell-icon/Continue.png)

3. Create a virtual machine in the region **us-central1** and zone **us-central1-a** using your name of choice:

```
gcloud compute instances create lampstack-1 --zone=us-central1-a --machine-type=f1-micro \
--subnet=default --network-tier=PREMIUM --tags=http-server --image=debian-10-buster-v20200902 \
--image-project=debian-cloud --boot-disk-size=10GB --boot-disk-type=pd-standard \
--boot-disk-device-name=lampstack-1
```

4. Connect to your instance through ssh:

```
gcloud compute ssh lampstack-1 --zone=us-central1-a
```

5. Install Apache2 and PHP on your instance:

```
sudo apt-get update
sudo apt-get install apache2
sudo apt install php php-cli php-fpm php-json php-pdo php-mysql php-zip php-gd \
php-mbstring php-curl php-xml php-pear php-bcmath
```

6. Start the Apache2 service:

```
sudo service apache2 start
```

7. Test Apache2 and PHP:

```
http://[YOUR_EXTERNAL_IP_ADDRESS]
```
> This will display the Apache2 default page

8. Create a test file in the default web server root at /var/www/html/:

- Change directory to /var/www/html/:

```
cd /var/www/html/
```

- Create a file phpinfo.php in this folder:

```
sudo touch phpinfo.php
```

- Paste some codes into the file:

```
sudo nano phpinfo.php
```

Paste this:

```
<html>
 <head>
  <title>PHP Test</title>
 </head>
 <body>
 <?php echo '<p>Hello World!!!</p>'; ?> 
 </body>
</html>
```

> Press Ctr+O and Enter to save, press Ctrl+X to exit the editor

9. Return to your root directory:

```
cd ~
```

10. Open a new tab using your external IP address and confirm the details on the php info page:

```
http://[YOUR_EXTERNAL_IP_ADDRESS]/phpinfo.php
```

11. Install MySQL on your instance and start MySQL service:

- Obtain Ubuntu updates
```
sudo apt-get update
```

- Install wget command on your VM:

```
sudo apt-get install wget
```

- Add the MSQL APT repo to your system's repo list:

```
wget https://dev.mysql.com/get/mysql-apt-config_0.8.15-1_all.deb
```

- Run the configuration package:

```
sudo dpkg -i mysql-apt-config_0.8.15-1_all.deb
```

- Select the following options accordingly

``` 
1. Select 'MySql Server and Cluster' and press Enter
2. Select 'mysql-5.7' and press Emter
3. Select 'Ok' and press Enter
```

> This would configure mysql-server into your repository list

- Update your repository list using the command:

```
sudo apt-get update
```

12. Install MySQL server:

```
sudo apt-get install mysql-server
```

> During the installation, MySQL will ask you to set a root password. If you miss the chance to set the password while the program is installing, it is very easy to set the password later from within the MySQL shell.

- Start MySQL service
```
sudo service mysql start
```

- Finish up by running the MySQL set up script:

```
sudo mysql_secure_installation
```

> The prompt will ask you for your current root password. Type it in.

> Then the prompt will ask you if you want to change the root password. Go ahead and choose N and move on to the next steps.

> It’s easiest just to say Yes to all the options. At the end, MySQL will reload and implement the new changes.


13. Finish up by restarting Apache2:

```
sudo service apache2 restart
```
