# Use an Amazon S3 trigger to invoke a Lambda function
This project involves creating a system where uploading a file to a specific storage location (S3bucket) triggers an automated process via AWS Lambda (email is sent with is verified by Amazon SES). In addition, all logs details are diplayed on CloudWatch.

Services Covered:
* AWS S3 
* AWS IAM (Identity Access Management)
* AWS Lambda (using Python Boto3 lib)
* AWS SES
* AWS CloudWatch

![AWS-Lambda-S3-flow](https://aws-bucket-caio.s3.sa-east-1.amazonaws.com/AWS-Lambda-S3-flow.png)


# Link to Video Explanation:

step by step:

  1) Create a S3 bucket
    Access Amazon S3 > Create Bucket > Give the bucket a name > leave the default settings for now 

  3) Manage Roles on IAM (Identity Access Management) to access to S3 and SES
    Access IAM > Roles > Create Role > Select AWS Service > On the use case menu, select Lambda > Next > Filter and select the AmazonS3FullAccess, AmazonSESFullAccess and CloudWatchFullAccess.

  This Step create a role for our Lambda function and determines which service it need to have access (Least Privilege Access). In this case, it will have full acces to Amazon S3, Amazon SES and CloudWatch. You can also enter in a level of detail ii which capabilities od each service the lambda will have access of.
  5) Create the function on Lambda > Add the Trigger

  Why Using AWS Lambda? 
     
    AWS Lambda is a no server needed tool with automatic scalability that has the flexibility to integrates with others services to run a Python code, and for this example, we will be using  boto3 lib to automate process of sending a emails notification when a S3 object is injested.
     
  7) Create a email identity at SES (Amazon Simple Email Service)
     
  9) Deploy Lambda Function
     
  11) Upload a file on S3 bucket
      
  13) Check at CloudWatch for log details
      Monitoring



Reference:
- https://docs.aws.amazon.com/pt_br/lambda/latest/dg/with-s3-example.html


