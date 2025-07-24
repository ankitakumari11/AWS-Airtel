
<img width="1920" height="968" alt="image" src="https://github.com/user-attachments/assets/e5395b08-03e3-43a0-a34d-c5ef04d0fa8d" />

<img width="1920" height="918" alt="image" src="https://github.com/user-attachments/assets/d938a37c-c748-4799-b667-ca8c4c541127" />

<img width="1918" height="918" alt="image" src="https://github.com/user-attachments/assets/d47a9450-8b89-4121-ab86-b772adb8582a" />

<img width="1911" height="963" alt="image" src="https://github.com/user-attachments/assets/0c300ed4-6ef9-49ec-85e4-3b79e08a89eb" />

<img width="1920" height="712" alt="image" src="https://github.com/user-attachments/assets/62d4567e-2aa8-46d3-adce-0ff8459fc9f1" />

<img width="1920" height="971" alt="image" src="https://github.com/user-attachments/assets/7fe59319-0a6f-4c39-8ed3-3f2dba4fd36c" />

<img width="1906" height="677" alt="image" src="https://github.com/user-attachments/assets/c315b10f-b766-474c-9af0-172f5318c2ff" />
  
select connect to ec2 instance
<img width="1920" height="976" alt="image" src="https://github.com/user-attachments/assets/2727644e-50ea-4fb9-825d-c8aed35a50be" />

now create an ec2 first: (vpc should be same for instance and rds both)

<img width="1919" height="975" alt="image" src="https://github.com/user-attachments/assets/31f80b64-d195-4b49-b36f-4ff9c48d77dc" />

Now select your created vm here:

<img width="1914" height="1031" alt="image" src="https://github.com/user-attachments/assets/f86c46e6-a17f-4a5f-a77a-de9bc2c6b02d" />

<img width="1920" height="985" alt="image" src="https://github.com/user-attachments/assets/bd008447-d707-4cf7-8c72-8393efeb1208" />

<img width="1920" height="914" alt="image" src="https://github.com/user-attachments/assets/e45046cf-3b75-4e77-89fc-a02fa1e9c9f5" />

<img width="1920" height="920" alt="image" src="https://github.com/user-attachments/assets/d4b08884-e671-4d33-9efa-2986886a3b12" />

<img width="1892" height="919" alt="image" src="https://github.com/user-attachments/assets/8aba9fbe-b618-434e-bb34-4158130b6c65" />

<img width="1915" height="959" alt="image" src="https://github.com/user-attachments/assets/043d54ad-214d-40bd-96dd-74dcbd1bfe46" />

<img width="1920" height="965" alt="image" src="https://github.com/user-attachments/assets/e4f2592f-2444-4606-ad95-3bba4db13ccf" />

Now below: everyday it will take automatic backups at 11:30 utc:  

<img width="1920" height="927" alt="image" src="https://github.com/user-attachments/assets/f5ec35ee-e47c-426e-b4ee-5d7507baffaf" />

<img width="1920" height="882" alt="image" src="https://github.com/user-attachments/assets/095fcd07-2355-40c3-9d63-795947fda223" />

Here maintenancy window and backup window shouldn't be same becoz maintainence will upgrade and maybe it will restart the rd and will interuupt backup:

<img width="1920" height="864" alt="image" src="https://github.com/user-attachments/assets/df4c86f5-06c1-46b8-9829-b9d081eff9f7" />

You can enable this: but here in the lab we are not:

<img width="1919" height="874" alt="image" src="https://github.com/user-attachments/assets/491e1d4a-2d10-4315-945b-fd4cca1dbb11" />

Now create database.

<img width="1920" height="615" alt="image" src="https://github.com/user-attachments/assets/79a45692-9eec-4729-81aa-9b31d7be0b3e" />

### Now we will connect to the database using ec2 instance

1. Connect to ec2
2. switch to root user
3. Install DB client (mysql) on the server (as our RDS is mysql)
```
sudo apt update
sudo apt install mysql-server -y
sudo systemctl start mysql
sudo systemctl enable mysql
sudo systemctl status mysql
```
4. Go to your RDS and checkendpoint

     <img width="1920" height="782" alt="image" src="https://github.com/user-attachments/assets/6fc41dd4-31e1-4f29-bfbf-cab293004576" />

5. Add inbound rule in the server

     <img width="1913" height="787" alt="image" src="https://github.com/user-attachments/assets/f260c1a9-82c2-49f3-a8bc-e6df9fb24643" />

6. run below command on your server:
```
mysql -h <endpoint> -P 3306 -u admin -p {here write your endpoint}
mysql -h database-1.cczawg2yku4m.us-east-1.rds.amazonaws.com -P 3306 -u admin -p
```
