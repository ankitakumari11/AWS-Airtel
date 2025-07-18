## S3

1. Create S3 bucket
2. Host the website on s3 bucket using index.html and then remove public access.
3. So now u will not be able to access the website as public access is disable so we will access it using cloud front.


## CLOUD FRONT:  
1. Go to cloud front -> security -> origin access
2. create OAC with S3 access and "Always" signing  
<img width="1920" height="958" alt="image" src="https://github.com/user-attachments/assets/55c7c90f-450d-44a7-8421-1a7c035b38f9" />


3. Create cloud fornt
<img width="1901" height="916" alt="image" src="https://github.com/user-attachments/assets/5dde9a51-d422-4960-bb0b-c49abe2a13a6" />

<img width="1920" height="914" alt="image" src="https://github.com/user-attachments/assets/adb41c7b-2c1c-49ae-9fc3-d7b67bc01103" />

<img width="1914" height="889" alt="image" src="https://github.com/user-attachments/assets/1a36652b-423f-4bbe-9417-f0abcdff011d" />
  

<img width="1903" height="844" alt="image" src="https://github.com/user-attachments/assets/e906ebd3-2492-4d88-acfc-77f8022ba635" />
  
<img width="1920" height="855" alt="image" src="https://github.com/user-attachments/assets/2a4d6a34-ba13-492e-a45a-1c74e6f4192c" />
  
<img width="1920" height="924" alt="image" src="https://github.com/user-attachments/assets/a2481c89-0083-43b6-9b3a-b26c92024514" />

<img width="1920" height="942" alt="image" src="https://github.com/user-attachments/assets/c9413992-db9c-4559-ab4c-b2fa8fe897a8" />

<img width="1920" height="411" alt="image" src="https://github.com/user-attachments/assets/8cc7c014-dbf3-42eb-abd1-7aa750b5c31c" />


Now go to your cloudfront distribution:

  <img width="1911" height="920" alt="image" src="https://github.com/user-attachments/assets/716c29aa-6ee6-4fed-9b4f-9fcbca265949" />

Go to default root object:

<img width="1920" height="916" alt="image" src="https://github.com/user-attachments/assets/4322a460-2caf-4e28-b1ce-78be61915b58" />

Go down and write index.html

<img width="1920" height="1020" alt="image" src="https://github.com/user-attachments/assets/9dbfc36e-5c0f-4fc3-8eb9-2f1f3cae9dce" />

Now copy the distribution domain name and paste in browser:

<img width="1920" height="737" alt="image" src="https://github.com/user-attachments/assets/b111e395-e74f-4928-abb0-a2c985949bb2" />

Now you will be able to see the site:

<img width="1920" height="202" alt="image" src="https://github.com/user-attachments/assets/139fc74b-3a7f-42f6-912a-7cb59a54355d" />






