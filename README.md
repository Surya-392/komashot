# komashot
Step 1: Launching EC2 Instances
1. Sign in to AWS Management Console.
2. Go to the EC2 dashboard.
3. Click on "Launch Instance".
4. Configure instance details:
   - Instance Name: EFS-1
5. Choose an Amazon Machine Image (AMI) for Linux.
6. Select instance type: t2.micro.
   - Key Pair: EFS
   - Network: Choose Subnet-1a
   - Security Group: Create a new security group (SG) and add NFS and allow from anywhere.
7. Launch the instance.
8. Repeat the above steps to launch another instance named EFS-2 in Subnet-1b with the same configuration.
Step 2: Creating an EFS File System
1. Go to the EFS service in the AWS Management Console.
2. Click on "Create file system".
3. Specify details:
   - Name: Optional
   - VPC: Default
   - Enable region button.
Click on "Next".
4. In the Network settings:
   - Delete all security groups.
   - Select the newly created security group (NFS).---which is created while creating instance
 Click on "Next" and review the configuration.
Click on "Create file system".
Step 3: Accessing the two EC2 instances named EFS-1 & EFS -2 in two different PowerShell sessions and performing the specified tasks:
Accessing EFS-1 Instances in Two Different PowerShell Sessions:
1. Open Two PowerShell Sessions:
   - Open two separate PowerShell windows or tabs on your local machine.
For each  Instance:
2. SSH into the Instance:
   - Use the SSH command to connect to the EFS-1 instance
          ssh -i [path-to-your-keypair.pem] ec2-user@[instance-public-ip]
3. Switch to Root User:
   - Gain root access by executing the following command:
         sudo su
    4. Create a Directory:
   - Make a directory named "efs" using the following command:
          mkdir efs
     5. Install Amazon EFS Utilities:
   - Use yum package manager to install the Amazon EFS utilities:
         yum install -y amazon-efs-utils
     6. List Files:
   - Execute the following command to list files in the current directory:
          ls
     7. Verify Installation (Optional):
   - Optionally, you can verify the installation of the Amazon EFS utilities by checking the version:
         efs-utils --version
     Repeat Steps 2-7 for -------------------------  EFS-2 Instance.
By following these steps, you'll have accessed each EFS-1 instance in separate PowerShell sessions, switched to the root user, created a directory named "efs," installed the Amazon EFS utilities, and listed files in the directory. This setup allows you to configure and manage each instance individually as needed.
Step 4: Attaching EFS to EC2 Instances
1. Go to the EFS service in the AWS Management Console.
2. Click on the target EFS file system.
3. Click on the "Attach" button.
4. Choose "Mount via DNS" option.
5. Copy the displayed command. ,on both ec2 instances(powershell)
6. Paste and execute the copied command in the terminal to mount the EFS file system onto the instance.
Step 5: Verify and Test EFS
1. Change the directory to EFS on both instances.
2. Create a file on one instance.
3. Verify that the file automatically synchronizes and appears on the other instance
