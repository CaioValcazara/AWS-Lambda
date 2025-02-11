# Use an Amazon S3 trigger to invoke a Lambda function
This project involves creating a system where uploading a file to a specific storage location (S3bucket) triggers an automated process via AWS Lambda (email is sent with is verified by Amazon SES). In addition, all logs details are diplayed on CloudWatch.

Services Covered:
* AWS S3 (Simple Storage Service)
* AWS IAM (Identity Access Management)
* AWS Lambda (using Python Boto3 lib)
* AWS SES (Simple Email Service)
* AWS CloudWatch

![AWS-Lambda-S3-flow](https://aws-bucket-caio.s3.sa-east-1.amazonaws.com/AWS-Lambda-S3-flow.png)

## 1) Create a S3 bucket:

  **Process:**
  1. Access Amazon S3
  2. Click on "Create Bucket"
  3. Give the bucket a name
  4. Leave the default settings for now
  
  This step creates a bucket on S3 which will be used to store objects that will be notify when injested.

## 2) Manage Roles on IAM (Identity Access Management) to access to S3 and SES:
     
  **Process:** 
  1. Access IAM
  2. Clickk on "Roles"
  3. Create Role
  4. Select "AWS Service"
  5. On the use case menu, select "Lambda"
  6. Click on "Next"
  7. Filter and select the "AmazonS3FullAccess", "AmazonSESFullAccess" and "CloudWatchFullAccess".
  
  ![AWS-Lambda-S3-flow](https://aws-bucket-caio.s3.sa-east-1.amazonaws.com/IAM_Role_Lambda.png)

  This step creates a role for our Lambda function and determines which service it need to have access (Least Privilege Access). 
  
  In this case, it will have full acces to Amazon S3, Amazon SES and CloudWatch. 
  
  You can also enter in a level of detail ii which capabilities od each service the lambda will have access of.
  
## 3) Create the function on Lambda and Add the Trigger

  Why Using AWS Lambda? 
     
  AWS Lambda is a no server needed tool with automatic scalability that has the flexibility to integrates with others services to run a Python code, and for this example, we will be using  boto3 lib to automate process of sending a emails notification when a S3 object is injested.
  
  **Process 3.1:** 
  1. Access Lambda
  2. Click on "Function"
  3. "Create function"
  4. Give it a name
  5. Select the Python Version (in this case, Python 3.13)
  6. In the Permission section, change the default execution role for the created on step 2 by selection "use an existing role"
  7. Click on "Create Function"

  **Process 3.2:** 
  1. Click on "Add Trigger"											    
![AWS-Lambda-S3-flow](https://aws-bucket-caio.s3.sa-east-1.amazonaws.com/lambda_adding_trigger.png)
  2. Filter for S3 Bucket
  3. Select the Bucket create on step 1
  4. Select the event type to be "all object create events"
  5. Check the "Recursive Invocation" box

**Process 3.3:** 
1. On Code Section, paste the following code (you can also find the code on AWS-Lambda repo):

![AWS-Lambda-S3-flow](https://aws-bucket-caio.s3.sa-east-1.amazonaws.com/Lambda_Code_Section.png)
```md
    import json
    
    import boto3

    def lambda_handler(event, context):
	
	file_name = event['Records'][0]['s3']['object']['key']
	bucketname = event['Records'][0]['s3']['bucket']['name']
	
	print ("Event Details: ", event)
	print ("File name: ", file_name)
	print ("Bucket name: : ", bucketname)
	subject = 'Event from' + bucketname
	client = boto3.client("ses")
	body = """
			<br>
			
			Lambda has been triggered! This is a notification mail to inform you regarding s3 event.
			The file {} is inserted in the {} bucket.
		""".format (file_name, bucketname)
	message = {"Subject": {"Data" : subject}, "Body": {"Html": {"Data" : body}}}
	response = client.send_email(Source = "YOUR EMAIL", Destination = {"ToAddresses": ["YOUR EMAIL"]}, Message = message)
	print("The email has sent successfully")
```
**Why using boto3?**

Used to Automates AWS services for tasks like creating EC2 instances, S3 buckets, or 
managing RDS databases

**ATTETION** on the sourve and destination email highlighted as "YOUR EMAIL".

## 4) Create a email identity at SES (Amazon Simple Email Service)
  
  **Process:** 
  1. Search for SES
  2. In the Configuration Section, click on "Identities"
  ![AWS-Lambda-S3-flow](https://aws-bucket-caio.s3.sa-east-1.amazonaws.com/SES_identity.png)
  3. Click on "Create Identity"
  4. Select "Email Address"
  5. Type your desired email ID on the "Email Address" field
  6. Click on "Create Identity"
  7. Finally, check your input email by clicking on the url sent by AWS in the same input email
![AWS-Lambda-S3-flow](https://aws-bucket-caio.s3.sa-east-1.amazonaws.com/SES_email_verification.png)

## 5) Deploy Lambda Function
  
  **Process:**
  1.  Seach for Lambda
  2.  Click on the Function created
  3.  Press the Deploy Button

![AWS-Lambda-S3-flow](https://aws-bucket-caio.s3.sa-east-1.amazonaws.com/Deploy_Lambda.png)
  
## 6) Upload a file on S3 bucket

  **Process:**
  1. Search for S3
  2. Click on the S3 Bucket created on step 1
  3. Click on "Upload" button
  4. Upload any file

When a File is uploaded, the following notification via outlook will be displayed:

![AWS-Lambda-S3-flow](https://aws-bucket-caio.s3.sa-east-1.amazonaws.com/notification.jpeg)

## 7) Check at CloudWatch for log details Monitoring

  **Process:**
  1. Search CloudWatch
  2. On Logs Section, click Log groups and then on you trigger created to see the logs
![AWS-Lambda-S3-flow](https://aws-bucket-caio.s3.sa-east-1.amazonaws.com/cloudwatch.png)

You can see in detail all logs for each event that trigges the lambda function.

![AWS-Lambda-S3-flow](https://aws-bucket-caio.s3.sa-east-1.amazonaws.com/log+events.png)

Reference:
- https://docs.aws.amazon.com/pt_br/lambda/latest/dg/with-s3-example.html
- https://aws-bucket-caio.s3.sa-east-1.amazonaws.com/Python_For_DevOps_Complete_Notes_1738871563.pdf


