# Workshop Guide: Setting Up an Elastic IP on AWS EC2

## Introduction
In this workshop, we will learn how to allocate and associate an Elastic IP address to an EC2 instance. An Elastic IP is a static, public IPv4 address designed for dynamic cloud computing. It allows you to mask instance or availability zone failures by quickly remapping your public IP addresses to any instance in your account.

## Prerequisites
- AWS account
- Basic knowledge of AWS EC2
- An existing EC2 instance to associate the Elastic IP with

## Steps to Set Up an Elastic IP

### Step 1: Sign in to the AWS Management Console
1. Go to the [AWS Management Console](https://aws.amazon.com/console/).
2. Sign in with your AWS credentials.

### Step 2: Navigate to the EC2 Dashboard
1. In the AWS Management Console, find and select **EC2** from the Services menu.

### Step 3: Allocate an Elastic IP Address
1. In the left navigation pane, click on **Elastic IPs** under the **Network & Security** section.
2. Click the **Allocate Elastic IP address** button.
3. (Optional) Choose to allocate from a specific pool (default is fine).
4. Click **Allocate**. You will see a confirmation message, and your new Elastic IP will be displayed.

### Step 4: Associate the Elastic IP with an EC2 Instance
1. Select the newly allocated Elastic IP from the list.
2. Click on the **Actions** dropdown menu.
3. Choose **Associate Elastic IP address**.
4. In the **Instance** field, start typing the ID or name of your EC2 instance and select it from the dropdown.
5. Leave the **Private IP address** field as default unless you have specific configurations.
6. Click **Associate**.

### Step 5: Verify the Association
1. Navigate back to the **Instances** section in the EC2 dashboard.
2. Select your EC2 instance and scroll down to the **Description** tab.
3. Verify that the **Elastic IP** is now listed under the instance's public IP address.

### Step 6: Update Security Group (If Necessary)
- Ensure your instance's security group allows inbound traffic on the required ports (e.g., port 80 for HTTP, port 22 for SSH).
  
### Step 7: Test the Elastic IP
1. Open a web browser.
2. Enter the Elastic IP address in the address bar.
3. If a web server is running on your instance, you should see the expected response.

## Cleanup
1. If you no longer need the Elastic IP, go back to the **Elastic IPs** section.
2. Select the Elastic IP and choose **Release Elastic IP address** from the **Actions** dropdown.
3. Confirm the release.

## Additional Resources
- [AWS EC2 Documentation](https://docs.aws.amazon.com/ec2/index.html)
- [Elastic IP Addresses](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html)


