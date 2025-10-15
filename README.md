# Static Website Hosted on AWS

### Deployment Steps

1. **Create S3 Bucket**
   ```bash
   aws s3 mb s3://your-bucket-name
   ```
2. **create index.htlm file with required content**


3. **Upload the HTML file to s3**
   ```bash
   aws s3 cp index.html s3://your-bucket-name/
   ```

4. **Enable static website hosting on s3**
   ```bash
   aws s3 website s3://your-bucket-name/ --index-document index.html
   ```

4. **Disable the block public access**
    ```bash
   aws s3api put-public-access-block \
  --bucket your-bucket-name \
  --public-access-block-configuration '{
    "BlockPublicAcls": false,
    "IgnorePublicAcls": false,
    "BlockPublicPolicy": false,
    "RestrictPublicBuckets": false
  }'
   ```

6. **Set Bucket Policy for Public Access**
  Create a file `bucket-policy.json`:
   ```json
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Sid": "PublicReadGetObject",
         "Effect": "Allow",
         "Principal": "*",
         "Action": "s3:GetObject",
         "Resource": "arn:aws:s3:::your-unique-bucket-name/*"
       }
     ]
   }
   ```
   
   Apply the policy:
   ```bash
   aws s3api put-bucket-policy --bucket your-bucket-name --policy file://bucket-policy.json
   ```

7. **Accessing the website**
   - Direct S3 URL: `http://your-bucket-name.s3-website-{region}.amazonaws.com`


**What else you would do with your website, and how you would go about doing it if you had more time**

### 1. Use Infrastructure as Code to deploy the aws infrastructure.

### 2. Add Cloudfront with SSL certificate and enable caching

### 3. Register domain via route53 and use custom domain name

**Alternative solutions that you could have taken but didn’t and explain why.**

### 1. EC2 + Nginx
- Not sutiable for static content
- Higher cost
- Need to manager the EC2 server for security patching etc.

### 2.ECS/EKS
- Not needed for static HTML, high complexity

***What would be required to make this a production grade website that would be developed by various development teams. The more detail, the better!***

### 1. Infrastructure as Code
- All the resources will be created via IAC

### 2. CI/CD pipelines
- The IAC code will be stored in Git repository, Will help in version control
- Implement code review, build, test and deploy stages
- branch protection rules
- PR based deployments

### 3. Environment based deployments
- Separate AWS accounts for dev/stage/prod

### 4. Use custom domain
- use Route 53 for DNS and ACM for certificate

### 5. Add cloudfront and attach WAF to it.
- implement caching via cloudfront and WAF for DDOS protection etc.
- use HTTPS only



