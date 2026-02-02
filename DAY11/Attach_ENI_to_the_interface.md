## Steps to Attach an Elastic Network Interface

### Step 1: Open the EC2 Console
- Log in to the AWS Management Console
- Navigate to **Services → EC2**

### Step 2: Create a Network Interface
- In the left navigation pane, select **Network Interfaces**
- Click **Create network interface**
- Choose the following:
  - VPC (same as the EC2 instance)
  - Subnet (same Availability Zone as the EC2 instance)
  - Security Group
- Click **Create**

### Step 3: Attach the ENI to an EC2 Instance
- Select the newly created network interface
- Click **Actions → Attach**
- Choose the target EC2 instance
- Set the device index (e.g., 1)
- Click **Attach**

- Make sure that the status of the elastic network interface shows attached