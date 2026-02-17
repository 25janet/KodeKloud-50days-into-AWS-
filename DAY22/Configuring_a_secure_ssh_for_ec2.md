EC2 Passwordless SSH Setup: datacenter-ec2
This document outlines the procedure for setting up an EC2 instance (t2.micro) accessible via root SSH from a landing host (aws-client) using RSA key-pair authentication.

1. Key Generation on aws-client
First, generate the RSA key pair on the landing host. This serves as the "identity" for the client.

Bash
# Ensure the .ssh directory exists with correct permissions
mkdir -p /root/.ssh
chmod 700 /root/.ssh

# Generate the 2048-bit RSA key pair
# -f defines the path, -N "" ensures no passphrase is required
ssh-keygen -t rsa -b 2048 -f /root/.ssh/id_rsa -N ""

# Display the public key (Copy this output for the next step)
cat /root/.ssh/id_rsa.pub
2. Infrastructure Configuration
The instance must be launched with specific parameters to allow connectivity and root-level access.

Instance Details
Parameter	Value
Name	datacenter-ec2
Instance Type	t2.micro
User	root
Security Group	Inbound TCP Port 22 allowed from aws-client IP
User Data Script
To automate the configuration, paste the following into the Advanced Details > User Data field in the AWS Console.

Note: Replace the ssh-rsa ... string below with your actual public key content.

Bash
#!/bin/bash
# Create root SSH directory
mkdir -p /root/.ssh
chmod 700 /root/.ssh

# Authorize the aws-client public key
echo "ssh-rsa AAAAB3NzaC1yc2E...[YOUR_KEY_HERE]... root@aws-client" >> /root/.ssh/authorized_keys
chmod 600 /root/.ssh/authorized_keys
chown -R root:root /root/.ssh

# Enable direct Root SSH login in the config
sed -i 's/^#PermitRootLogin.*/PermitRootLogin yes/' /etc/ssh/sshd_config
sed -i 's/^PermitRootLogin.*/PermitRootLogin yes/' /etc/ssh/sshd_config

# Restart the SSH service to apply changes
systemctl restart sshd
3. Network Verification
Before connecting, ensure the AWS "path" is open.

Security Group: Verify Port 22 is open for Inbound traffic.

Route Table: Ensure the subnet has a route (0.0.0.0/0) to an Internet Gateway (IGW).

Public IP: Confirm the instance has a Public IPv4 address assigned.

4. Connection Procedure
From the aws-client terminal, execute the following command:

Bash
ssh -i /root/.ssh/id_rsa root@<EC2_PUBLIC_IP>
Expected Output
Fingerprint Warning: Type yes to continue.

Success: You should be logged in immediately without a password prompt.

5. Troubleshooting
Connection Timeout: Usually a Security Group or Routing issue. Check if Port 22 is open to the world (0.0.0.0/0) temporarily for testing.

Permission Denied: Usually means the id_rsa.pub was pasted incorrectly in User Data or the SSH service didn't restart.

Refused Login: If you can't get in as root, try ec2-user and check /var/log/cloud-init-output.log to see if the script failed.