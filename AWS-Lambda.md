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
- Choose:
  - Author from scratch
  - Function name: s3-upload-logger
  - Runtime: Python 3.12 (or Node.js if you prefer)
- Click Create function.

<img width="1920" height="862" alt="image" src="https://github.com/user-attachments/assets/94edfe78-6ef0-49d5-b434-a88b1474007a" />

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

<img width="1920" height="900" alt="image" src="https://github.com/user-attachments/assets/f84f7183-03c7-415b-9efa-7c365fc48db9" />

<img width="1894" height="328" alt="image" src="https://github.com/user-attachments/assets/54604c99-2f3d-4b7b-bbad-0f2297b7f40c" />



✅ Step 4: Create S3 Trigger
- Scroll up to Function overview → Click + Add trigger
  
<img width="1913" height="690" alt="image" src="https://github.com/user-attachments/assets/9fcc4b92-3fd8-4726-95ad-03a06ca5dc7e" />
  
- Choose S3
- Configure:
  - Bucket: Select the bucket you created
  - Event type: PUT (when a new file is uploaded)
  - Check ✅ "Acknowledge that existing permissions will be modified"
- Click Add
  
<img width="1908" height="910" alt="image" src="https://github.com/user-attachments/assets/142ec575-60a8-493d-b28e-41c321588316" />
  
<img width="1920" height="723" alt="image" src="https://github.com/user-attachments/assets/973b8c26-8396-4bd0-9d00-e1b55e695511" />
  
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

<img width="1913" height="936" alt="image" src="https://github.com/user-attachments/assets/5408c449-54a0-46f9-8309-ca7ace2a753d" />

*Currently, your Lambda function's IAM role has only permissions for CloudWatch Logs — which means it can write logs but cannot access your S3 bucket yet.*  
From your Lambda function page:  
- Scroll to Configuration → Permissions
- Click on the Role name under Execution role
- On the IAM Role page → Click Add permissions → Create inline policy
- Choose the JSON tab
- Paste the following JSON (replace your actual bucket name if needed):
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-lambda-trigger-bucket/*"
    }
  ]
}
```

<img width="1916" height="740" alt="image" src="https://github.com/user-attachments/assets/42b5978b-8df1-4cd9-ad2e-00006d55afdf" />

- Click Next: Tags → (optional: skip tags)
- Click Next: Review
- Name the policy something like LambdaS3ReadAccess
- Click Create policy

<img width="1919" height="891" alt="image" src="https://github.com/user-attachments/assets/d57407dd-e02b-4c0d-abb2-fe957b2e3ee2" />

<img width="1920" height="703" alt="image" src="https://github.com/user-attachments/assets/db76cbcc-d4c9-478c-b6c4-e806c707d381" />


✅ Step 6: Test It!
- Go to S3 bucket.
- Upload any small file (like test.txt).
- Wait a few seconds.

<img width="1920" height="317" alt="image" src="https://github.com/user-attachments/assets/c43676da-7314-44ea-b129-af47c518ea49" />
  
✅ Step 7: View Logs
- Go to CloudWatch → Logs → /aws/lambda/s3-upload-logger
- Click on the latest log stream.
- You’ll see:
```
New file uploaded to bucket: my-lambda-trigger-bucket
File name: test.txt
File size: 1024 bytes
```

<img width="1915" height="920" alt="image" src="https://github.com/user-attachments/assets/43cbca64-be39-42d4-a1b7-4f4e8fe907aa" />


**🎉 Done! You just built your first event-driven Lambda function!**

--------------------------------------------------------------------------------------------------------------------------------------------------------------------
  
### ✅ Why You See Output in CloudWatch (Even Without Mentioning It in lambda code)  
🔹 1. Automatic Logging  
- Every AWS Lambda function automatically sends:
  - print() output
  - Exceptions
  - Errors  
→ to Amazon CloudWatch Logs, without any additional setup.

🔹 2. IAM Role with Logging Permissions  
- When you create a Lambda function, AWS creates or attaches an IAM execution role that includes permissions like:
```
{
  "Effect": "Allow",
  "Action": [
    "logs:CreateLogGroup",
    "logs:CreateLogStream",
    "logs:PutLogEvents"
  ],
  "Resource": "*"
}
```
- These permissions allow Lambda to:
  - Create a log group /aws/lambda/<function-name>
  - Write logs from print() or exceptions

🔹 3. Behind the Scenes
- When you do: `print("Something happened")`
- Lambda sends this to: `CloudWatch → Log Group: /aws/lambda/s3-upload-logger → Log Stream: <timestamp>`
