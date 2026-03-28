# 1) Access the Public EC2 and secure identity
scp -i /root/.ssh/KEY.pem /root/.ssh/KEY.pem ubuntu@PUBLIC_IP:/home/ubuntu/.ssh/KEY.pem
ssh -i /root/.ssh/KEY.pem ubuntu@PUBLIC_IP
chmod 400 /home/ubuntu/.ssh/KEY.pem

# 2) Jump to the Private EC2 and create a dedicated transfer key
ssh -i /home/ubuntu/.ssh/KEY.pem ubuntu@PRIVATE_IP
ssh-keygen -t ed25519 -f ~/.ssh/log_transfer_key -C "log-transfer" -N ""
cat ~/.ssh/log_transfer_key.pub

# 3) Exit back to Public EC2, Authorize the new targeted Key
echo 'ssh-ed25519 PASTE_PUBLIC_KEY_HERE log-transfer' >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys

# 4) On Private EC2, Set up SCP Cron task (Every Minute)
crontab -e
* * * * * /usr/bin/scp -i /home/ubuntu/.ssh/log_transfer_key -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null /var/log/boots.log ubuntu@PUBLIC_IP_INTERNAL:/tmp/boots.log >> ~/scp-transfer.log 2>&1

# 5) On Public EC2, Set up S3 Upload Cron task
sudo apt update && sudo snap install aws-cli --classic
crontab -e
* * * * * /snap/bin/aws s3 cp /tmp/boots.log s3://YOUR_BUCKET/boot/boots.log >> ~/s3-upload.log 2>&1