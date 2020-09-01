# Google Cloud Fundamentals: Getting Started with Compute Engine

## Overview
In this lab, you will create virtual machines (VMs) and connect to them. You will also create connections between the instances.

## Objectives
* Create a Compute Engine virtual machine using the gcloud command-line interface.
* Connect between the two instances.

## Task 1: Sign in to the Google Cloud Platform (GCP) Console
To initialize the lab: [**Task 1: Sign in to the Google Cloud Platform (GCP) Console**](https://github.com/SymbioteKe/GADS-2020-GCP-Practice-Project/blob/translation/translation/Task%201%20-%20Sign%20in%20to%20the%20Google%20Cloud%20Platform%20(GCP)%20Console.md)

## Task 2: Create the first virtual machine using the gcloud command line

1. In GCP console, on the top right toolbar, click the Open Cloud Shell button.![alt text](https://cdn.qwiklabs.com/vdY5e%2Fan9ZGXw5a%2FZMb1agpXhRGozsOadHURcR8thAQ%3D)

2. Click **Continue**.![alt text](https://cdn.qwiklabs.com/lr3PBRjWIrJ%2BMQnE8kCkOnRQQVgJnWSg4UWk16f0s%2FA%3D)

3. To display a list of all the zones in the region to which Qwiklabs assigned you, enter this partial command `gcloud compute zones list | grep` followed by the region that Qwiklabs or your instructor assigned you to.

   Your completed command will look like this:  

   `gcloud compute zones list | grep us-central1`

4. Choose the zone assigned by Qwiklabs

5. To set your default zone to the one you just chose, enter this partial command gcloud config set compute/zone followed by the zone you chose.

   Your completed command will look like this:  

   `gcloud config set compute/zone us-central1-a`

6. To create a VM instance called **my-vm-1** in that zone, execute this command:  

   `gcloud compute instances create "my-vm-1" \  
   --machine-type "n1-standard-1" \  
   --image-project "debian-cloud" \  
   --image "debian-9-stretch-v20190213" \  
   --subnet "default"`  

   **Note**: The VM can take about two minutes to launch and be fully available for use.

7. To displays all Google Compute Engine instances in the project, execute the following command:  

   `gcloud compute instances list`  

   You will see the VM instance you have just created.

## Task 3: Create the second virtual machine using the gcloud command line

1. To display a list of all the zones in the region to which Qwiklabs assigned you, enter this partial command `gcloud compute zones list | grep` followed by the region that Qwiklabs or your instructor assigned you to.  

   Your completed command will look like this:  

   `gcloud compute zones list | grep us-central1`

2. Choose a zone from that list other than the zone to which Qwiklabs assigned you. For example, if Qwiklabs assigned you to region `us-central1` and zone `us-central1-a` you might choose zone `us-central1-b`.

3. To set your default zone to the one you just chose, enter this partial command `gcloud config set compute/zone` followed by the zone you chose.

   Your completed command will look like this:  

   `gcloud config set compute/zone us-central1-a`

4. To create a VM instance called **my-vm-2** in that zone, execute this command:  

   `gcloud compute instances create "my-vm-2" \  
   --machine-type "n1-standard-1" \  
   --image-project "debian-cloud" \  
   --image "debian-9-stretch-v20190213" \  
   --subnet "default"`  

   **Note**: The VM can take about two minutes to launch and be fully available for use.

## Task 4: Connect between VM instances
1. To displays all Google Compute Engine instances in the project, execute the following command:  

   `gcloud compute instances list`  

   You will see the VM instance you created.  

   By default, instances from all zones are listed. The results can be narrowed down using a filter:  

   `--filter="zone:( ZONE … )"`.

   You will see the two VM instances you created, each in a different zone.  

   Notice that the Internal IP addresses of these two instances share the first three bytes in common. They reside on the same subnet in their Google Cloud VPC even though they are in different zones.

2. To open a command prompt on the **my-vm-2** instance, click **SSH** in its row in the **VM instances** list.

3. Use the ping command to confirm that **my-vm-2** can reach **my-vm-1** over the network:  

   `ping my-vm-1`  

   Notice that the output of the ping command reveals that the complete hostname of **my-vm-1** is **my-vm-1.c.PROJECT_ID.internal**, where PROJECT_ID is the name of your Google Cloud Platform project. GCP automatically supplies Domain Name Service (DNS) resolution for the internal IP addresses of VM instances.

4. Press **Ctrl+C** to abort the ping command.

5. Use the **ssh** command to open a command prompt on **my-vm-1**:  

   `ssh my-vm-1`

   If you are prompted about whether you want to continue connecting to a host with unknown authenticity, enter **yes** to confirm that you do.

6. At the command prompt on **my-vm-1**, install the Nginx web server:  

   `sudo apt-get install nginx-light -y`

7. Use the **nano** text editor to add a custom message to the home page of the web server:  

   `sudo nano /var/www/html/index.nginx-debian.html`

8. Use the arrow keys to move the cursor to the line just below the `h1` header. Add text like this, and replace YOUR_NAME with your name:  

   `Hi from YOUR_NAME`

9. Press **Ctrl+O** and then press **Enter** to save your edited file, and then press **Ctrl+X** to exit the nano text editor.

10. Confirm that the web server is serving your new page. At the command prompt on **my-vm-1**, execute this command:  

   `curl http://localhost/`

   The response will be the HTML source of the web server's home page, including your line of custom text.

11. To exit the command prompt on **my-vm-1**, execute this command:  

    `exit`

    You will return to the command prompt on **my-vm-2**

12. To confirm that **my-vm-2** can reach the web server on **my-vm-1**, at the command prompt on **my-vm-2**, execute this command:  

    `curl http://my-vm-1/`  

    The response will again be the HTML source of the web server's home page, including your line of custom text.

13. To displays all Google Compute Engine instances in the project, execute the following command:  

    `gcloud compute instances list`  

    You will see the VM instances you have in the project.

14. Copy the External IP address for **my-vm-1** and paste it into the address bar of a new browser tab. You will see your web server's home page, including your custom text.

    If you forgot to click **Allow HTTP traffic** when you created the **my-vm-1** VM instance, your attempt to reach your web server's home page will fail. You can add a [firewall rule](https://cloud.google.com/vpc/docs/firewalls) to allow inbound traffic to your instances, although this topic is out of scope for this course.

## Congratulations!
In this lab, you created virtual machine (VM) instances in two different zones and connected to them using ping, ssh, and HTTP.
