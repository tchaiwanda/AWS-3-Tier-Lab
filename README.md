# AWS 3-Tier Architecture Example

This project demonstrates a simple **2-instance 3-tier architecture** on AWS using a **public NAT instance** and a **private instance** in separate subnets. The setup is designed for learning purposes and documentation.


## Disclaimer

This project is intended **for educational purposes only**. Do not use real IP addresses, security credentials, or key pairs in public repositories. Always follow AWS best practices for security and access control.

---

- **NAT instance**: Public, acts as jump host and outbound gateway for the private instance.
- **Private instance**: Private, no public IP, accessed only through the NAT instance.
- **Key pair**: Shared between both instances (`"<your-key-pair>.pem>"`).

---

## VPC and Subnets

| Component         | CIDR          | Notes                                |
|------------------|---------------|--------------------------------------|
| VPC              | 10.0.0.0/16   | Single VPC for the project           |
| Public Subnet    | 10.0.1.0/24   | NAT instance, auto-assign public IP  |
| Private Subnet   | 10.0.2.0/24   | Private instance, no public IP       |
| Internet Gateway | n/a           | Provides internet access to public subnet |
| Route Tables     | Public → IGW, Private → NAT | Ensures proper routing for connectivity |

---

## Security Groups

### NAT Security Group

- **Inbound Rules**:
  - SSH (Port 22) from your Windows 11 public IP
- **Outbound Rules**:
  - All traffic (default)

### Private Security Group

- **Inbound Rules**:
  - SSH (Port 22) from NAT instance private IP or NAT SG
- **Outbound Rules**:
  - All traffic (default)

---

## Key Pair

- **File Name**: `-<your-key-pair>.pem`
- Used for both NAT and private instances
- Stored locally on Windows and copied to NAT for private SSH access

**Copy key to NAT instance:**

```powershell
scp -i "C:\Users\<YourUser>\Downloads\<your-key-pair>.pem.pem" "C:\Users\<YourUser>\Downloads\<your-key-pair>.pem" ec2-user@<NAT_Public_IP>:/home/ec2-user/
```
**SSH Workflow**

Step 1: Windows → NAT Instance
 ``` ssh -i "C:\Users\<YourUser>\Downloads\<your-key-pair>.pem" ec2-user@<NAT_Public_IP> ```

Step 2: Set Key Permissions on NAT:

``` chmod 400 "<your-key-pair>.pem>" ```

Step 3: NAT → Private Instance
``` ssh -i ./<your-key-pair>.pem> ec2-user@10.0.2.x ```

## Notes

The private instance cannot be accessed directly from the internet; all access goes through the NAT instance.

Ping may fail by default due to security groups blocking ICMP — this does not indicate a connectivity issue.

This setup simulates a simplified 3-tier architecture for learning and documentation purposes.

## Instances 📸

![Instances](screenshots/instances.png)

## Security Groups 📸

![Security-Groups](screenshots/sec-group.png)

## Pings 📸

![Pings](screenshots/sec-group.png)

## Successful Connection 📸

![Success](screenshots/success.png)

## ERROR 📸

![Error](screenshots/error.png)

## Permission Denied 📸 

![Permission Denied](screenshots/permission-denied.png)

## Challenges Faced

- Configuring the NAT and private instance correctly with proper subnets and route tables.
- Ensuring SSH access worked through the NAT instance without exposing private instance publicly.
- Managing key pair files on Windows and setting correct permissions for secure SSH.
- Troubleshooting connectivity issues due to incorrect security group rules or IP addresses.

## Lessons Learned

- Understanding the difference between public and private subnets, and why private instances cannot be accessed directly from the internet.
- Importance of using a NAT instance or NAT gateway for outbound internet access from private instances.
- Properly structuring AWS resources for a simple 3-tier architecture.
- Using placeholders for sensitive data when documenting cloud projects publicly.


## Author
**Tchai Chambers**  
[LinkedIn](https://linkedin.com/in/tchaiwanda) | [GitHub](https://github.com/tchaiwanda)


## 🏷️ Tags

`#AWS` `#EC2` `#CloudComputing` `#Networking` `#VPC` `#NAT` `#Subnet` `#SSH` `#Linux` `#Windows` `#3TierArchitecture` `#AWSCloud` `#CloudSecurity` `#AWSNetworking` `#InfrastructureAsCode` `#AWSBeginner` `#LearningProject` `#CloudProject`
