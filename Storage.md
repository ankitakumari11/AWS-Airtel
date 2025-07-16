## BUCKET -POLICY:

GO TO bucket ->your bucket -> permissions ->bucket policy -> edit bucket policy -> add:  
```
{
    "Version": "2012-10-17",
    "Id": "DenyPublicAccess",
    "Statement": [
        {
            "Sid": "DenyPublicRead",
            "Effect": "Deny",
            "Principal": "*",
            "Action": [
                "s3:GetObject",
                "s3:GetObjectVersion"
            ],
            "Resource": "arn:aws:s3:::ankita-airtel/*",
            "Condition": {
                "Bool": {
                    "aws:SecureTransport": "false"
                }
            }
        },
        {
            "Sid": "DenyPublicAccessIfNotFromSpecificAWSAccount",
            "Effect": "Deny",
            "Principal": "*",
            "Action": "s3:*",
            "Resource": [
                "arn:aws:s3:::ankita-airtel",
                "arn:aws:s3:::ankita-airtel/*"
            ],
            "Condition": {
                "StringNotEquals": {
                    "aws:PrincipalAccount": "064197589772"
                }
            }
        }
    ]
}
```

*explanation of above policy:  To deny all public access to an AWS S3 bucket, you can use a bucket policy that explicitly denies access if the request is not from your AWS account or does not use certain conditions (like specific VPCs or IPs).*  


## ENABLE STATIC WEBSITE HOSTING:  

1. Again create a bucket : "ankita-airtel2"  
2. go to your bucket -> properties - > go to end ->edit static website hosting -> enable -> (hosting type)->host a static website ->(index document) ->index.html.  
3. Go to your bucket , upload index.html  
4. Go to permissions ->(Block public access (bucket settings)) ->off ->bucket policy->add below policy  
```
bucket policy:
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadForStaticWebsite",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::ankita-airtel2/*"
        }
    ]
}
```

5.Go to properties and go to webiste line , you will be able to see the website and u can share the link too with anyone as u have allowed public access.  


# EBS (ELASTIC BLOCK STORAGE)  


