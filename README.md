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
19. Enter this policy code with your updated bucket ARN, then save changes

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
