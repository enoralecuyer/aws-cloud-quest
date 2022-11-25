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


### 4. AWS Pricing Calculator
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
