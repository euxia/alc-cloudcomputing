# How to Set Up an Auto Scaling Group in EC2

## Prerequisites
Before creating an Auto Scaling Group, ensure that:
- You have an **EC2 instance** or an **Amazon Machine Image (AMI)**.
- You've configured a **key pair** for SSH access.
- A **VPC** (Virtual Private Cloud) and **subnets** are available for your instances.
- Ensure that you have the necessary **IAM permissions** to create Auto Scaling Groups, Launch Templates, and EC2 Instances.

## Step 1: Create a Launch Template or Launch Configuration

A **Launch Template** or **Launch Configuration** defines the configuration for instances in your Auto Scaling Group, including the AMI, instance type, key pair, security group, and block storage options.

### Using the AWS Management Console:

1. **Navigate to the EC2 Dashboard**:
   - Log in to the AWS Management Console.
   - In the search bar, type "EC2" and select **EC2** from the results.

2. **Create a Launch Template**:
   - On the left sidebar, under **Instances**, click **Launch Templates**.
   - Click **Create launch template**.
   - Provide a **Name** and **Description** for the template.
   - Under **Source AMI**, select the Amazon Machine Image (AMI) you wish to use for your instances.
   - Choose an **Instance type** (e.g., t2.micro for free tier).
   - Assign a **Key Pair** for SSH access to your instances.
   - In the **Network settings**, select your **VPC** and choose the appropriate **subnets** for your instances.
   - Under **Security Groups**, either select an existing security group or create a new one to define inbound/outbound traffic rules.
   - Configure **storage** by selecting the root volume size and type (e.g., 8 GiB General Purpose SSD).
   - Click **Create launch template**.

### Using AWS CLI:

You can also create a Launch Template using the AWS CLI:

```
aws ec2 create-launch-template \
    --launch-template-name my-template \
    --version-description "Version 1" \
    --launch-template-data '{
      "ImageId": "ami-0abcdef1234567890",
      "InstanceType": "t2.micro",
      "KeyName": "my-key-pair",
      "SecurityGroupIds": ["sg-0123456789abcdef0"],
      "BlockDeviceMappings": [{
        "DeviceName": "/dev/xvda",
        "Ebs": {
          "VolumeSize": 8,
          "VolumeType": "gp2"
        }
      }]
    }
```

## Step 2: Create an Auto Scaling Group

An Auto Scaling Group ensures that you always have the right number of EC2 instances running to handle your application's load. It automatically increases or decreases the number of instances according to your defined policies.

### Using the AWS Management Console:

1. Navigate to Auto Scaling Groups:
   - In the EC2 Dashboard, under Auto Scaling, click Auto Scaling Groups.
Click Create Auto Scaling group.  
2. Configure Basic Settings:
   - Name your Auto Scaling group. Select the Launch Template or Launch Configuration created earlier.
3. Choose a VPC and Subnets:
   - Select a VPC and at least two subnets (for high 
availability).
4. Configure Instance Scaling
   - Set the Minimum, Desired, and Maximum number of instances.
    Example:
     - Minimum: 1
     - Desired: 2
     - Maximum: 4
  
5. Configure Scaling Policies (Optional):
    - Set up policies to automatically scale based on CPU usage or other metrics.
    - Select Target tracking scaling policy and define a target, e.g., maintaining CPU utilization at 50%.

6. Configure Notifications (Optional):
    - Add notifications to receive alerts for scaling activities.

7. Review and Create:
   - Review the configuration and click Create Auto Scaling group.

### Using AWS CLI:

You can also create an Auto Scaling Group using the AWS CLI:

```
aws autoscaling create-auto-scaling-group \
    --auto-scaling-group-name my-auto-scaling-group \
    --launch-template "LaunchTemplateName=my-template,Version=1" \
    --min-size 1 \
    --max-size 4 \
    --desired-capacity 2 \
    --vpc-zone-identifier "subnet-0123456789abcdef0,subnet-0abcdef1234567890"
```

## Step 3: Configure Health Checks  

1. Navigate to the Auto Scaling Group in the AWS Console.
2. Select the Auto Scaling group you just created.
3. Under Instances, click Edit.
4. Choose EC2 health check or ELB health check (if using an Elastic Load Balancer).
5. Save your changes.

### Using AWS CLI:

```
aws autoscaling update-auto-scaling-group \
    --auto-scaling-group-name my-auto-scaling-group \
    --health-check-type ELB \
    --health-check-grace-period 300
```

## Step 4: Verify Auto Scaling Behavior
1. Go to Auto Scaling Groups in the AWS Console.
2. View the status of your instances and check whether they scale based on load.
3. Adjust your load (e.g., using a stress test) to see the scaling in action.
   
### Monitoring (Optional):

Use CloudWatch to monitor CPU utilization and trigger scaling events.

```
aws cloudwatch put-metric-alarm \
    --alarm-name "ScaleUpOnHighCPU" \
    --metric-name CPUUtilization \
    --namespace AWS/EC2 \
    --statistic Average \
    --period 60 \
    --threshold 70 \
    --comparison-operator GreaterThanOrEqualToThreshold \
    --dimensions "Name=AutoScalingGroupName,Value=my-auto-scaling-group" \
    --evaluation-periods 2 \
    --alarm-actions "arn:aws:autoscaling:region:account-id:scalingPolicy:policy-id:autoScalingGroupName/my-auto-scaling-group:policyName/ScaleOutPolicy"
```


