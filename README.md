# terraform-aws-module-import-vpc-ec2-s3
Terraform import is a command that uses an existing cloud resource's unique ID to establish a link between that resource and your Terraform state, bringing it under Terraform's management.


### Create a Custom VPC:

- Go to VPC Dashboard > Your VPCs > Create VPC.

- Choose "VPC only".

- Name tag: `My-VPC`

- IPv4 CIDR block: `10.0.0.0/16`

- Click "Create VPC".


### Create a Public Subnet:

- Go to VPC Dashboard > Subnets > Create subnet.

-  VPC ID: Select My-VPC.

- Subnet name: `Public-subnet-a`

- Availability Zone: ap-south-1a (or your preferred AZ in ap-south-1).

- IPv4 CIDR block: `10.0.1.0/24`

- Click "Create subnet".

- Crucial Step: Enable Auto-assign Public IP:

- Select your Public-subnet-a subnet.

- Go to "Actions" > "Modify auto-assign IP settings".

- CHECK "Enable auto-assign public IPv4 address". Click "Save".



### Create an Internet Gateway (IGW):

- Go to VPC Dashboard > Internet Gateways > Create internet gateway.

- Name tag: `My-internet-gateway`

- Click "Create internet gateway".

- Attach to VPC: Select your new IGW, go to "Actions" > "Attach to VPC". Select My-VPC.

Record the IGW ID (e.g., igw-xxxxxxxxxxxxxxxxx).




### Create a Public Route Table:

- Go to VPC Dashboard > Route Tables > Create route table.

- Name tag: `Public-route-table-a`

- VPC: Select My-VPC.

- Click "Create route table".

- Record the Route Table ID (e.g., rtb-xxxxxxxxxxxxxxxxx).

- Associate with Subnet: Go to "Subnet associations" tab > "Edit subnet associations". Select your Public-subnet-a. Click "Save associations".

- Add Internet Route: Select your Public-route-table-a. Go to "Routes" tab > "Edit routes" > "Add route".

  - Destination: 0.0.0.0/0

  - Target: Select "Internet Gateway" and choose your My-internet-gateway.

- Click "Save changes".

- Associate with Subnet: Go to "Subnet associations" tab > "Edit subnet associations". Select your Public-subnet-a. Click "Save associations".








### Launch an EC2 Instance:

- Go to EC2 Dashboard > Instances > Launch instances.

- Name: `My-EC2`

- AMI: Search for "Amazon Linux 2023 AMI" and select a valid one for ap-south-1 (e.g., ami-0d03cb826412c6b0f).

- Instance type: t2.micro

- Key pair (login): Choose an existing key pair or create a new one.

- Network settings:

- VPC: Select My-VPC.

- Subnet: Select Public-subnet-a.

- Auto-assign public IP: This should show "Enable" (inherited from subnet).

- Firewall (security groups): Select "Create security group".

- Security group name: `My-EC2-SG` (Use this exact name).

- Description: Security group for My-EC2 instance (Use this exact description).

- Add inbound rule: Type SSH, Source Anywhere (0.0.0.0/0).

- Click "Launch instance".

- Record the EC2 Instance ID (e.g., i-xxxxxxxxxxxxxxxxx).

- Record the Custom Security Group ID (for My-EC2-SG, e.g., sg-xxxxxxxxxxxxxxxxx).

### Create an S3 Bucket:

- Go to S3 Dashboard > Buckets > Create bucket.

- Bucket name: `my-existing-s3-bucket-name-08-july-2025` (MUST be globally unique).

- AWS Region: Select AP-South-1.

- Block Public Access settings: Keep the default (all public access blocked).

- Tags: Do NOT add any tags here. Leave this section empty.

- Click "Create bucket".

- Record the S3 Bucket Name (e.g., my-existing-s3-bucket-name-08-july-2025).

- Find Default Security Group ID:

- Go to VPC Dashboard > Security Groups.

- Filter by your new VPC ID (My-VPC).

- Find the security group named default.

- Record its Security Group ID (e.g., sg-xxxxxxxxxxxxxxxxx)























