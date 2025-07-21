## When a file is uploaded to an S3 bucket, the Lambda function will run and print a log (e.g., file name and size).

✅ Prerequisites:  
- An AWS account
- Permissions to access S3, Lambda, and IAM

🛠️ Step-by-Step Setup  
✅ Step 1: Create an S3 Bucket  
- Go to S3 in the AWS Console.
- Click Create bucket.
- Name it something like my-lambda-trigger-bucket (name must be globally unique).
- Leave other options default and click Create bucket.

✅ Step 2: Create a Lambda Function  
- Go to Lambda in the AWS Console.
- Click Create function.
- Choose:Author from scratch
- Function name: s3-upload-logger
- Runtime: Python 3.12 (or Node.js if you prefer)
- Click Create function.

✅ Step 3: Add Code to Lambda
- Paste this code into the code editor section of the Lambda:
```
import json

def lambda_handler(event, context):
    # Get bucket and object info from the event
    bucket = event['Records'][0]['s3']['bucket']['name']
    key = event['Records'][0]['s3']['object']['key']
    size = event['Records'][0]['s3']['object']['size']
    
    print(f"New file uploaded to bucket: {bucket}")
    print(f"File name: {key}")
    print(f"File size: {size} bytes")
    
    return {
        'statusCode': 200,
        'body': json.dumps('File processed!')
    }
```
- Click Deploy to save the code.

✅ Step 4: Create S3 Trigger
- Scroll down to Function overview → Click + Add trigger
- Choose S3
- Configure:
  - Bucket: Select the bucket you created
  - Event type: PUT (when a new file is uploaded)
  - Check ✅ "Acknowledge that existing permissions will be modified"
- Click Add

✅ Step 5: Set IAM Permissions
- If AWS asks you to create or update a role with permissions — allow it.
- Ensure your Lambda role has this policy:
```
{
  "Effect": "Allow",
  "Action": "s3:GetObject",
  "Resource": "arn:aws:s3:::my-lambda-trigger-bucket/*"
}
```

✅ Step 6: Test It!
- Go to S3 bucket.
- Upload any small file (like test.txt).
- Wait a few seconds.

✅ Step 7: View Logs
- Go to CloudWatch → Logs → /aws/lambda/s3-upload-logger
- Click on the latest log stream.
- You’ll see:
```
New file uploaded to bucket: my-lambda-trigger-bucket
File name: test.txt
File size: 1024 bytes
```

**🎉 Done! You just built your first event-driven Lambda function!**

