## **ALTSCHOOL AFRICA**

## **Holiday Project**

<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-project">Project Overview</a>
      <ul>
       <li><a href="#prerequisites">What is Load Balancing</a></li>
        <li><a href="#prerequisites">Application Load Balancer Overview</a></li>
        <li><a href="#built-with">Virtual Private Cloud</a></li>
        <li><a href="#built-with">VWhat is Network Address Translation(Nat) Gateway?</a></li>
      </ul>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li><a href="#steps">Creating VPC</a></li>
        <li><a href="#steps">Create a Private EC2 Instance</a></li>
        <li><a href="#steps"> Create a Bastion Host</a></li>
        <li><a href="#steps">SSH Connections</a></li>
        <li><a href="#steps">Installation and Configuration of Nginx Server on Private EC2 Instances</a></li>
        <li><a href="#steps">Creating Target Group</a></li>
        <li><a href="#steps">Creating Application Load Balancer</a></li>
        <li><a href="#steps">Configuring Application Load Balancer Security Group</a></li>
      </ul>
    </li>
    <li><a href="#contact">Contacts</a></li>
    <li><a href="#acknowledgments">Acknowledgments</a></li>
  </ol>
</details>

## **Project Overview**

## What is Load Balancing?

This is a way of distributing incoming traffic efficiently across multiple servers.

## Application Load Balancer Overview

Application Load Balancer Routes incoming Client HTTP/HTTPS traffic across multiple servers

## Virtual Private Cloud (VPC)

A VPC is an Isolated Private Cloud within a Public Cloud.

## What is Network Address Translation(Nat) Gateway?

This is a service that enables instances in a private subnet to access the internet but prevents the internet to directly connect to the instances. Simply put, it means the IP address of your computer is not seen on the Internet when you connect to web pages, etc.

## TASK

## You are required to perform the following tasks

- Set up 2 EC2 instances on AWS(use the free tier instances).

- Deploy an Nginx web server on these instances(you are free to use Ansible)

- Set up an ALB(Application Load balancer) to route requests to your EC2 instances

- Make sure that each server displays its own Hostname or IP address. You can use any programming language of your choice to display this.

- Work on building a personal portfolio and CV (Check out resumeworded.com).

## Important points to note

- I should not be able to access your web servers through their respective IP addresses. Access must be only via the load balancer

- You should define a logical network on the cloud for your servers.

- Your EC2 instances must be launched in a private network.

- Your Instances should not be assigned public IP addresses.

- You may or may not set up auto scaling(I advice you do for knowledge sake)

- You must submit a custom domain name(from a domain provider e.g. Route53) or the ALB’ domain name.

## **Getting Started**

## **Step One:**

## **1. Creating VPC**

Open the Amazon VPC console at https://console.aws.amazon.com/vpc/

On the VPC Dashboard, choose Launch VPC Wizard.

![s1](/images/vpc1.png)

On the VPC configuration Dashboard choosing VPC and more automatically launches Private Subnets, Public Subnets, Routing Tables and Subnet Associations, Internet GateWay, Elastic IP, IP CIDR block, Availability Zones and Network Access Translator.

On the Auto-generate input field, write the name of your VPC

![s1](/images/vpc2.png)

