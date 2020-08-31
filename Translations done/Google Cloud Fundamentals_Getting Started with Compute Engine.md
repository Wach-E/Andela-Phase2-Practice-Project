# Lab: Google Cloud Fundamentals - Getting Started with Compute Engine

---

## Overview
In this lab, you will create virtual machines (VMs) and connect to them. You will also create connections between the instances.

## Objectives
In this lab, you will learn how to perform the following tasks:
- Create two Compute Engine virtual machine using the gcloud command-line interface.
- Connect between the two instances.

## Steps
1. Sign in to Google Cloud Platform using your qwiklabs credentials.

2. Click on the Cloud shell icon to activate the web command line.

![Select the Cloud Shell Icon](https://storage.googleapis.com/practise-test-cloud-shell-icon/personal%20shell%20stuff.png)
- Click Continue

![Continue](https://storage.googleapis.com/practise-test-cloud-shell-icon/Continue.png)

3. Create a virtual machine which allows HTTP traffic:

- Create a virtual machine in us-central1-a:

```
gcloud compute instances create my-vm-1 --zone=us-central1-a --machine-type=n1-standard-1 \
--subnet=default --network-tier=PREMIUM --tags=http-server --image=debian-9-stretch-v20200805 \
--image-project=debian-cloud --boot-disk-size=10GB --boot-disk-type=pd-standard \
--boot-disk-device-name=my-vm-1
```

- Create a firewall ingress rule that allows HTTP traffic for my-vm-1:

```
gcloud compute firewall-rules create default-allow-http \
--direction=INGRESS --priority=1000 --network=default --action=ALLOW  \
--rules=tcp:80 --source-ranges=0.0.0.0/0 --target-tags=http-server
```

4. Create another vm in the zone us-central1-b:

- To display the zones for this region, enter the **gcloud compute zones list | grep** command followed by the region for creation:

```
gcloud compute zones list | grep us-central1
```

- Choose any zone of your choice from the list and set your default zone to the one you just chose using the **gcloud config set compute/zone** command followed by your zone:

```
gcloud config set compute/zone us-central1-b
```

5. Create a VM instance called my-vm-2 in that zone by executing this command:

```
gcloud compute instances create "my-vm-2" --machine-type="n1-standard-1" \
--image-project="debian-cloud" --image="debian-9-stretch-v20190213" \
--subnet="default"
```

6. SSH into **my-vm-2**:

```
gcloud compute ssh my-vm-2
```

7. Use the **ping** command to confirm that **my-vm-2** can reach **my-vm-1**

```
ping -c 3 my-vm-1
```

> Notice that the output of the ping command reveals that the complete hostname of my-vm-1 is **my-vm-1.c.PROJECT_ID.internal** , where PROJECT_ID is the name of your Google Cloud Platform project

8. Use the ssh command to open a command prompt on my-vm-1:

```
ssh my-vm-1
```

9. At the command prompt on my-vm-1, install the Nginx web server:

```
sudo apt-get install nginx-light -y
```

10. Use the nano text editor to add a custom message to the home page of the web server:

```
sudo nano /var/www/html/index.nginx-debian.html
```

11. Use the arrow keys to move the cursor to the line just below the h1 header. Add text like this, and replace YOUR_NAME with your name:

```
Hi from Wachukwu Emmanuel
```

- Press **Ctrl+O** and press **Enter** to save. Press **Ctrl+X** to Exit

12. Confirm that the web server is serving your new page. At the command prompt on my-vm-1, execute this command:

```
curl http://localhost/
```
> Output: The response will be the HTML source of the web server's home page, including your line of custom text.

13. To exit the command prompt on my-vm-1, execute this command:

```
exit
```

> You will return to my-vm-2 ssh

14. To confirm that my-vm-2 can reach the web server on my-vm-1, at the command prompt on my-vm-2, execute this command:

```
curl http://my-vm-1/
```

> Output: The response will again be the HTML source of the web server's home page, including your line of custom text.

15. Exit the ssh session on my-vm-2:

```
exit
```

16. View the details of **my-vm-1** instance with the command:

```
gcloud compute instances list --zone us-central1-a
```

17. Copy the External IP address for my-vm-1 and paste it into the address bar of a new browser tab.

> You will see your web server's home page, including your custom text.