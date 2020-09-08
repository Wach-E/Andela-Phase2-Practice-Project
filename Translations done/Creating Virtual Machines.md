# Lab: Creating Virtual Machines

---

## Overview
In this lab, you will explore the Virtual Machine instance options and create several VMs with different characteristics.

## Objectives
In this lab, you explore the available options for VMs and see the differences between locations.

In this lab, you learn how to perform the following tasks:

- Create several standard VMs
- Create advanced VMs

## Steps
1. Sign in to Google Cloud Platform using your qwiklabs credentials.

2. Click on the Cloud shell icon to activate the web command line.

![Select the Cloud Shell Icon](https://storage.googleapis.com/practise-test-cloud-shell-icon/personal%20shell%20stuff.png)
- Click Continue

![Continue](https://storage.googleapis.com/practise-test-cloud-shell-icon/Continue.png)

3. Create a virtual machine in the region **us-central1** and zone **us-central1-c** without an external IP using your name of choice:

```
gcloud compute instances create wach-vm --zone=us-central1-c --machine-type=n1-standard-1 \
--subnet=default --no-address --network-tier=PREMIUM --tags=http-server --image=debian-9-stretch-v20200805 \
--image-project=debian-cloud --boot-disk-size=10GB --boot-disk-type=pd-standard \
--boot-disk-device-name=wach-vm
```

4. Locate the CPU Platform of your VM:

- To view the various CPU Platforms of your zone, run:

```
gcloud compute zones describe us-central1-c --format="value(availableCpuPlatforms)"
```

- To view the CPU Platform that is working on your VM, run:

```
gcloud compute instances describe wach-vm --zone=us-central1-c
```

5. To view the availability policy of your VM run:

```
gcloud compute operations list --filter="zone:(us-central1-c)"
```

6. To view the logs of your VM:

> View the logs of your project using:

```
gcloud logging logs list
```

> Copy the logs of your VM instance into a textfile **content.txt** by running the following commands:

```
touch content.txt
```

- Read the logs of your VM and copy it to the file you just created:

```
gcloud logging read "resource.type=gce_instance" > content.txt
```

- Display the contents of the **content.txt** file:

```
cat content.txt
```

7. Create a VM instance that allows HTTP and HTTPS traffic:

> Create a VM with the windows server image using a name of your choice:

```
gcloud compute instances create windows-vm --zone=europe-west2-a --machine-type=n1-standard-2 \
--subnet=default --network-tier=PREMIUM --tags=http-server,https-server \
--image=windows-server-2016-dc-core-v20200813 --image-project=windows-cloud \
--boot-disk-size=100GB --boot-disk-type=pd-ssd --boot-disk-device-name=windows-vm
```

> Create a Firewall rule to allow HTTP traffic:

```
gcloud compute firewall-rules create default-allow-http --direction=INGRESS --priority=1000 \
--network=default --action=ALLOW --rules=tcp:80 --source-ranges=0.0.0.0/0 \
--target-tags=http-server
```

> Create a Firewall rule to allow HTTPS traffic:
```
gcloud compute firewall-rules create default-allow-https --direction=INGRESS --priority=1000 \ 
--network=default --action=ALLOW --rules=tcp:443 --source-ranges=0.0.0.0/0 \
--target-tags=https-server
```

8. Create a password for your windows VM using:

```
gcloud compute reset-windows-password windows-vm
```

- You will be presented with a confirmation prompt; this will need to be accepted by entering **Y** and/or pressing Enter. It can be rejected by entering **N**, then pressing Enter

```
This command creates an account and sets an initial password for the
user [username] if the account does not already exist.
If the account already exists, resetting the password can cause the
LOSS OF ENCRYPTED DATA secured with the current password, including
files and stored passwords.

For more information, see:
https://cloud.google.com/compute/docs/operating-systems/windows#reset

Would you like to set or reset the password for [username] (Y/n)?
```

> Result: On confirmation, you will be shown your instance **ip address, password and username.**

9. Create a custom VM and give it a name of your choice:

```
gcloud compute instances create custom-instance --zone=us-west1-b \
--machine-type=n1-custom-6-32768 --subnet=default --network-tier=PREMIUM \
--image=debian-9-stretch-v20200805 --image-project=debian-cloud --boot-disk-size=10GB \
--boot-disk-type=pd-standard --boot-disk-device-name=custom-instance 
```

10. SSH into your custom VM by running:

```
gcloud compute ssh custom-vm --zone us-west1-b
```

11. To see information about unused and used memory and swap space on your custom VM, run the following command:

```
free
```

12. To see details about the RAM installed on your VM, run the following command:

```
sudo dmidecode -t 17
```

13. To verify the number of processors, run the following command:

```
nproc
```

14. To see details about the CPUs installed on your VM, run the following command:

```
lscpu
```

15. To exit the SSH terminal, run the following command:

```
exit
```
