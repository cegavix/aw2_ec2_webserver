## Practice: Combining CD and Cloud Computing 

Tools/Concepts explored here
- Continuous deployment with GIT
- Crontab
- AWS EC2 Server and automating updates
- Synchronizing changes across AWS S3 Storage Spaces and Git
Deliverable: A deployed static website accessible via the EC2 instance and synchronized with an Amazon S3 bucket.

Requirements: 
1. Static website for a client on AWS infrastructure. 
2. Code in Git repository and needs to be synchronized with Amazon S3 for scalable and reliable hosting. 
3. Deployment should utilize an AWS EC2 instance running Linux as the web server.

Requirements:
1. **Setup EC2 Instance**: Launch a new AWS EC2 instance with Linux as the operating system. Configure security groups to allow inbound traffic on HTTP (port 80) and SSH (port 22)

2. **Install Web Server**: Install and configure a web server (e.g., Apache) on the EC2 instance to serve the static website files. Ensure the web server starts automatically on boot
hOW TO INSTALL http : 
dnf install -y httpd
 sudo systemctl start httpd 
sudo systemctl enable httpd # enable on boot


3. **Clone Git Repository**: Clone the Git repository containing the static website files onto the EC2 instance. Ensure that Git is installed on the instance.

dnf install git 
git clone https://github.com/cegavix/aw2_ec2_webserver.git

4. **Deploy Static Website**: Copy the contents of the cloned Git repository to the appropriate directory served by the web server on the EC2 instance.

cp aw2_ec2_webserver/* /var/www/html
Check if its running locally by getting ur local ip address, that says on aws (dont press it where it says to open link cos that doesnt work, just copy the ip in ur browser)
TADA!! U should see ur copied web pages

5. **Setup Amazon S3 Bucket**: Create a new Amazon S3 bucket to host the static website. Configure the bucket to allow public read access.

C

6. **Sync Content with S3**: Synchronize the contents of the web server's document root directory with the Amazon S3 bucket. Use the AWS CLI to perform this synchronization.
sudo yum install aws-cli 
aws configure # AWS Access Key ID, Secret Access Key, region, and output format.
Get these credentials by making a user in the IAM section of AWS (search for it) make
the user then generate access key - programmatic access after originally making the user. Select user, press generate access key . U will get a code and passkey 
Open the user, add permissions, where u want 7 or RW: AmazonS3FullAccess: Grants full access to S3 (including read and write) AmazonS3ReadOnlyAccess: Grants read-only access.



#synchronise with ur bucket 
aws s3 sync /var/www/html s3://my2ndbucketmthree
#verify the synchronization 
aws s3 ls s3://my2ndbucketmthree
#Delete any files that were deleted in the update
aws s3 sync /var/www/html s3://your-bucket-name --delete
CRONTAB -E : 0 * * * * aws s3 sync /var/www/html s3://your-bucket-name

7. **Testing and Validation**: Ensure that the static website is accessible via the EC2 instance's public IP address or domain name. Verify that changes pushed to the Git repository are automatically reflected on the website hosted on the EC2 instance and synchronized with the S3 bucket.

MAKE A CRONTAB JOB THAT RUNS A SCRIPT THAT PULLS FROM GITHUB, COPIES TO /var/www/html, pushes/synchronises to s3  bucket .
Make shell script

chmod +x ~/update_git.sh
sudo yum install cronie
crontab -e >> APPEND THE LINE: 0 * * * * /bin/bash update_website.sh






