## Overview
This project leads us through the steps to launch and configure our first virtual machine in the AWS cloud* This involves  using Amazon Machine Images to launch Amazon EC2 instances, creating key pairs for SSH authentication, securing network access to Amazon EC2 instances with security groups, automatically configuring Amazon EC2 instances with bootstrapping scripts, and attaching Elastic IPs to Amazon EC2 instances to provide static Internet addresses* At the end we will have deployed a simple
web server which includes an informational page to display details of your virtual web server instance*

## Topics
By the end of this exercise, you will be able to:
* Create a new Amazon EC2 server instance from an existing server template.
* Create a security group to restrict access to the server's resources.
* Launch the instance.
* Access the instance's Linux command-line interface directly, using a key pair for authentication.
* Associate an Elastic IP address with our Amazon EC2 instance

## Steps
* On the Services menu, click EC2.
* Click Launch Instance.
* Because you require a Linux instance, in the row for the basic 64-bit Amazon Linux 2 AMI, which will normally be the first option on the list, click Select. When you create an instance, AWS will ask you which instance family you want to use. The family you choose determines how much throughput and processing cycles are available to your instance.
* On the Choose an Instance Type page, the Micro instance type, which is the lowest-cost option, should be automatically selected.
* Click Next: Configure Instance Details.
*  On the Configure Instance Details page, scroll down to view Advanced Details section.
* For User data, select As text.
#!/bin/sh
sudo yum update -y
sudo amazon-linux-extras install -y lamp-mariadb10.2-php7.2 php7.2
sudo yum install -y httpd mariadb-server
sudo systemctl start httpd
sudo systemctl enable httpd
sudo systemctl is-enabled httpd
* Click Next: Add Storage
* Click Next: Add Tags to accept the default storage device configuration.
* On the Tag Instance page, Click Add Tag button and type a name for your instance in the Value box. (Key: Name, Value: YourNameWebServer) This name, more correctly known as a tag, will appear in the console when the instance launches. It makes it easy to keep track of running machines in a complex environment.
* Click Next: Configure Security Group.
* For Assign a security group, click Create a new security group.
* In the Security group name box, type the same name that you used for the tag with post fix -SG.
* Type a description for your security group.
* By default, SSH port 22 is Open. To add new rule click Add Rule to open a new port.
* In the Type drop-down list, click HTTP.
* Click Review and Launch.
* Review your choices, and then click Launch.
* In the key pair dialog box, create a new key pair from dropdown menu, named YourNameKey and download it on your desktop 
* Click Launch Instances.
* On the status page, which notifies you that your instances are launching, click View Instances.
* Select your instance to display a list of details and status update in the lower pane
* In your list of running Amazon EC2 instances, select the instance to display the default webserver page.
* Copy the Public DNS value to your Clipboard. It will look something like ec2-54-84-236-205.compute-1.amazonaws.com.
* Open a new browser window, and then paste the public DNS value into the address bar.
* Your Apache webserver’s default page should be displayed in the browser. 
It looks like this ![default web browser](./images/Default%20Wesever%20Page.PNG)
* In your list of running Amazon EC2 instances, select the instance to display the instance details.
* Copy the Public DNS value to your Clipboard. It will look something like ec2-54-84-236-205.compute-1.amazonaws.com.
* Start PuTTYgen.
* Under Type of key to generate, choose RSA.
* Choose Load.
* Select your .pem file for the key pair that you specified when you launched your instance, and then choose Open. Choose OK to dismiss the confirmation dialog box.
* Choose Save private key to save the key in the format that PuTTY can use. PuTTYgen displays a warning about saving the key without a passphrase. Choose Yes.  
* Specify the same name for the key that you used for the key pair (for example, mykey-pair). PuTTY automatically adds the  ppk file extension. 
* Open PuTTY.exe (you may have downloaded it at the beginning of this lab).
* In the Host Name box, enter ec2-user@<public DNS> Paste the public DNS value from your Clipboard.
* In the Category list, expand SSH.
* Click Auth (don't expand it).
* In the Private key file for authentication box, browse to the PPK file that you downloaded and double-click it.
* Click Open.
* Click Yes when prompted to allow a first connection to this remote SSH server. Because you are using a key pair for authentication, you will not be prompted for a password.
* Type the following into PuTTY or Terminal window in order to create an index.php file at the root of your HTTP web server HTML document directory:
```
cd /var/www/html
sudo nano index.php
```
* Copy the following code to a text file, and then copy it from the text file to Nano:
```
<?php
 $url = "http://169.254.169.254/latest/meta-data/instance-id";
 $instance_id = file_get_contents($url);
 echo "Instance ID: <b>" . $instance_id . "</b><br/>";
 $url = "http://169.254.169.254/latest/meta-data/placement/availability-zone";
 $zone = file_get_contents($url);
 echo "Zone: <b>" . $zone . "</b><br/>";
 ?>
 ```
* Press CTRL+O, ENTER to save your document as index.php.
* Press CTRL+X to exit the Nano editor.
* Close your PuTTY or Terminal window.
* Return to the AWS Management Console.
* In your list of running Amazon EC2 instances, select the instance to display the instance details.
* Copy the Public DNS value to your Clipboard. It will look something like ec2-54-84-236-205.compute-1.amazonaws.com.
* Open a new browser window, and then paste the public DNS value into the address bar. Your instance ID and Availability Zone should be displayed in the browser:
![website](./images/View%20Website.PNG)








