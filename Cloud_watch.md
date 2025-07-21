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
   - Go to CloudWatch → Alarms
     - Click Create Alarm.
   - Select Metric
     - Choose Select Metric.
     - Browse to:EC2 → Per-Instance Metrics → Choose CPUUtilization for your instance.
     - Click Select metric.
   - Set Conditions
     - Threshold type: Static
     - Whenever CPUUtilization is: Greater than 60
     - For...: 5 consecutive minutes
        
       <img width="1920" height="909" alt="image" src="https://github.com/user-attachments/assets/8eb447d7-cd14-4fb4-9d79-20bd980e0fe5" />

       <img width="1913" height="501" alt="image" src="https://github.com/user-attachments/assets/0eb82e7b-c3d0-4659-9532-ac0c2fe2a9a4" />


   - Add Notification (SNS)
     - If you have an SNS topic already:
     - Select the topic.
     - If not: Click Create new topic.
     - Give it a name (e.g., cpu-alert-topic)
     - Add your email (e.g., you@example.com)
     - Confirm the subscription via the email you receive.

       <img width="1920" height="913" alt="image" src="https://github.com/user-attachments/assets/ce021ed5-cde0-4fad-a4f1-828f295f4c28" />
          
    - Finalize
      - Give the alarm a name (e.g., HighCPUAlarm)
      - Click Create Alarm

        <img width="1905" height="889" alt="image" src="https://github.com/user-attachments/assets/eda9dfd1-15b4-441b-bf26-a914169decde" />

        <img width="1907" height="301" alt="image" src="https://github.com/user-attachments/assets/6022dd18-19ba-43f0-9b7e-df35c94ff1fd" />


7. **CREATE DASHBOARDS**
   - Go to CloudWatch → Dashboards
     - Click Create Dashboard
     - Give it a name (e.g., EC2-Monitoring)
   - Add Widgets
     - Click Add widget → Choose: Line or Stacked Area → Add metric
     - Select:EC2 → Per-Instance Metrics
     - Add CPUUtilization, NetworkIn/Out, DiskReadBytes/WriteBytes

       <img width="1432" height="769" alt="image" src="https://github.com/user-attachments/assets/b643ced9-f887-448e-ae0b-6fdec51fae37" />

       <img width="1806" height="1004" alt="image" src="https://github.com/user-attachments/assets/b3778761-ad1e-40fe-ae66-5ffd2292caa2" />
         
   - Arrange & Save
     - Drag to rearrange widgets
     - Click Save dashboard

        <img width="1920" height="725" alt="image" src="https://github.com/user-attachments/assets/7eea89c0-3984-450c-a1da-838acb967274" />
      

8. **SEND LOGS TO CLOUDWATCH** (Optional)
   - Install awslogs on ec2
   - sudo yum install awslogs
   - Configure /etc/awslogs/awslogs.conf
     ```
     sudo vi /etc/awslogs/awslogs.conf
     ---
      [general]
      state_file = /var/lib/awslogs/agent-state
      
      [/var/log/messages]
      file = /var/log/messages
      log_group_name = ec2-system-logs
      log_stream_name = {instance_id}/messages
      datetime_format = %b %d %H:%M:%S
     ```
   - Update /etc/awslogs/awscli.conf with your region:
   ```
    [plugins]
    cwlogs = cwlogs
    [default]
    region = ap-south-1  # Change to your region
   ```
   - Start and enable awslogsd service
   ```
   sudo systemctl start awslogsd
   sudo systemctl enable awslogsd
   sudo systemctl status awslogsd
   ```

