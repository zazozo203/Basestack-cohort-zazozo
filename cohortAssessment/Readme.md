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



