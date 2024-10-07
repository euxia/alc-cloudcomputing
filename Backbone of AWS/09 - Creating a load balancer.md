# Step-by-Step Guide to Configuring an Elastic Load Balancer with Multiple Instances

This guide will walk you through the process of setting up an Elastic Load Balancer (ELB) with three EC2 instances that provide slightly different outputs. This setup will help demonstrate how load balancing works by distributing traffic across the instances.

## Prerequisites

- Access to an AWS account
- Basic understanding of EC2 and Elastic Load Balancer

## Step 1: Create Three EC2 Instances

1. **Launch EC2 Instances**:
   - Go to the EC2 Dashboard.
   - Click on "Launch Instance."
   - Choose the image we created earlier.
   - Same keypair and security group.
   - Launch the instances in the same VPC and availability zone.

2. **SSH into Each Instance**:
   - Use SSH to connect to the 2nd and 3rd instance we created.
   - Make slightly different changes to the Apache web server on each instance.
     - For example, change the default page content or add a different image.

## Step 2: Configure the Elastic Load Balancer

1. **Navigate to Load Balancers**:
   - In the EC2 Dashboard, click on "Load Balancers" under "Load Balancing".

2. **Create Load Balancer**:
   - Click on "Create Load Balancer" and select "Application Load Balancer".

3. **Configure Load Balancer**:
   - **Name**: Enter a name like `apache-demoloadbalancer`.
   - **Scheme**: Select "Internet-facing".
   - **VPC and Subnets**: Choose the same VPC and select all availability zones where your instances are located.
     - To check the availability zone of your instances:
       - Click on the instance ID of the first instance.
       - Select "Networking" to find the availability zone.
       - Repeat for the second instance.

4. **Select Security Group**:
   - Choose the security group that applies to all instances.

5. **Listeners and Routing**:
   - Leave the default listener set to port 80 (HTTP).

6. **Create Target Group**:
   - Create a new target group for the instances.
   - Select "Instance" as the target type.
   - Add all three instances to the target group.

7. **Launch the Load Balancer**:
   - Review the settings and click "Create".

## Step 3: Test the Load Balancer

1. **Obtain Load Balancer DNS**:
   - Once the load balancer is created, obtain its DNS name from the Load Balancers section.

2. **Access the Load Balancer**:
   - Open a web browser and enter the DNS name of the load balancer.
   - Refresh the page multiple times to see the different instance outputs:
     - Instance 1
     - Instance 2
     - Instance 3

## Step 4: Monitor Load Balancer and Instances

1. **Check Target Group Health Status**:
   - In the Load Balancers section, click on your load balancer.
   - Select the target group associated with your instances.
   - Check the health status of each instance to ensure they are properly registered and healthy.

2. **Scaling and Optimization**:
   - Optionally, configure auto-scaling policies to manage traffic effectively.

