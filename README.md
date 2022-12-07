# aws-cloud-quest

## 1. Migrate a stateless website to an AWS S3 Bucket
1. Log into AWS
2. Go to S3 (Simple Storage Service) 
3. Create a Bucket and save the name on your notepad (e.g new-bucket-for-my-github-documentation)
4. Choose the Region "US East (N. Virginia) us-east-1"
5. In the Object Ownership section, choose "ACL enabled", then "Object Writer"
6. Deselect "Block all public access" and check the "acknowledgment" checkbox
7. Enable encryption and keep the default config "Amazon S3-managed keys (SSE-S3)"
8. Click Create Bucket
9. A green banner will let you know that the bucket has been successfully created. 
10. Click View Details from the banner
11. Click Upload, Add Files, Upload, then Close
12. If you want to edit a file, select the file, click "Actions", and for example "Rename File"
13. Click on the tab "Properties", then at the very bottom of the page on click Edit on the block "Static website hosting"
14. Enable static site hosting, type the name of your index document (index.html), and save
15. Click on the tab "Permissions"
16. Make sure the "Block all public access" is off
17. Click edit on the Bucket policy
18. Copy your bucket ARN
19. Enter this policy code with your updated bucket ARN, then save changes:

```
{
  "Id": "StaticWebPolicy",
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "S3GetObjectAllow",
      "Action": [
        "s3:GetObject"
      ],
      "Effect": "Allow",
      "Resource": "Your_Bucket_ARN/*",
      "Principal": "*"
    }
  ]
}
```
20. Click Properties
21. Scroll all the way down and click on the URL of your [stateless website hosted in AWS S3](http://new-bucket-for-my-github-documentation.s3-website-us-east-1.amazonaws.com/)!
![2](https://user-images.githubusercontent.com/48727972/203665691-6c87c817-aa01-43d1-a7e0-48f0dbb7ebf7.png)


## 2. Migrate infrastructures to AWS EC2

1. Go to EC2 (Elastic Compute Cloud)
2. Click "Launch Instance" x2
3. Name your server, e.g webserver01
4. Under "Quick Start" make sure that Amazon Linux is selected
5. Make sure that the Amazon Mahcine Image is Amazon Linux 2 AMI
6. Make sure that the Instance Type is t2.micro
7. Under Key Pair, select "Proceed without a key pair"
8. Under Network Settings, click Edit
9. Keep VPC and subnet
10. Security Group Name: Security-Group-Lab
11. Description: HTTP Security Group
12. Security Group Rule, Type: HTTP
13. Under configuration storage, make sure you are on the free tier
14. Open the Advanced Details dropdown menu
15. Copy Paste the content of your user data:

```
#!/bin/bash
sudo yum update -y
sudo yum install -y httpd
sudo yum install -y git
export META_INST_ID=`curl http://169.254.169.254/latest/meta-data/instance-id`
export META_INST_TYPE=`curl http://169.254.169.254/latest/meta-data/instance-type`
export META_INST_AZ=`curl http://169.254.169.254/latest/meta-data/placement/availability-zone`
cd /var/www/html
echo "<!DOCTYPE html>" >> index.html
echo "<html lang="en">" >> index.html
echo "<head>" >> index.html
echo "    <meta charset="UTF-8">" >> index.html
echo "    <meta name="viewport" content="width=device-width, initial-scale=1.0">" >> index.html
echo "    <style>" >> index.html
echo "        @import url('https://fonts.googleapis.com/css?family=Open+Sans&display=swap');" >> index.html
echo "        html {" >> index.html
echo "            position: relative;" >> index.html
echo "            overflow-x: hidden !important;" >> index.html
echo "        }" >> index.html
echo "        * {" >> index.html
echo "            box-sizing: border-box;" >> index.html
echo "        }" >> index.html
echo "        body {" >> index.html
echo "            font-family: 'Open Sans', sans-serif;" >> index.html
echo "            color: #324e63;" >> index.html
echo "        }" >> index.html
echo "        .wrapper {" >> index.html
echo "            width: 100%;" >> index.html
echo "            width: 100%;" >> index.html
echo "            height: auto;" >> index.html
echo "            min-height: 90vh;" >> index.html
echo "            padding: 50px 20px;" >> index.html
echo "            padding-top: 100px;" >> index.html
echo "            display: flex;" >> index.html
echo "        }" >> index.html
echo "        .instance-card {" >> index.html
echo "            width: 100%;" >> index.html
echo "            min-height: 380px;" >> index.html
echo "            margin: auto;" >> index.html
echo "            box-shadow: 12px 12px 2px 1px rgba(13, 28, 39, 0.4);" >> index.html
echo "            background: #fff;" >> index.html
echo "            border-radius: 15px;" >> index.html
echo "            border-width: 1px;" >> index.html
echo "            max-width: 500px;" >> index.html
echo "            position: relative;" >> index.html
echo "            border: thin groove #9c83ff;" >> index.html
echo "        }" >> index.html
echo "        .instance-card__cnt {" >> index.html
echo "            margin-top: 35px;" >> index.html
echo "            text-align: center;" >> index.html
echo "            padding: 0 20px;" >> index.html
echo "            padding-bottom: 40px;" >> index.html
echo "            transition: all .3s;" >> index.html
echo "        }" >> index.html
echo "        .instance-card__name {" >> index.html
echo "            font-weight: 700;" >> index.html
echo "            font-size: 24px;" >> index.html
echo "            color: #6944ff;" >> index.html
echo "            margin-bottom: 15px;" >> index.html
echo "        }" >> index.html
echo "        .instance-card-inf__item {" >> index.html
echo "            padding: 10px 35px;" >> index.html
echo "            min-width: 150px;" >> index.html
echo "        }" >> index.html
echo "        .instance-card-inf__title {" >> index.html
echo "            font-weight: 700;" >> index.html
echo "            font-size: 27px;" >> index.html
echo "            color: #324e63;" >> index.html
echo "        }" >> index.html
echo "        .instance-card-inf__txt {" >> index.html
echo "            font-weight: 500;" >> index.html
echo "            margin-top: 7px;" >> index.html
echo "        }" >> index.html
echo "    </style>" >> index.html
echo "    <title>Amazon EC2 Status</title>" >> index.html
echo "</head>" >> index.html
echo "<body>" >> index.html
echo "    <div class="wrapper">" >> index.html
echo "        <div class="instance-card">" >> index.html
echo "            <div class="instance-card__cnt">" >> index.html
echo "                <div class="instance-card__name">Your EC2 Instance is running!</div>" >> index.html
echo "                <div class="instance-card-inf">" >> index.html
echo "                    <div class="instance-card-inf__item">" >> index.html
echo "                        <div class="instance-card-inf__txt">Instance Id</div>" >> index.html
echo "                        <div class="instance-card-inf__title">" $META_INST_ID "</div>" >> index.html
echo "                    </div>" >> index.html
echo "                    <div class="instance-card-inf__item">" >> index.html
echo "                        <div class="instance-card-inf__txt">Instance Type</div>" >> index.html
echo "                        <div class="instance-card-inf__title">" $META_INST_TYPE "</div>" >> index.html
echo "                    </div>" >> index.html
echo "                    <div class="instance-card-inf__item">" >> index.html
echo "                        <div class="instance-card-inf__txt">Availability zone</div>" >> index.html
echo "                        <div class="instance-card-inf__title">" $META_INST_AZ "</div>" >> index.html
echo "                    </div>" >> index.html
echo "                </div>" >> index.html
echo "            </div>" >> index.html
echo "        </div>" >> index.html
echo "</body>" >> index.html
echo "</html>" >> index.html
sudo service httpd start
```
16. Make sure the number of instances is 1
17. Click Launch Instance
18. A green banner appears letting you know that your launch was successful
19. Click View All Instances
20. Select your instance, make sure your instance is running
21. Copy your public IPv4 DNS (not the address!) and paste it into your broswer, e.g. ec2-18-144-7-69.us-west-1.compute.amazonaws.com

![1](https://user-images.githubusercontent.com/48727972/203665630-922d854e-d31a-4eaf-99ab-8fdda3fa5af2.png)

## 3. Increase the AWS EC2 Instance Size

|| Summary ||
1. Go to AWS EC2
2. Stop your instance
3. Update your instance type
4. Re-start your instance

|| Extended ||
1. Go to AWS EC2
2. Click Instances from the left menu
3. Select your Instances and review the Details tab
4. Click Instance Types from the left menu
5. Search for the t3.large, r5.large, and c5.large types
6. Select all results and review details (compute, networking, storage, accelerator, and price)
7. Go back to Instances from the left menu and copy the Public IPv4 Address of your instance
8. Open your Public IPv4 in your browser:
- ![1](https://user-images.githubusercontent.com/48727972/204032280-dc359b6e-ce2f-4083-b251-750556f110bb.png)
9. Go back to Instances from the left menu, select your instance and click Connect
10. Review the EC2 Instance Connect tab, the Session Manager tab, and the SSH Client tab
11. Go to the EC2 Instance Connect tab and click Connect. 
12. The terminal opens in a new tab. Type:
``` cd sample_add
ls \\to view the files in the directory
tail -1f aws_compute_solutions.log \\to check the instance log
```
13. Go back to Instance, select it, click Actions > Instance Settings > Edit user data
14. Review the commands in the current user data and then click Cancel
```
#!/bin/bash
# Update and install python3
sudo yum update -y
sudo yum install -y python3

#Export environment variables
export APP_NAME=sample_app
export LAB_ID=399068fe-9ca4-42de-a351-3d0048824a10
export PROVISION_BUCKET_NAME=pu-base-buckets-v1-provision-lab

#Declare files and create directory
EC2_FILES="lab app.py requirements.txt templates/index.html"
mkdir /home/ec2-user/$APP_NAME
mkdir /home/ec2-user/$APP_NAME/templates

#Copy files from S3
for file_ in $EC2_FILES
do
aws s3 cp s3://$PROVISION_BUCKET_NAME/$LAB_ID/$APP_NAME/$file_ /home/ec2-user/$APP_NAME/$file_
done

# Install and start app
mv /home/ec2-user/$APP_NAME/lab /etc/rc.d/init.d/
chmod +x /etc/rc.d/init.d/lab
chkconfig lab on
sudo chown -R ec2-user:ec2-user /home/ec2-user/$APP_NAME
sudo pip3 install -r /home/ec2-user/$APP_NAME/requirements.txt
service lab start
```
15. Go back to Instances, Click Instance State > Stop Instance > Stop (To change your instance, you must fist stop it)
16. Under the details tab, the Public IPv4 address and DNS are now empty
17. Select Instance > Click Actions > Instance Settings > You can change instance type, e.g m4.large, termination protection, or shutdown behavior
18. Then restart the instance by clicking on Instance > Instance State > Start Instance. The Public IPv4 address and DNS are now generated


## 4. Create price estimates with the AWS Pricing Calculator
1. Go to https://calculator.aws
2. Click Create Estimate
3. Click My Estimate (top left corner)
4. Click Create Group and rename it to Web Services
5. Click Add Service
6. Under Find Service, type ec2, then click Configure on the Amazon EC2
7. Under Description, type Web Server Estimate
8. Make sure the location type is Region; 
9. Select N. Virginia
10. Click on Advanced Estimate
11. Make sure the OS is Linux
12. Under Workload, choose Daily Spike Traffic
12. Under Workload days, select every single day
13. Baseline = 2; Peak = 4; Duration of peak = 8 and 0
14. Under EC2 Instance: vCPUs = 2; GiB = 4; Any Networkwork Performance
15. Choose the instance type t2.medium
16. Pricing Model = On-Demand
17. Click Show Calculation
18. Click Estimated Workload Hours and review the info, including the # of hours in the utilization summary, then close
19. Under Amazon Elastic Block Storage (EBS): General Purpose SSD (gp2); Storage Amount 30GB; Snapshot Frequency Weekly; Amount changed per snapshot 1GB
20. Under Data Transfer: InBound Data Transfer internet (free) 1TB per month; Outbound Data Transfer Internet 0.05 - 0.09 USD per GB, 100GB per month
21. Click Show Calculation, review your data, then click Save and Add Service
22. Click View Summary
23. Click Share > Agree and Continue > Copy public link 

## 5. Troubleshoot and connect Web Server and Database Server
1. Go to EC2
2. Click Instances
3. Select the Web Server and copy the Public IPv4 Address > Open it on another tab > error connection times out
4. Go to the Networking tab and copy the Private IPv4 address and the Public IPv4 address
5. Click on the subnet ID
6. Select the network-concept-subnet, click Route Table tab, then click on the route table link
7. Select Route Table > Routes tab > Edit Routes
8. Click Remove to delete the NAT gateway from the route table
9. Add route > Destination 0.0.0.0/0; Target Intenet Gateway becomes igw-xxxx > Save Changes
10. Review the new gateway association at the bottom of the page: the subnet is now reachable
11. Click Services (top left corner) > EC2 > Instances
12. Select Web Server > Security Tab > Click link of the webserversecuritygroup under Security groups
13. Click Edit inbound rules
14. Click Add Rule > Update Type Custom TCP to HTTP > Update source to anywhere IPv4 > Save rules
15. Click Instance > Select Web Server > Networking tab > Copy Public IPv4 address > Paste into a new tab
16. The connection from the internet to the Web Server is successful. The connection between the Web Server and the Database Server failed:
-  ![3](https://user-images.githubusercontent.com/48727972/204059953-45bf2d10-5962-4a7d-9092-a4b6f6535374.png)
17. Change the security group rules to allow traffic over port 3306 with custom source subnet 10.10.0.0/24
- ![6](https://user-images.githubusercontent.com/48727972/204060261-066c929b-1a33-43b0-8f68-03ac04a14bae.png)

## 6 Allow communication between AWS VPCs with VPC Peering

|| starts at #14 ||

1. Go to AWS VPC
2. Click Your PVCs and review the current VPCs
3. Go to AWS EC2
4. Click Instance (running)
5. Click on Financial VPC (it currently doesn't have access to internet, no public IPv4 and private subnet)
6. Click on Marketing VPC and check its VPC ID
7. Click Connect > EC2 Instance Connect Tab > Connect
8. Type ping 172.32.0.10 (connect to the private IPv4 Private address of Financial)
9. There is no connection because Financial server doesn't have an IPv4 Public address. Ctrl + C to close 
10. Go back to EC2 > Marketing Server > Click on its ID
11. Click on the subnet ID (A subnet is a range of IP addresses in the VPC)
12. Select the checkbox of the subnet > Click on the name of the route table
13. Click Route tab. There are two routes, one for the local traffic and one for the internet traffic through the internet gateway
- ![1](https://user-images.githubusercontent.com/48727972/204885341-a0fdec11-7ff6-48a1-97e2-a20332d9b02b.png)
14. Click Peering Connection (left menu)
15. Click Create Peering Connection
16. Name = marketing <> finance
17. Select a local VPC to peer with (marketing VPC). Make sure Marketing VPC has 10.10.0.0/16 as its CIDR range
19. VPC ID (Accepter) = Finance VPC. Make sure Marketing VPC has 172.31.0.0/16 as its CIDR range (copy the CIDR)
20. Click Create Peering Connection
21. Click Peering Connection (left menu) > Select marketing <> finance > Make sure the status is Pending Acceptance
22. Click drop down menu Actions > Accept Request > Accept Request
23. Click Route Tables (left menu) > Select MarketingPublicSubnet1 > Route tab > Edit Route
24. Click Add Route > Destination = 172.31.0.0/16 > target = pcx- (autopopulates with marketing <> finance) > Saves changes
25. Click Route tables (left menu) > Select FinancePrivateSubnet1 > Route tab > Edit Route
26. Click Add Route > Destination = 10.10.0.0/16 (CIDR)> target = pcx- (autopopulates with marketing <> finance) > Saves changes
27. Go to EC2 > Instance > Select Marketing Server > Connect > Connect
28. Type ping 172.32.0.10 (they peered but the security specs don't allow them to connect just yet)
29. Instances > Financial Server > Security tab > Click on the Security server named FinanceServerSecurityGroup
30. Click Edit inbound rules > Click Add rule > Custom TCP = All ICMP IPv4 > Source = 10.10.0.0./16 (CIDR)> Save rules
31. Go to EC2 > Instance > Select Marketing Server > Connect > Connect
32. Type ping 172.31.0.10 > successful connection!
- ![2](https://user-images.githubusercontent.com/48727972/204892075-fe493c09-4744-44fe-9b4c-7a7055f968a5.png)

## 7. Improve Database Infrastructure with AWS RDS (use multiple Availability Zones (AZ) and a read replica)

1. Go to AWS RDS (Relational Database Service)
2. Click Database (left menu) > Create Database
3. Click StandardCreate > MariaDB > leave the engine version untouched > Template = Dev/Prod > DB Instance Identifier = my-database > Enter credentials > DB Instance Class = burstable and db.t3.xlarge > Storage Type = General Purpose SSD > Allocated Storage = 20 > Enable storage autoscaling > Maximum Storage Threshold = 1000 > Multi-AZ Deployment = Create a standby instance > VPC = default > subnet = default 
4. Click Additional Configuration to expand > Initial database name = my_database > Enable encryption > uncheck performance insight > uncheck monitoring > uncheck enable auto mirror version upgrade > Maintenance window = no preference > Click Create Database
5. It takes 15-20min for the database to be created. Click refresh icon if needed 
6. Click my-database
7. Click Action > Create read-replica
- ![7](https://user-images.githubusercontent.com/48727972/204912221-b9b15d15-4983-455c-8676-ed5377573140.png)

## 8. Create an AWS DynamoDB Database (NoSQL)

1. Go to AWS DynamoDB
2. Click Create Table
3. DB name = userVideoHistory > partition key = userId (string) > sort key = lastDateWatched (number) > Default Settings > Create Table
4. Once the status changes to Active, click the table name
5. Click Actions > Create item (make sure the db is selected) > userId = 12345-abcd-6789 > lastDateWatched = 1619156406 (UNIX stamp)
6. Click Add new attribute > String > attribute name = videoId > value = 9875-djac-1859
7. Click Add new attribute > String > preferredLanguage = English
8. Click Add new attribute > List > attribute Name = supportedDeviceTypes > Insert a field = String 
9. Click on the + icon next to supportedDeviceTypes and add Amazon Fire TV
10. Insert another field type string > cick on the + icon > Amazon Fire Tablet > Click Create Item 
11. Click on the userId 12345-abcd-6789 (you should be on the Explore item left menu) to edit it
12. Click Add new attribute > Number > Atrribute name = lastStopTime > Value = 90 > Save Changes
13. Click Scan/Query items to expand it > Click Query > userId = 12345-abcd-6789 > Greater than 1609477200
14. Click Scan and Run to see all the items in your DynamoDB table
15. You can create new items with new ids and new attributes
- ![6](https://user-images.githubusercontent.com/48727972/204923747-aa5b8e7e-b48c-4d2a-84e4-0006cdf10242.png)
- ![5](https://user-images.githubusercontent.com/48727972/204923864-255a9f1e-b47a-478f-a0d5-ffed710298ca.png)

## 9. AWS IAM (Identity and Access Management)

|| create users, groups, and permissions ||

1. Go to AWS IAM (Identity and Access Management)
2. Click User Groups (left pannel) > Click Create Group > User group name = SupportEngineers > Search for the Attach Permissions Policies = AmazonEC2ReadOnlyAccess > Select it > Click Create Group
3. Click Users (left pannel) > Click Add Users > user name = support-engineer-1 > Click and enable Access Key - Programmatic Access > Click and enable password AWS Console Management Access > Choose Custom password > Enter a password > Uncheck Require Password Reset > Click Next: Permissions
4. Click Add user to group > Select SupportEngineers > Click Next: Tags
5. To add Tag, Key = job title > Value = Support Engineer > Click Next: Review > Click Create User
6. Copy the AWS Console Management Access link (e.g. https://140768770893.signin.aws.amazon.com/console) > Click Close
7. Open a new incognito window and paste the link. Enter your username support-engineer-1 and password > 
8. As your new user, click EC2 > Make sure your region is US East (N. Virginia) 
9. Click instances (running) > Choose the Web Server > Instance State > Terminate Instance > Terminate > Error confirms that you are not able to termine the instance
10. You can add new permissions by clicking on User Group > select it > Permission tab > Add permission > Search it > Click it > Add permission
- ![5](https://user-images.githubusercontent.com/48727972/205126529-cd43df8f-6fcb-4c0f-af89-de32f508d49d.png)


## 10. Create a File System with AWS EFS (ELastic File System)

|| The AWS EFS will need to be accessible by 3 servers. Mount EFS into 2 EC2 Instances ||

1. Go to AWS EC2
2. Click Instances (left pannel) > Review the three instances and their availability zones 
3. Click Security Groups (left pannel) > Create Security Group > Security Group Name = PetModels-EFS-1-SG > Description = Restrict access to webservers only > Click X to remove the existing VPC and update it > Under Inbound rules, click Add Rule > Type = NFS (Network File System) > Source = security groups is web service > Create Security Group > Review the name of the security group
4. Go to AWS EFS
5. Click Create File System > Name = PetModels-EFS-1 > VPC = Pet models > Click Customize > Disable enable automatic backup > Transition into AI = none > Click Next > Remove all Mount Targets > Keep us-east-1a with security group petModel > Click Next > Click Next > Click Create
6. A green banner confirms that the EFS was created with a name > Click on the name of the EFS > Click Attach > Copy onto a separate notepad the script unedr EFS mount helper (below) > Click Close
```
sudo mount -t efs -o tls fs-03c1e1d600c76d897:/ efs)
```
7. Go to AWS EC2
8. Click Instances (left pannel)
9. Select PetModels-A > Click Connect > Click Connect > Enter Script (below)
```
sudo yum install -y amazon-efs-utils
mkdir data
ls
sudo mount -t efs -o tls fs-03c1e1d600c76d897:/ data
cd data
sudo bash -c "cat >> efs-1-setup.log"
efs-1 mounted in site A
Ctrl + C (to end)
cat efs-1-setup.log
```
- In this step you used Linux commands to create a data directory, then mounted the newly created Amazon EFS file system to that directory. You created a log file and appended information to it. The log file and its contents will be visible from other instances that have the same file system mounted.
10. Go to EFS
11. Click on the name of the EFS > Click Network tab > Click Manage > Click Add Mount Target > Choose the subnet and the Security Group that you created > Click Save > The new network will have the status Available (refresh if needed)
12. Go to AWS EC2
13. Repeat step 9 for each other instance

## 11. AWS EC2 Auto Scaling (min, max, schedule, policy, monitoring)

1. Go to EC2
2. Click Instances (left pannel) > Click the Instance ID > Copy the Public IPv4 address > Open a new tab > Type http:// and paste the public IPv4 address and make sure the server is up
3. Go back to Instances (left pannel) > Select your instance > Click Actions > Images and templates > Create Image > name = GameServer > Description = Regular customer game server > Make sure Tag image and snapshots together is selected > Click Create Image 
4. Click AMI (left pannel) > Make sure the Image has been created > Refresh until the status is Available
5. Click Launch Template (left pannel) > Click Create Launch Template > Launch Template Name = GameServerTemplate > Template Version Description = Regular customer game server template > Select Auto Scaling Guidance >  Under Application and OS Images > My AMIs tab > Own by me > Choose the AMI GameServer > Instance type = t2.nano > Click Create a new key pair > name = GameServerKeyPair > Select RSA > Select .pem (if you use Linux and MaOS) or .ppk (Windows) > Click Create Key Pair > Under Network Settings, choose the Security Group WebServerSecurityGroup > Click Create launch template > Click View Launch Templates
6. Click Auto Scaling groups (left pannel) > Create Auto Scaling Group > Name = RegularCustomerGameServer > Lauch Template = GameServerTemplate > Click Next > VPC = autos scaling and healing game server VPC > Choose both subnets > Click Next > Select No Load Balancer > Health check grace period = 240 sec > Click Next > Group Size Desired capacity = 2 > Min = 2 > Max = 4 > Scaling Policy = RegularCustomerGameServer > Scaling policy name = CPU Utilization > Metric type = average CPU utilization > target value = 70 > Click Next > Click Skip to review > Click create auto scaling group 
7. Click on the new Auto scaling group > Click Activity tab > Make sure that two instances were created under Activity History, for desired and actual capacity > Click Automatic Scaling tab > Click Create Scheduled Action > Name = SecondWaveOfRegulars > Desired Capacity = 3 > Min = 3 > Max = 4 > Recurrence = every week > Update Specific Start time and hour > Click create
8. You can create a new scheduled action with a max of 0 at 1am to end the server access for a group

## 12. Highly Available Web Applications

|| Create an Auto Scaling group Caccross multiple Available Zones ||
|| Create an Elastic Loan Balancer (ELB) ||
|| Attach an auto scaling group to an ELB ||
|| Create an ELB Health check for EC2 Instances ||

1. Go to AWS EC2
2. Click Auto-scaling groups (left pannel)
3. Click the auto-scaling group and check the max min capacity. 
4. Click Instance Management tab and make sure there is at least one instance
5. Click Details tab > Under Load Balancing click Edit > Click Add a new loan balancer > Choose Application Loan Balancer > Choose Internet facing > Under Availability Zones and subnets, choose all three Zones > Under Default Routing, choose Create a target group > Click Update
6. Click Security Groups (left pannel) > Click Create Security Group > Type Name > Type Description > Choose VPC > Under Inbound rules, click Add Rule > Type = HTTP > Source = 0.0.0.0/0 > Outbound rule type = HTTP > Source = security group > Click Create Security Group
7. Click Security Groups (left pannel) > Select the web server security group > Click Actions > Edit inbound rules > Delete > Add rule > Type = HTTP > Source = security group load balancer > Click Save rules
8. Click Load Balancer (left pannel) > Click on the loan balancer > Click Security Tab > Click Edit Security Groups > Add the loan balancer security group and remove the web server security group > Click Save changes > Copy the DNS of the Amazon Load Balancer (ALB) > Paste the DNS in a new tab to confirm that the website is up and behind an ALB > add /healthy at the end of the url to do a health check on the root path
9. Click Target Groups (left pannel) > Select the existing target group > Click Health check tab > Click Edit > type /health in the health check path > Click Advanced health check settings > Unhealthy threshold = 2 > Timeout = 2 > Interval = 5 > Click Save changes
10. Click Auto Scaling Groups (left pannel) > Select the group > Scroll down to Network and click Edit > update from public to private subnet
11. Click Instances (left pannel) > Select the instance > Click Instance State > Terminate instance (this instance is in the public subnet) > Wait for a few minutes > A new instance has been automatically created, with a private subnet > 
12. Click Auto Scaling groups (left pannel) > Select the instance > Click Activity tab > Make sure the status is successful and terminated for the old instance > refresh the website tab, the instance number is updated > now click on Network and Edit > Add both private subnets > Click Update > Now click Group Details > Edit > desired capacity = 2, min = 1, max = 2 > Click Update > Review the successful status of both instances under the Activity tab
13. Click Instances (left pannel) > Choose newest instance > Click Networkingn tab > make sure it is in the private subnet 2 > 
14. To add new private subnets, make sure to increase your desired capacity, and then refresh your Instance page :)
