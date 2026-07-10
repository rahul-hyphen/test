# AWS S3 Static Website Hosting using CloudFront, ACM & Route 53

Deploy a secure static website on AWS using:

- Amazon S3
- Amazon CloudFront
- AWS Certificate Manager (ACM)
- Amazon Route 53

## Architecture

```

Browser
│
▼
Route53
│
▼
CloudFront
│
(OAC)
│
▼
Amazon S3 Bucket
