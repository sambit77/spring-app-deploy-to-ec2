# spring-app-deploy-to-ec2
A demo springboot application deployed on EC2

### Steps to build and deploy the application to AWS EC2
1. Run the given app locally and access API at localhost:9090
2. Generate jar file from the application using following command
```
./mvnw clean install   
```
3. Jar file will be available at this location "target/demo-0.0.1-SNAPSHOT.jar"

#### Deploy the application to AWS-EC2

1. Create a s3 bucket in AWS and upload this jar file to s3 bucket
2. Create a IAM role with policy "AmazonS3fullAccess" for EC2
3. Launch a new EC2 Instance (with .pem key for ssh, allow access from public, t2micro)
4. Attach the IAM role created in step-2 to this EC-2 instance (In security tab)
5. Open port 9090 on the EC2 instance by writing a inbound rule in EC2's security group
6. Connect/SSH into EC2 instance using below steps 
    ```
   chmod 0400 MySpringbootServerLey.pem 
   ssh -i MySpringbootServerLey.pem ec2-user@ec2-13-126-226-7.ap-south-1.compute.amazonaws.com
   ```
L1: Give proper access to .pem key file (which will be downloaded suring EC2 instance creation)
L2: SSH into EC-2 instance using the downloaded key and EC2 public ipv4 dns
7. Install java on EC2 machine `sudo dnf install java-17-amazon-corretto`
8. Verify java installation `javac -version`
9. Copy the jar file from s3 Bucket to EC2 instance
    `aws s3 cp s3://myspringjar-sambit/realtimenotiapp-0.0.1-SNAPSHOT.jar app.jar`
10. Run the jar file in EC2 `java -jar app.jar`
11. API will be available <ec2-ip>:9090