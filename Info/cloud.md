# Cloud Security

## Securing an S3 Bucket

### Encryption
Enforce encryption at the bucket level. There is no downside of not encrypting your objects.

### Bucket Policies
Bucket policies take precedence over other IAM policies. Set these first and keep them restrictive.

### IAM Policies
Don't put `s3:*` in your policies. Restrict access to buckets by creating policies that allow access based on demonstrated need.

### Block Public Access
AWS has a new feature to block all public access as well as ACLs. Turn that on

### Monitoring
* Set up AWS Config to check your policies are in place and alert on them.
* Set up server access logging and CloudTrail to log S3 events to important buckets.

