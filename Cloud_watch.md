# Cloud Watch

1. Create ec2 instance (Amazon linux)
2. Enable monitoring and send logs (login to ec2 instance)
```
sudo yum update -y
sudo yum install httpd -y
sudo systemctl start httpd
echo "Welcome to ec2" > /var/www/html/index.html
```

4. Go to ec2 instance  -> monitoring -> Managed detailed monitoring ->enable
                <img width="1920" height="736" alt="image" src="https://github.com/user-attachments/assets/76663338-ee63-466a-a4be-539a583b6fad" />

  *After you enable detailed monitoring, the Amazon EC2 console displays monitoring graphs with a 1-minute period for the instance.*

5. Go to cloudwtach on aws -> metric ->all metrics -> browse -> ec2 -> per instance metrics -> select instance
                          <img width="1918" height="941" alt="image" src="https://github.com/user-attachments/assets/0ff95d48-edf5-4bc3-b73d-1294547a4969" />

6. **CLOUD WATCH ALARMS**
   - Cloudwatch -> Alarms -> create Alarm
   - Select ec2 -> CPU Utilisation
   - Set threshold : > 60% for 5 mins
   - Add SNS notification for alerts

7. **CREATE DASHBOARDS**
   - Cloudwatch -> dashboards ->create
   - Add graphs for CPU , Network , Disk
   - Arrange widgets and save

8. **SEND LOGS TO CLOUDWATCH** (Optional)
   - Install awslogs on ec2
   - sudo yum install awslogs
   - Configure /etc/awslogs/awslogs.conf
   - Start and enable awslogsd service

