All Amazon S3 buckets must have encryption enabled, enforce SSL, and block public access.
Amazon DynamoDB tables, streams, tables must have encryption enabled. 
All Amazon SNS topics must have encryption enabled and enforce SSL. 
All Amazon SNS queues must enforce SSL.

### Rules specific to my workspace

No public facing API endpoints, load balancers.  Secure them using WAF.

Check for the presense of .env file with variable - IP_ALLOWLIST which take comma separated CIDR ranges.  If present, lock down access to those CIDR ranges only.  If it's not present, the application can be open to the public. 

When you try to do a task, check if there's existing scripts available at ./infrastructure/scripts/ folder. 
