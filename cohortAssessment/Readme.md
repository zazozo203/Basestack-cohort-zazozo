Student: Akande Mayokun
Training account / team:
AWS Region: Europe Stockholm
Date: 26/10/2026


1. What I built
I built an E-learning web app using EC2 instance, VPC,subnets,security groups, route tables, s3 buckets e.t.c

<img width="958" height="469" alt="image" src="https://github.com/user-attachments/assets/94d0f60f-e367-4cb9-96e2-81fb171e6a90" />

2. Architecture
<img width="1200" height="720" alt="architecture" src="https://github.com/user-attachments/assets/3e9045d5-8e85-44bc-b838-76e2a195aed9" />

A visitor types the instance's public IPv4 address into a browser. The request travels over the internet to the Internet Gateway, which is attached to my VPC. The route table sends the traffic into the public subnet, where the security group checks it and allows it because it is HTTP on port 80. The web server on the EC2 instance then returns the BrightPath Tutors page the same way.


3. Security decisions
• How is HTTP traffic allowed? The security group brightpath-web-sg has one inbound rule: HTTP, TCP, port 80, from 0.0.0.0/0. This lets any visitor load the website. Nothing else is allowed in.
• Why did I not open SSH to the internet? I did not need to log in to the server, because the User data script installs the web server and creates the page on first boot. An open SSH port (22) would only give attackers another way in, so leaving it closed keeps the instance safer.
• How is the S3 backup bucket protected? Block Public Access is on, so the bucket and its objects cannot be made public by accident. Default encryption is enabled, and versioning keeps older copies of a file. The bucket is a private backup, not the website itself.
• What does CloudTrail help me review? CloudTrail records supported AWS account activity: who did what, when, and from where. In Event history I could open a project event such as RunInstances and see its name and time. That is how I would check who changed or created resources.


4. Storage and backup
• What did I put in S3? I uploaded index.html and put old-brochure.txt under the archive/ prefix.
• What did versioning show? After I changed one sentence and uploaded index.html again with the same name, the object showed two versions. S3 kept the original instead of overwriting it, and the newer upload is the current version.
• What is an EBS snapshot for? A snapshot is a point-in-time backup of an EBS volume. If the disk is damaged or deleted, I can create a new volume from the snapshot and get back the data as it was when the snapshot was taken.
• What lifecycle rule did I create? The rule archive-to-infrequent-access applies only to the archive/ prefix and moves current versions to S3 Standard-IA after 30 days. Infrequent-access storage costs less for files that are rarely read.

5. Database and scaling choices
This website only shows fixed information (services, contact details and opening hours), so it does not need a database, a load balancer or Auto Scaling



8. Evidences

 VPC details showing the project CIDR.
<img width="954" height="345" alt="image" src="https://github.com/user-attachments/assets/7ec8336d-d27f-458e-9430-d1cb2e2328ec" />

 Public subnet details and its route table association.
<img width="808" height="351" alt="image" src="https://github.com/user-attachments/assets/e40e3887-2aa8-497f-b038-ce3ebd3ba9ec" />

Route table showing 0.0.0.0/0 to the Internet Gateway.
<img width="959" height="368" alt="image" src="https://github.com/user-attachments/assets/294bb092-7ec3-423a-8ca5-72047ed82e2d" />

 Security group inbound rules showing HTTP port 80 and no SSH port 22 rule.

 <img width="956" height="284" alt="image" src="https://github.com/user-attachments/assets/cf8fd233-a9cb-4831-9fae-f19ec793ff7b" />


EC2 instance running, with instance type, AMI, and EBS volume visible.
<img width="800" height="338" alt="image" src="https://github.com/user-attachments/assets/f6d98165-06b8-4f5a-a0bd-99cb6057d8d8" />

BrightPath Tutors website open in a browser using the instance public IPv4 address.

<img width="952" height="472" alt="image" src="https://github.com/user-attachments/assets/a60c950a-65f6-4dc2-ab93-50254e8817b8" />

Private S3 bucket with Block Public Access enabled and encryption shown.

<img width="809" height="352" alt="image" src="https://github.com/user-attachments/assets/ba057ded-723d-4094-80da-96f69fe7ab9e" />

 S3 versioning enabled and two versions of index.html visible.

<img width="944" height="342" alt="image" src="https://github.com/user-attachments/assets/ba99f5e2-355b-449e-a61a-6493df9510ae" />

 S3 lifecycle rule for prefix archive/ visible.

 <img width="941" height="352" alt="image" src="https://github.com/user-attachments/assets/d728c0b8-eeaa-4d20-9415-d232ce3b83c6" />

 EBS snapshot completed.

<img width="940" height="340" alt="image" src="https://github.com/user-attachments/assets/a209faa0-a65e-4ea5-b319-fe9d9af27c83" />


 CloudTrail Event history with one project event.


 <img width="745" height="341" alt="image" src="https://github.com/user-attachments/assets/1e378963-c2d0-47bb-9b49-2d2b922c444a" />




Cleanup proof: EC2 terminated, snapshot removed, bucket emptied/deleted, and networking resources removed.




<img width="835" height="234" alt="image" src="https://github.com/user-attachments/assets/d1a7d0e2-8b14-48c8-aaa8-b426c4a5f247" />


<img width="935" height="340" alt="image" src="https://github.com/user-attachments/assets/ad5e8095-0406-441b-a1c7-3c4f86ba2eb8" />


<img width="812" height="347" alt="image" src="https://github.com/user-attachments/assets/c14550ed-08d5-49de-bd27-e574a7b02fc3" />




<img width="955" height="342" alt="image" src="https://github.com/user-attachments/assets/8f46509f-bc6f-4929-b316-92dce27964e9" />


