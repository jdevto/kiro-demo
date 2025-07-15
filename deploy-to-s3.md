# Deploy Kiro IDE Website to AWS S3

This guide will help you deploy your static website to AWS S3 and make it publicly accessible.

## Prerequisites

1. AWS CLI installed and configured
2. An AWS account with appropriate permissions

## Step 1: Create S3 Bucket

```bash
# Create bucket (use a unique name if kiro-ide-website is taken)
aws s3 mb s3://kiro-ide-website --region ap-southeast-2
```

## Step 2: Disable Block Public Access (MUST BE DONE FIRST)

**Important**: This must be done BEFORE applying the bucket policy:

```bash
aws s3api put-public-access-block --bucket kiro-ide-website --public-access-block-configuration "BlockPublicAcls=false,IgnorePublicAcls=false,BlockPublicPolicy=false,RestrictPublicBuckets=false"
```

## Step 3: Apply Bucket Policy for Public Access

The `bucket-policy.json` file should contain:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::kiro-ide-website/*"
        }
    ]
}
```

Apply the policy:

```bash
aws s3api put-bucket-policy --bucket kiro-ide-website --policy file://bucket-policy.json
```

## Step 4: Configure Static Website Hosting

```bash
aws s3 website s3://kiro-ide-website --index-document index.html --error-document index.html
```

## Troubleshooting Bucket Policy Errors

If you get errors applying the bucket policy:

1. **Access Denied**: Ensure your AWS user has `s3:PutBucketPolicy` permission
2. **Invalid Policy**: Double-check bucket name in ARN matches exactly
3. **Block Public Access**: Make sure Step 2 was completed first

**Alternative - Use AWS Console**:

1. Go to S3 Console → Your bucket → Permissions tab
2. Edit "Block public access" and uncheck all boxes
3. Add bucket policy in "Bucket policy" section

## Step 5: Upload Website Files

```bash
# Upload all files to S3
aws s3 sync . s3://kiro-ide-website --exclude "*.md" --exclude "bucket-policy.json" --exclude ".git/*"

# Set correct content types
aws s3 cp s3://kiro-ide-website/index.html s3://kiro-ide-website/index.html --content-type "text/html" --metadata-directive REPLACE
aws s3 cp s3://kiro-ide-website/styles.css s3://kiro-ide-website/styles.css --content-type "text/css" --metadata-directive REPLACE
```

## Step 6: Get Website URL

Your website will be available at:

```plaintext
http://kiro-ide-website.s3-website-ap-southeast-2.amazonaws.com
```

## Optional: Custom Domain with CloudFront

For better performance and custom domain support, consider setting up CloudFront:

1. Create a CloudFront distribution
2. Set the S3 bucket as the origin
3. Configure your custom domain
4. Set up SSL certificate via AWS Certificate Manager

## Updating the Website

To update your website, simply run the sync command again:

```bash
aws s3 sync . s3://kiro-ide-website --exclude "*.md" --exclude "bucket-policy.json" --exclude ".git/*" --delete
```

The `--delete` flag removes files from S3 that are no longer in your local directory.
