# aws-cloud-quest

### 1. Migrate a stateless website to an AWS S3 Bucket
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

### 2. Migrate infrastructures to AWS EC2

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


