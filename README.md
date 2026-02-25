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
-----------
week-4

1.Creating an S3 bucket, upload an object, and enable public access 
1️⃣ Create an S3 Bucket (with Public Access Enabled)
Step 1: Open S3 Service
Login to AWS Management Console
Go to Services → S3
Click Create bucket

Step 2: Bucket Configuration
Bucket name: my-public-bucket-123
Region: Choose nearest region

Step 3: Disable Block Public Access (IMPORTANT)
Under Block Public Access settings for this bucket:
❌ Uncheck Block all public access
You must uncheck ALL options:
✔ Check I acknowledge that this bucket will become public
👉 Click Create bucket

2️⃣ Enable Public Access at Bucket Level (ACL)
Step 4: Open the Bucket
Click on the bucket name
Go to Permissions tab

Step 5: Edit Bucket ACL
Scroll to Access Control List (ACL)
Click Edit
Under Public access:
Everyone (public access) → ✔ Read access
✔ Save changes
📌 This allows public access at bucket level

3️⃣ Upload Object to the Bucket
Step 6: Upload File
Go to Objects tab
Click Upload
Click Add files
Select file (e.g., image.jpg)
Click Upload

4️⃣ Enable Public Access for Object (ACL)
Step 7: Object-Level ACL Permission
Click on uploaded object
Go to Permissions tab
Scroll to Access Control List (ACL)
Click Edit
Under Public access:
Everyone (public access) → ✔ Read access
✔ Save changes

5️⃣ Verify Public Access
Step 8: Test Object URL
Copy Object URL
Open in browser (incognito)
✔ Object should be accessible without login

2. S3 Versioning & Cross-Region Replication 

1️⃣ Enabling Versioning in S3 (with Output Scenario)
🔸 Objective
To enable version control on an S3 bucket and observe object versions.

🔸 Steps to Enable Versioning
1.Open AWS Management Console
2.Navigate to S3
3.Select the bucket
4.Go to Properties tab
5.Scroll to Bucket Versioning
6.Click Edit
7.Select Enable
8.Click Save changes
✔ Versioning is now enabled

🔸 Output Scenario (What You Observe)
Step A: Upload Object
Upload file: report.pdf
📌 Output:
Version ID is assigned automatically

Step B: Modify and Re-upload Same Object
Upload report.pdf again (same name)
📌 Output:
Old version is preserved
New version becomes current

Step C: Delete Object
Click Delete on report.pdf
📌 Output:
Object appears deleted
A Delete Marker is created
Older versions still exist and can be restored


2️⃣ Cross-Region Replication (CRR) 
🔸 Objective
To replicate objects from one bucket to another in a different region using AWS-managed permissions.

🔸 Prerequisites (Very Important)
✔ Versioning must be enabled on both buckets
✔ Source and destination buckets must be in different regions
✔ AWS Academy automatically handles replication permissions

Step 1️⃣ Create Destination Bucket
1.Go to S3 → Create bucket
2.Bucket name: dest-crr-bucket
3.Select different region
4.Complete bucket creation
5.Enable Versioning (same steps as above)

Step 2️⃣ Create Replication Rule (Without IAM Selection)
1.Open Source bucket
2.Go to Management tab
3.Click Replication rules
4.Click Create replication rule

Step 3️⃣ Configure Replication Rule
Rule name: academy-crr-rule
Status: Enabled
Scope: Apply to all objects

Step 4️⃣ Choose Destination Bucket
Select Another bucket
Choose Destination bucket
Confirm different region
📌 AWS Academy Note:
You will see an option like:
"AWS will create and manage the required permissions automatically"
✔ Select Use AWS-managed role / default role

Step 5️⃣ Replication Options
Replicate:
o✔ New objects
o❌ Existing objects (optional, depends on Academy permissions)
Encryption: Leave default
Click Save
✔ Replication rule is now active

3️⃣ CRR Output Scenario (What You Observe)
🔸 Step A: Upload New Object to Source Bucket
Upload file: image.png
📌 Output:
Object is uploaded normally

🔸 Step B: Verify Destination Bucket
Open destination bucket
Object image.png appears automatically
Version ID is present
✔ Replication successful


3.Static Website Hosting in S3
(Using Bucket-Level & Object-Level Permissions – ACL Method)

🔹 Objective
To host a static website in Amazon S3 using index.html and error.html, and allow public access using ACL-based bucket and object permissions.

🔹 Prerequisites
AWS / AWS Academy account
Two files:
oindex.html
oerror.html

1️⃣ Create an S3 Bucket
1.Login to AWS Management Console
2.Go to Services → S3
3.Click Create bucket
4.Enter:
oBucket name: my-static-site-bucket-123
oRegion: Nearest region
5.Under Block Public Access:
o❌ Uncheck Block all public access
o✔ Acknowledge the warning
6.Click Create bucket
✔ Bucket created successfully

2️⃣ Upload Website Files
1.Open the bucket
2.Go to Objects tab
3.Click Upload
4.Click Add files
5.Select:
oindex.html
oerror.html
6.Click Upload
✔ Files uploaded

3️⃣ Enable Static Website Hosting
1.Go to Properties tab
2.Scroll to Static website hosting
3.Click Edit
4.Select Enable
5.Choose Host a static website
6.Enter:
oIndex document: index.html
oError document: error.html
7.Click Save changes
✔ Static website hosting enabled

4️⃣ Enable Bucket-Level Public Access (ACL)
🔹 Purpose
Allows public users to access objects inside the bucket.

1.Go to Permissions tab
2.Scroll to Access Control List (ACL)
3.Click Edit
4.Under Public access:
oEveryone (public access) → ✔ Read
5.Click Save changes
✔ Bucket-level public read access enabled

5️⃣ Enable Object-Level Public Access (ACL)
Steps (For Each File)
1.Go to Objects tab
2.Click on index.html
3.Go to Permissions
4.Scroll to Access Control List (ACL)
5.Click Edit
6.Under Public access:
oEveryone (public access) → ✔ Read
7.Save changes
🔁 Repeat the same steps for error.html
✔ Object-level public access enabled

6️⃣ Access the Static Website
1.Go to Properties
2.Scroll to Static website hosting
3.Copy the Bucket website endpoint
4.http://my-static-site-bucket-123.s3-website-region.amazonaws.com
5.Paste into browser
✔ index.html page loads successfully

7️⃣ Error Page Output Scenario
🔹 Test Error Page
Open an invalid URL:
http://bucket-name.s3-website-region.amazonaws.com/invalid.html
📌 Output:
error.html page is displayed
✔ Error page working correctly
