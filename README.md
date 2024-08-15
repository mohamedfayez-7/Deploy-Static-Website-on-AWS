---

# Hosting a Static Website on AWS Using CloudFront and S3

## Overview

This guide explains how to host a static website on Amazon Web Services (AWS) using an S3 bucket for storage and CloudFront for content delivery.

## Prerequisites

- AWS Account
- AWS CLI installed and configured (optional, for command-line operations)
- Basic understanding of AWS services (S3, CloudFront)

## Steps

### 1. Create an S3 Bucket

1. **Log in to AWS Management Console**:
   Go to the [S3 Console](https://console.aws.amazon.com/s3/).

2. **Create a New Bucket**:
   - Click on "Create bucket."
   - Enter a unique bucket name (e.g., `my-static-website-bucket`).
   - Choose the AWS region where you want to create the bucket.
   - Click "Create bucket."

3. **Configure Bucket for Website Hosting**:
   - Go to the "Properties" tab of your bucket.
   - Click on "Static website hosting."
   - Select "Use this bucket to host a website."
   - Enter the index document name (e.g., `index.html`).
   - Optionally, specify an error document (e.g., `error.html`).
   - Click "Save changes."

4. **Upload Your Website Files**:
   - Go to the "Objects" tab of your bucket.
   - Click on "Upload" and add your website files (e.g., HTML, CSS, JavaScript).

5. **Set Permissions**:
   - Go to the "Permissions" tab of your bucket.
   - Edit the bucket policy to allow public access to your website files. Example policy:
     ```json
     {
       "Version": "2012-10-17",
       "Statement": [
         {
           "Sid": "PublicReadGetObject",
           "Effect": "Allow",
           "Principal": "*",
           "Action": "s3:GetObject",
           "Resource": "arn:aws:s3:::my-static-website-bucket/*"
         }
       ]
     }
     ```

### 2. Set Up CloudFront Distribution

1. **Create a CloudFront Distribution**:
   - Go to the [CloudFront Console](https://console.aws.amazon.com/cloudfront/).
   - Click on "Create Distribution."
   - Choose "Web" under "Web" distribution.

2. **Configure Distribution Settings**:
   - **Origin Domain Name**: Select your S3 bucket from the dropdown.
   - **Origin Path**: Leave this blank unless you want to serve content from a specific folder.
   - **Viewer Protocol Policy**: Choose "Redirect HTTP to HTTPS" or "HTTPS Only" for better security.
   - **Default Root Object**: Enter `index.html`.

3. **Configure Caching and Distribution Settings** (optional):
   - Adjust cache settings as needed to optimize performance.
   - Configure other settings such as logging and error pages as required.

4. **Create Distribution**:
   - Click "Create Distribution."
   - Wait for the distribution status to become "Deployed."

### 3. Update DNS Settings

1. **Get the CloudFront Distribution Domain Name**:
   - In the CloudFront Console, locate your distribution and copy the domain name (e.g., `d1234abcd8e0f.cloudfront.net`).

2. **Update Your DNS Records**:
   - Go to your DNS provider’s management console.
   - Create a CNAME record that points your domain name (e.g., `www.example.com`) to the CloudFront distribution domain name.

## Testing

- Visit your domain name (e.g., `www.example.com`) to verify that your static website is being served correctly through CloudFront.

## Troubleshooting

- **Content Not Loading**: Ensure that the S3 bucket policy allows public access and that the CloudFront distribution is correctly configured.
- **404 Errors**: Check that the `index.html` file is correctly named and uploaded in the S3 bucket.

## Security Considerations

- **S3 Bucket Permissions**: Regularly review and update bucket policies to ensure that only the necessary permissions are granted.
- **CloudFront Settings**: Utilize HTTPS to secure data in transit and configure caching to optimize performance.

---

S3 Bucket Website Hosting Endpoint:
http://my-748515101939-bucket.s3-website-us-east-1.amazonaws.com

Cloud Front Domain Name Endpoint:
d142dho7tzsudq.cloudfront.net
