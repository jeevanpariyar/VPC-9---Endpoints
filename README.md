## Introducing Today's Project!

### ✍️ What are we here to do today?

In this project, I will create an S3 Gateway VPC endpoint to connect my VPC privately to Amazon S3 without using the public internet. I'm doing this project to learn how to improve network security, configure bucket policies, and control access to AWS services through VPC endpoints.

### What is Amazon VPC?

Amazon VPC (Virtual Private Cloud) is an AWS service that allows me to create my own logically isolated virtual network in the AWS Cloud.

It is useful because it gives me control over IP address ranges, subnets, route tables, security groups, and network connections. It helps me securely deploy and manage AWS resources such as EC2 instances while controlling how they communicate with other resources and the internet.

### How I used Amazon VPC in this project

I used Amazon VPC to create a custom network (\`NextWork-vpc\`) with a CIDR block of \`10.0.0.0/16\` and a public subnet where I launched my EC2 instance. I configured a route table and created an S3 Gateway VPC Endpoint to allow my EC2 instance to access my S3 bucket privately without routing S3 traffic through the public internet. I also used a bucket policy and endpoint policy to control and test access securely.

### One thing I didn't expect in this project was...

One thing I didn't expect was that simply creating a VPC Endpoint would not be enough to access S3. I also had to associate the endpoint with my subnet's route table. This taught me how important route tables are for controlling network traffic in AWS.

### This project took me...

This project took me approximately 1-2 hours, including setting up the VPC, creating the S3 Gateway Endpoint, configuring policies, troubleshooting access, and testing the connection.

## In the first part of my project...

### Step 1 - Architecture set up

In this step, I will create a VPC, launch an EC2 instance in its public subnet, and create an S3 bucket with two files. These resources will help me test S3 connectivity before setting up a VPC endpoint.

### Step 2 - Connect to EC2 instance

In this step we're gonna connect to your EC2 instance and try access S3 through the public internet.

### Step 3 - Set up access keys

In this step, I will create Access Keys because to access your AWS services, we need to set up access keys right away.

### Step 4 - Interact with S3 bucket

In this step, I will return to my EC2 instance and use the AWS CLI to access my S3 bucket. I'll configure the AWS credentials and test the connection by listing my S3 bucket and its objects.

## Architecture set up

I launched a VPC named \`NextWork-vpc\`, an EC2 instance running Amazon Linux 2023 in its public subnet, and created a security group for SSH access. I also created an S3 bucket named \`nextwork-vpc-project-jeevanpariyar\` and uploaded two PNG files.

In this step, I created an Amazon S3 bucket named nextwork-vpc-project-jeevanpariyar in the same AWS Region as my VPC and uploaded two PNG files to the bucket.

![Architecture set up](https://nextwork.ai/intense_gray_jolly_kokako/uploads/749fab76-0a0b-5822-9b31-c7863050c9bd_4334d777)

## Access keys

### Credentials

To set up my EC2 instance to interact with my AWS environment, I configured:

1\. Access Key ID – identifies my IAM user.\
2\. Secret Access Key – authenticates my IAM user.\
3\. Default AWS Region – specifies the AWS Region where my resources are located.\
4\. Default output format – determines how AWS CLI displays command results.

An access key ID is a part of a credential!

Your credentials are made up of a username and password; think of the access key ID as the username.

You don't automatically have one, but you can create access keys IDs through AWS IAM.

The secret access key is like the password that pairs with your access key ID (your username). You need both to access AWS services.

Secret is a key word here - anyone who has it can access your AWS account, so we need to keep this away from anyone else!

### Best practice

The best practice alternative is to create an IAM role with the required permissions and attach it to the EC2 instance. This provides temporary credentials automatically without manually creating or storing access keys.

## Connecting to my S3 bucket

first command, try running aws s3 ls, which is a command used to list the S3 buckets in your account (yup, ls stands for list!).

The terminal responded with a list of my S3 buckets, including \`nextwork-vpc-project-jeevanpariyar\`. This indicated that the access keys I set up were working correctly and my EC2 instance could authenticate with AWS and access S3.

![Connecting to my S3 bucket](https://nextwork.ai/intense_gray_jolly_kokako/uploads/749fab76-0a0b-5822-9b31-c7863050c9bd_4334d778)

## Connecting to my S3 bucket

The command I ran was:\
aws s3 ls s3://nextwork-vpc-project-jeevanpariyar

This command lists all the objects stored in my S3 bucket. The terminal displayed the two uploaded PNG files, confirming that my EC2 instance could access the bucket.

![Connecting to my S3 bucket](https://nextwork.ai/intense_gray_jolly_kokako/uploads/749fab76-0a0b-5822-9b31-c7863050c9bd_4334d779)

## Uploading objects to S3

To upload a new file to my bucket, I first ran the command sudo touch /tmp/nextwork.txt

This command creates a new txt file on my EC2 Server .

The second command I ran was\
aws s3 cp /tmp/nextwork.txt s3://nextwork-vpc-project-jeevanpariyar

This command will copy the .txt file to the S3 bucket .

Finally, run aws s3 ls nextwork-vpc-project-jeevanpariyar to see what objects are listed in your bucket now.

![Uploading objects to S3](https://nextwork.ai/intense_gray_jolly_kokako/uploads/749fab76-0a0b-5822-9b31-c7863050c9bd_3e1e79a2)

## In the second part of my project...

### Step 5 - Set up a Gateway

In this step, I will create an S3 Gateway VPC endpoint to allow my VPC to communicate directly and privately with Amazon S3, without routing traffic through the public internet.

### Step 6 - Bucket policies

In this step, I will update my S3 bucket policy to allow access only through my VPC endpoint. This will block all other traffic and help me verify that my EC2 instance can still access S3 privately through the endpoint.

### Step 7 - Update route tables

In this step, I will test whether my EC2 instance can still access my S3 bucket after restricting the bucket policy to allow traffic only through my VPC endpoint. If access works, it confirms that my VPC endpoint is configured correctly and my EC2 instance is accessing S3 privately.

![In the second part of my project...](https://nextwork.ai/intense_gray_jolly_kokako/uploads/749fab76-0a0b-5822-9b31-c7863050c9bd_67081648)

### Step 8 - Validate endpoint conection

To validate my work, let's get our EC2 instance to interact with your S3 bucket one last time.

## Setting up a Gateway

A Gateway is a type of endpoint used specifically for Amazon S3 and DynamoDB (DynamoDB is an AWS database service).

### What are endpoints?

An endpoint in AWS is a service that allows private connections between your VPC and other AWS services without needing the traffic to go over the internet.

![Setting up a Gateway](https://nextwork.ai/intense_gray_jolly_kokako/uploads/749fab76-0a0b-5822-9b31-c7863050c9bd_09bcaa8a)

## Bucket policies

A bucket policy is a JSON-based access policy attached to an Amazon S3 bucket. It controls who can access the bucket and what actions they can perform, such as viewing, uploading, or deleting objects.

In this project, I will use a bucket policy to allow S3 access only through my VPC endpoint and block access from all other sources.

My bucket policy denies S3 access to everyone unless the request comes through my specified VPC endpoint. This ensures that only traffic from my VPC endpoint can access the bucket, blocking access from other sources, including the public internet.

![Bucket policies](https://nextwork.ai/intense_gray_jolly_kokako/uploads/749fab76-0a0b-5822-9b31-c7863050c9bd_7316a13d)

## Bucket policies

Right after saving my bucket policy, my S3 bucket page showed 'denied access' warnings. This was because...

I updated my route table to associate it with the S3 Gateway VPC Endpoint so that traffic from my EC2 instance to Amazon S3 would be routed privately through the endpoint instead of the public internet. This allowed my EC2 instance to access the S3 bucket securely, even after restricting bucket access to requests coming through the VPC endpoint.

![Bucket policies](https://nextwork.ai/intense_gray_jolly_kokako/uploads/749fab76-0a0b-5822-9b31-c7863050c9bd_4ec7821f)

## Route table updates

I went to VPC → Endpoints, selected my S3 Gateway VPC Endpoint, and opened the Route tables tab. I clicked Manage route tables, selected the route table associated with my public subnet, and clicked Modify route tables. This associated the route table with the endpoint and automatically added a route for S3 traffic through the VPC Endpoint.

After updating my public subnet's route table, my terminal could return the objects in the S3 buckrt not without the traffic flowing throgh the public internet.

![Route table updates](https://nextwork.ai/intense_gray_jolly_kokako/uploads/749fab76-0a0b-5822-9b31-c7863050c9bd_d116818e)

## Endpoint policies

An endpoint policy is a resource-based policy attached to a VPC Endpoint that controls which AWS resources and actions can be accessed through that endpoint.

For example, an S3 Gateway Endpoint policy can allow or deny access to specific S3 buckets and operations, such as listing or uploading files. It adds an extra layer of access control to my VPC Endpoint..

I went to VPC → Endpoints, selected my S3 Gateway Endpoint, and edited its endpoint policy. I changed the policy's \`Effect\` from \`Allow\` to \`Deny\` and tested access from my EC2 instance. The S3 access request failed with an Access Denied error. I then changed it back to \`Allow\`, restoring access to my S3 bucket through the endpoint.

![Endpoint policies](https://nextwork.ai/intense_gray_jolly_kokako/uploads/749fab76-0a0b-5822-9b31-c7863050c9bd_3e1e79a3)
