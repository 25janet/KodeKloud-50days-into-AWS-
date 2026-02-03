EBS volumes are attached to an EC2 instance , it is a virtual hard disk for the instance.

They are persistent even if you terminate the EC2 instance , it won't be deleted 

Except if it is  a root EBS volume is deleted once its instance is terminated.

EBS volumes can be encrypted  using KMS that is: data at rest, data in transit and also snapshots are encrypted

EBS  are AZ locked, meaning that you can only attach  in one AZ , and also that AZ must be the same as the required instance

If you want to move it to another AZ create a snapshot and restore the snapshotin that new AZ

EBS  volumes are different types and provide different functionalities that is : 
            gp3/gp2 = general purpose
            io1/io2 = high IOPS,mission-critical databases
            st1 = throughput optimized
            sc1 = cold HDD
Brand new EBS volumes are empty one connect to the instance(ssh -i key.pem ec2-user@<public-ip>):
 
            (i). check if the volume is detected -lsblk

            (ii). must create a file system - sudo mkfs -t ext4 /dev/sdb

            (iii).create a mount point - sudo mkdir /data

            (iv).mount the volume - sudo mount /dev/sdb /data

            (v). verify - df -h