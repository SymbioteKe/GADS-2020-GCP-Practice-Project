# Google Cloud Fundamentals: Getting Started with Cloud Storage and Cloud SQL

## Overview
In this lab, you create a Cloud Storage bucket and place an image in it. You'll also configure an application running in Compute Engine to use a database managed by Cloud SQL. For this lab, you will configure a web server with PHP, a web development environment that is the basis for popular blogging software. Outside this lab, you will use analogous techniques to configure these packages.

You also configure the web server to reference the image in the Cloud Storage bucket.

## Objectives
In this lab, you learn how to perform the following tasks:

* Create a Cloud Storage bucket and place an image into it.

* Create a Cloud SQL instance and configure it.

* Connect to the Cloud SQL instance from a web server.

* Use the image in the Cloud Storage bucket on a web page.

## Task 1: Sign in to the Google Cloud Platform (GCP) Console
To initialize the lab: [**Task 1: Sign in to the Google Cloud Platform (GCP) Console**](https://github.com/SymbioteKe/GADS-2020-GCP-Practice-Project/blob/translation/translation/Task%201%20-%20Sign%20in%20to%20the%20Google%20Cloud%20Platform%20(GCP)%20Console.md)

## Task 2: Deploy a web server VM instance

1. In GCP console, on the top right toolbar, click the Open Cloud Shell button.![alt text](https://cdn.qwiklabs.com/vdY5e%2Fan9ZGXw5a%2FZMb1agpXhRGozsOadHURcR8thAQ%3D)

2. Click **Continue**.![alt text](https://cdn.qwiklabs.com/lr3PBRjWIrJ%2BMQnE8kCkOnRQQVgJnWSg4UWk16f0s%2FA%3D)

3. To display a list of all the zones in the region to which Qwiklabs assigned you, enter this partial command `gcloud compute zones list | grep` followed by the region that Qwiklabs or your instructor assigned you to.

   Your completed command will look like this:  

   `gcloud compute zones list | grep us-central1`

4. Choose the zone assigned by Qwiklabs

5. To set your default zone to the one you just chose, enter this partial command `gcloud config set compute/zone` followed by the zone you chose.

   Your completed command will look like this:  

   `gcloud config set compute/zone us-central1-a`

6. To create a VM instance called **bloghost** in that zone, execute this command:  

   `gcloud compute instances create "bloghost" \--machine-type "n1-standard-1" \--image-project "debian-cloud" \--image "debian-9-stretch-v20190514" \--subnet "default" \--tags http-server \--metadata startup-script='#! /bin/bash
   sudo su -
   apt-get update
   apt-get install apache2 php php-mysql -y
   service apache2 restart'`

   **Note**: The VM can take about two minutes to launch and be fully available for use.

7. To displays all Google Compute Engine instances in the project, execute the following command:  

   `gcloud compute instances list`  

   You will see the VM instance you have just created.

## Task 3: Create a Cloud Storage bucket using the gsutil command line

1. For convenience, enter your chosen location into an environment variable called LOCATION. Enter one of these commands:

export LOCATION=US

Or

export LOCATION=EU

Or

export LOCATION=ASIA

2. In Cloud Shell, the DEVSHELL_PROJECT_ID environment variable contains your project ID. Enter this command to make a bucket named after your project ID:

gsutil mb -l $LOCATION gs://$DEVSHELL_PROJECT_ID

3. Retrieve a banner image from a publicly accessible Cloud Storage location:

gsutil cp gs://cloud-training/gcpfci/my-excellent-blog.png my-excellent-blog.png

4. Copy the banner image to your newly created Cloud Storage bucket:

gsutil cp my-excellent-blog.png gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.png

5. Modify the Access Control List of the object you just created so that it is readable by everyone:

gsutil acl ch -u allUsers:R gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.png
