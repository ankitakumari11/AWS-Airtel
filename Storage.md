# S3- BUCKET

**Versioning:** 
1. Go to your bucket -> Properties ->edit bucket versioning -> enable.
2. Now make changes locally in your laptop and in the file which u uploaded earlier and upload again.
3. Now inside your bucket (check mark : "show versions") and now u will see multiple versions of your file.  

## LIFE-CYCLE RULE:

1. go to bucket->your bucket->management->lifecyle rules->create lifecycle rule->rule name (ankita-lifecycle) -> choose a rule scope ->apply to all objects in the bucket ->give prefix (Anything)
2. inside lifecyle rule actions->check [ Transition current versions of objects between storage classes], similairly add other 
```
note : minimum of 30 days required for Standard IA.
One Zone-IA: standard +30
Galcier flexible retreival : one zone +30
glacier deep archive: Galcier flexible retreival + 90

Day 0
Objects uploaded
Day 30
Objects move to Standard-IA
Day 60
Objects move to One Zone-IA
Day 90
Objects move to Glacier Flexible Retrieval (formerly Glacier)
Day 180
Objects move to Glacier Deep Archive
```


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
4. Go to permissions ->(Block public access (bucket settings)) ->off ->bucket policy->add below policy  -> (here in the below policy replace this "ankita-airtel2" with your bucket name.  
```
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

- Aws ebs = elastic block storage
- high performance block storage for ec2
- persistent , scalable and reliable
- supports snapshots and encryption
- *It is a block storage service used with amazon ec2 instances. EBS volumes are persistent and continue to exist independently even after an instance is stopped or terminated.*

**EBS VOLUME TYPES**
- gp3 (General purpose SSD) : balanced performances and cost, it is advance version of gp2.
- Io2 Block express : High -Performance for IOP-intensive workloads.
- stl (Throughput HDD) : Big data , log processing.
- scl (Cold HDD) : Infrequent access , lowest cost.
- *EBS offers multiple volume types based on performance and cost. GP3 is a general-pirpose ssd used in most wrokloads. Io2 is ideal for high performance applications like databased. St1 and sc1 are HDD-based and suited for large volumes of infrequenctly accessed data.*

**Features of EBS**:

- Durability : For web apps , backups , and big data.
- Snapshots : For high perfromance storage attached to EC2.
- Encryption : For shared access and file-based workloads.
- Attach/detach : For long-term , low-cost archiving.
- Resize : For backup automation and compliance
- *EBS volumes are highly durable and can be spanshotted for backup and disaster recovery. Encryption is available at rest and in transit. You can also change volume configuration on the fly with minimal or no downtime.*

**Common use**
- Running databases (mysql , PostgresSQL , Oracle)
- hosting containers and applications
- storing logs , cache and swap files.
- high - speed transactional workloads.
- *EBS is used in various workloads that need low-latency, consistent performance. It's perfect for databases,container storage, and file systems. Since it supports high IOPS , it suits transactional apps and real-time processing.*

**Summary and best practice**
- Choose the right volume type based on workload
- use snapshots regularly for backups
- emable encryption for secuirty
- monitor using cloudwatch
- consider i02 for mission-critical apps
- *To summarize, EBS is a powerful storage solution for EC2 , select the right volume type for performance and cost balance. Don't forget to setup snapshots and monitor performance using CloudWatch. For critical workloads , use io2 volumes with high durability and performance.*

> [!NOTE]
> - ebs volume and instance should be in same AZ.
> - *To summarize, ebs is a powerful storage solution for ec2. Select the right volume type for performance and cost baalance. Dont forget to setup snapshots and monitor performance using cloudwatch.For critical workloads , use io2 volumes with high durability and performance.*
> - - *ssd has more speed and more capacity as compare to hdd*

### HANDS-ON EBS

**CREATING VOLUME AND MOUNTING**

1. Create an instance and see the Avalaibility zone of the server
2. Go to volumes and Create volume. Here select the same zone in which the ec2 is there and keep everything default i.e gp3.
           <img width="1897" height="966" alt="image" src="https://github.com/user-attachments/assets/a2770630-6094-459c-a1a5-aa9bbe3a900f" />
3. Here u will see that maybe some volumes present already , those are create by default whenevr we create a new instance so it is attached to that instance.
4. Volume which u created:
           <img width="1915" height="848" alt="image" src="https://github.com/user-attachments/assets/f710c6ad-94fa-4684-a631-294cf127ac5e" />
5. Go to actions now on the volume -> attach
           <img width="1902" height="949" alt="image" src="https://github.com/user-attachments/assets/d94c47b3-41e4-45d7-a10b-182b89743326" />

   *here /dev/sdb is*
6. Now u can see on the server , a new volume is attached :
           <img width="1886" height="314" alt="image" src="https://github.com/user-attachments/assets/697d728a-6d00-4f9f-a621-87ae709122ee" />
7. Mounting the volume
- `sudo fdisk -l` : The sudo fdisk -l command is used on Linux systems to list all available disk partitions and details about each connected storage device.  
            <img width="1920" height="720" alt="image" src="https://github.com/user-attachments/assets/380f5f98-11f3-421e-8593-70375a595c75" />

  <img width="1070" height="587" alt="image" src="https://github.com/user-attachments/assets/3cdba0c5-16a1-4f17-9e9c-f7757de0b994" />

- `file -s /dev/xvdg`: It is used to identify the type of data or filesystem on the block device /dev/xvdg.
            <img width="1917" height="91" alt="image" src="https://github.com/user-attachments/assets/8895341a-5aee-43e1-9ef0-47553a943981" />
              
- `mkfs -t xfs /dev/xvdg` : is used to format the disk /dev/xvdg with the XFS filesystem.  
            <img width="1917" height="309" alt="image" src="https://github.com/user-attachments/assets/84550784-3c9a-4164-9301-afe245ba5af4" />
              
- `file -s /dev/xvdg` : This indicates that /dev/xvdg now has a valid XFS filesystem, and it's ready to be mounted.This tells Linux to inspect the contents of the block device /dev/xvdg and identify what kind of data or filesystem is on it.
            <img width="1920" height="82" alt="image" src="https://github.com/user-attachments/assets/bc2d16b1-c849-4912-9779-9abee12a23d7" />
              
- `mkdir /myebsvol`
- `mount /dev/xvdg /myebsvol`
            <img width="1920" height="369" alt="image" src="https://github.com/user-attachments/assets/31896909-ee86-43e3-aca9-5d3d0144802e" />
              
- `ls -lart /myebsvol/` : is used to list the files and directories inside the /myebsvol/ directory in long format, including hidden files, sorted by modification time, with the newest files at the bottom.  
- `df -h`
            <img width="1920" height="406" alt="image" src="https://github.com/user-attachments/assets/8f30ff27-4902-4c5a-94c0-3077c5056f21" />


**RESIZING EBS VOLUME**

- You can't reduce the volume , you can delete it directly.
- Go to your ebs volume -> modify -> increase size from 100 to 105.
        <img width="1551" height="318" alt="image" src="https://github.com/user-attachments/assets/e7bf7d32-ddfe-40d4-a0e2-2a8c46093e23" />


**TAKING SNAPSHOTS OF EBS**  
  
- Go to your volume -> click your volume -> actions -> create snapshot
          <img width="1920" height="888" alt="image" src="https://github.com/user-attachments/assets/f1d07f14-1140-47b6-9b4a-a38a4198847b" />

- Go to snapshots and u can see the snapshot:
          <img width="1562" height="268" alt="image" src="https://github.com/user-attachments/assets/eea858a6-dc09-4089-8cdc-5f6317157030" />


**CREATING EBS VOLUME FROM THAT SNAPSHOT**

- Go to snapshots -> select your snapshot -> actions -> create volume from snapshot
         <img width="1913" height="872" alt="image" src="https://github.com/user-attachments/assets/f2814139-86d2-4b3a-9c29-e9137736308d" />

- Volume from snapshot is created now:
         <img width="1556" height="301" alt="image" src="https://github.com/user-attachments/assets/2645ebb6-ab87-4592-9e21-22ee4050cb49" />












  
  




