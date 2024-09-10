Hosting a Static Website on Amazon S3 using CloudFront and Route 53

These are the steps I used to host a static website on Amazon S3, use CloudFront for content delivery, and Route 53 for domain management.

Prerequisites

An AWS account.
A registered domain name (can be registered via Route 53 or another domain registrar).
AWS CLI is installed and configured on your machine.
Step 1: Create and Configure an S3 Bucket

Create an S3 Bucket:

Go to the S3 service in the AWS Management Console.
Click "Create bucket".
Enter a unique bucket name (e.g., your-domain.com).
Choose a region and click "Create bucket".
Configure the Bucket for Static Website Hosting:

Select your bucket and go to the "Properties" tab.
Scroll down to the "Static website hosting" section and click "Edit".
Select "Enable", and choose "Host a static website".
Enter the index document (e.g., index.html) and optionally an error document (e.g., error.html).
Click "Save changes".
Set Bucket Permissions:

Go to the "Permissions" tab.

Click "Bucket Policy" and paste the following policy (replace your-bucket-name with your actual bucket name):
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowPublicReadAccess",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::your-bucket-name/*"
    }
  ]
}


Click "Save changes".

Upload Website Files:

Go to the "Objects" tab.
Click "Upload", add your website files (e.g., index.html, styles.css, etc.), and click "Upload".
Step 2: Create a CloudFront Distribution

Create a CloudFront Distribution:

Go to the CloudFront service in the AWS Management Console.
Click "Create Distribution".
Select "Web" for the delivery method.
For the origin domain, enter your S3 bucket's website endpoint (e.g., your-bucket-name.s3-website-us-east-1.amazonaws.com).
Keep the defaults in the "Default Cache Behavior Settings" section or adjust as needed.
Enter your domain name in the "Distribution Settings" section (e.g., www.your-domain.com).
Click "Create Distribution".
Note the CloudFront Domain Name:

Once the distribution is created, note the CloudFront domain name (e.g., d1234567890.cloudfront.net).
Step 3: Configure Route 53

Create a Hosted Zone:

Go to the Route 53 service in the AWS Management Console.
Click "Create hosted zone".
Enter your domain name (e.g., your-domain.com) and click "Create hosted zone".
Create an Alias Record for CloudFront:

Select your hosted zone and click "Create record".
Select "Simple routing".
Click "Define simple record".
Enter your subdomain name (e.g., www).
Select "Alias to CloudFront distribution" for the record type.
Select the CloudFront distribution you created earlier in the "Route traffic to" section.
Click "Define simple record" and then "Create records".
Update Domain Registrar:

If your domain is registered with a registrar other than Route 53, update the name servers to the ones provided by Route 53 for your hosted zone.
Step 4: Verify the Setup

Propagation Time:

DNS changes may take some time to propagate. Wait for the changes to take effect.
Access Your Website:

Open a web browser to your domain (e.g., www.your-domain.com).
Your static website should be displayed.
Challenges Faced: I had issues with the Route 53 setup since I had not created records using the CName which I solved it by creating records in Route 53.

Learning Resources/References. AWS documentation. YouTube Channel