Choose the number of Avalaibility Zones (AZ's) in which to create your NAT GateWay.

![s1](/images/vpc4.png)

The image below shows the auto-generated configurations i.e. Subnets, Routes Tables and Network Connections.

![s1](/images/vpc3.png)

## **2. Create a Private EC2 Instance**

Navigate to the ec2 console and click on Launch Instance

![s1](/images/e1.png)

Write the name of your instances, select the number of instances and use Ubuntu as choice of Linux Distro.

![s1](/images/e2.png)

Select your key-pair if you dont have a key-pair create one

Next, select the VPC that you previously created, and choose any of the private subnet, Disable the Auto-Assigned Public IP, and finally Create a Security Group keeping the default settings then click on Launch Instance.

![s1](/images/e3.png)

## **3. Create a Bastion Host**

A bastion host is a server whose purpose is to provide access to a private network from an external network. This is necessary because our subnets are private, hence, the bastion host which is within the same VPC serves as a bridge to establibish connection to the private subnets. The best practice is that anyone who needs access to any of the computers inside the VPC must SSH into the bastion host first before doing another SSH to the instance they want to go to.

Navigate to the ec2 console and click on Launch Instance

Write the name of your instance, and use Ubuntu as the choice of Linux Distro.

![s1](/images/bastion1.png)

Select your key-pair

Next, select the VPC that you previously created, and choose any of the public subnet, Enable the Auto-Assigned Public IP, and finally choose a Security Group keeping the default settings then click on Launch Instance.

![s1](/images/bastion2.png)

## **4. SSH Connections**

Select on the Bastion Host Instance and click on connect which will launch a Dashboard.
![s1](/images/connect1.png)

Select SSH client and follow the instructions on how to connect.

![s1](/images/connect2.png)

Access the bastion host via SSH. Copy the keypair you downloaded to the bastion host server. Then, run chmod 400 yourkeypairname.pem

![s1](/images/connect3.png)

Below is an Example of SSH key-pair that has been copied into the Bastion Host server

![s1](/images/connect6.png)

Run chmod 400 yourkeypairname.pem, then access the Private Server via SSH from the Bastion host.

![s1](/images/connect7.png)

## **5. Installation and Configuration of Nginx Server on Private EC2 Instances**

Using the following commands; update and install Nginx and login to the root user to setup static webpage.

    $ sudo apt update; sudo apt install nginx -y; sudo su

![s1](/images/connect8.png)

From the root user, run the following commands to display the hostname of your server;

    # echo "<h1>This is my server2 $(hostname -f)</h1>" > /var/www/html/index.nginx-debian.html

Then cat your file to confirm that host is being displayed using;

    # cat /var/www/html/index.nginx-debian.html

Exit the root user and then enable, start, and confirm status of nginx server using the following commands

    $ sudo systemctl enable nginx; sudo systemctl start nginx; sudo systemctl status nginx

![s1](/images/connect9.png)

Exit from the first Private server and from your Bastion host ssh into your second server and repeat whole process to update and install nginx, login to root user and echo your hostname, cat file to confirm hostname is being displayed, exit root user, and then enable, start, and confirm status of the nginx server.

## **6. Creating Target Group**

Navigate to Target Group and click on create Target Group

![s1](/images/target1.png)

Choose instances as your target type and give your target group a name.

![s1](/images/target2.png)

Select the VPC you have already created on the drop down menu and leave the default protocol on Http1.

![s1](/images/target3.png)

Click on next

![s1](/images/target4.png)

Then select the Private instances in your VPC and click on the include as pending.

![s1](/images/target5.png)

Currently there is no load balancer configured to this target group. Click on the None associated and select the new load balancer.

![s1](/images/target6.png)

## **7. Creating Application Load Balancer**

Give your Application Load Balancer a name and leave the Scheme and IP address type on default.

![s1](/images/load1.png)

Then on Network Mapping select your VPC and choose your public subnets associated with your VPC.

![s1](/images/load2.png)

For the Security Groups click on create new security group

![s1](/images/load3.png)

Give your Security Group a name, brief description and ensure you choose your created VPC from the drop down menu.

![s1](/images/load4.png)

Edit the Inbound rules to allow Http and Https traffic from anywhere and leave Outbound rules on default. Then click on create and return to the previous page to assign the newly created Security Group.

On the **Listeners and routing section**, select the target group previously created, leaving the rest of the configuration on default and finally click on create load balancers.

**NOTE:** The Load Balancers takes awhile to provision.

![s1](/images/load5.png)

## **Configuring Application Load Balancer Security Group**

Navigate to your Security Group and select the security group. that is associated with your instances

![s1](/images/s1.png)

Edit the inbound rules to allow traffic on only the load balancer by selecting the Security Group that is associated with the load balancer. Then save the rules.

![s1](/images/s2.png)

Confirm the health status of your target groups, if it is unhealthy, re-start the nginx server and refresh the target groups page

    $ sudo systemctl restart nginx

![s1](/images/health.png)

Now to check whether things are working properly let's test our Load Balancer Distribution. We will copy the Distribution domain name and enter it into our browser.

![s1](/images/ip1.png)

![s1](/images/ip2.png)

## 🔗 Contacts

## BENEDICTA ONYEKWERE

[![linkedin](https://img.shields.io/badge/linkedin-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/benedicta-onyekwere-063682123/)
[![WhatsApp](https://img.shields.io/badge/WhatsApp-25D366?style=for-the-badge&logo=whatsapp&logoColor=white)](https://wa.me/2349095717383)
[![Twitter](https://img.shields.io/badge/Twitter-1DA1F2?style=for-the-badge&logo=Twitter&logoColor=white)](https://twitter.com/BennieOnyekwere)

## MICHAEL AGBIAOWEI

[![linkedin](https://img.shields.io/badge/linkedin-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/maiempire/)
[![WhatsApp](https://img.shields.io/badge/WhatsApp-25D366?style=for-the-badge&logo=whatsapp&logoColor=white)](https://wa.me/2348089440108)
[![Twitter](https://img.shields.io/badge/Twitter-1DA1F2?style=for-the-badge&logo=Twitter&logoColor=white)](https://twitter.com/michaelagbiaow2)


## Acknowledgments

- [https://dev.to/raphael_jambalos/secure-aws-environments-with-private-public-subnets-2ei9](https://dev.to/raphael_jambalos/secure-aws-environments-with-private-public-subnets-2ei9)

- [Altschool Africa](https://www.altschoolafrica.com/)
