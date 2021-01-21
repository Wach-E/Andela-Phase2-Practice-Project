# Lab: Google Cloud Fundamentals: Getting Started with Cloud Marketplace

---

## Overview
In this lab, you will replicate the operation of using Cloud Marketplace on the Cloud Shell to deploy a LAMP stack on a Compute Engine instance.

| Component | Role |
| ----------- | ----------- |
| Linux | Operating system |
| Apache HTTP Server | Web server |
| MySQL Server | Relational Database Server |
| MariaDB | Relational Database |
| PHP | Web application framework |

## Objectives
In this lab, you will learn how to deploy the LAMP stack on your created instance.

## Steps
1. Sign in to Google Cloud Platform using your qwiklabs credentials.

2. Click on the Cloud shell icon to activate the web command line.

![Select the Cloud Shell Icon](https://media.cloudbooklet.com/wp-content/uploads/2019/03/03055011/53650cb2-activate-cloud-shell.png)

- Click Continue

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

5. Install Apache2 on your instance:

```
sudo apt-get update
sudo apt-get install apache2
```

- Test Apache2:

```
http://[YOUR_EXTERNAL_IP_ADDRESS]
```
> This will display the Apache2 default page

6. Install mysql-server by running:

```
sudo apt-get install default-mysql-server
```

- To remove some insecure default settings and lock down access to your database system. Start the interactive script by running:

```
sudo mysql_secure_installation
```

7. The msql-server installed previously comes with MariaDB database. Log in to the MariaDB console by typing:

```
sudo mariadb
```

8. To create a new database, run the following command from your MariaDB console where **wach_db** is the name of the database:

```
CREATE DATABASE wach_db;
```

9. Now you can create a new user (**wach_user**) and grant them full privileges on the custom database you’ve just created. The following command defines this user’s password as **donbliss**, but you should replace this value with a secure password of your own choosing.

```
GRANT ALL ON wach_db.* TO 'wach_user'@'localhost' IDENTIFIED BY 'donbliss' WITH GRANT OPTION;
```

> This will give the wach_user user full privileges over the wach_db database, while preventing this user from creating or modifying other databases on your server.

10. Flush the privileges to ensure that they are saved and available in the current session:

```
FLUSH PRIVILEGES;
```

11. Exit the MariaDB shell:

```
exit
```

12. You can test if the new user has the proper permissions by logging in to the MariaDB console again, this time using the custom user credentials:

```
mariadb -u wach_user -p
```

> Note the -p flag in this command, which will prompt you for the password used when creating the wach_user user

13. After logging in to the MariaDB console, confirm that you have access to the wach_db database:

```
SHOW DATABASES;
```

> Your output should look like this:

```
+--------------------+
| Database           |
+--------------------+
| information_schema |
| wach_db            |
+--------------------+
2 rows in set (0.000 sec)
```

- Exit the MariaDB shell:

```
exit
```

14. Intsall PHP and its dependencies by running:

```
sudo apt install php libapache2-mod-php php-mysql
```

15. We want to tell the web server to prefer PHP files over others, so make Apache look for an index.php file first. To do this, type the following command to open the dir.conf file in a text editor with root privileges:

```
sudo nano /etc/apache2/mods-enabled/dir.conf
```

> It should look like this:

```
<IfModule mod_dir.c>
        DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
</IfModule>
# vim: syntax=apache ts=4 sw=4 sts=4 sr noet
```

- Move the PHP index file to the first position after the DirectoryIndex specification, like this:

```
<IfModule mod_dir.c>
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
# vim: syntax=apache ts=4 sw=4 sts=4 sr noet
```

> Ctrl+O and Enter to save, Ctrl+X to exit

16. Reload Apache’s configuration with:

```
sudo systemctl reload apache2
```

17. You can check on the status of the apache2 service with systemctl status:

```
sudo systemctl status apache2
```

18. Test the PHP Processing on your Web Server:

- Change directory to /var/www/html/:

```
cd /var/www/html/
```

- Create a file phpinfo.php in this folder:

```
sudo touch phpinfo.php
```

- Open the file using the nano editor and paste some codes into the file:

```
sudo nano phpinfo.php
```

Paste this:

```
<?php
phpinfo();
```

> Press Ctr+O and Enter to save, press Ctrl+X to exit the editor

19. Exit the SSH session:

```
exit
```

20. Open a new tab using your external IP address and confirm the details on the php info page:

```
http://[YOUR_EXTERNAL_IP_ADDRESS]/phpinfo.php
```

