# Use an Amazon S3 trigger to invoke a Lambda function
This project involves creating a system where uploading a file to a specific storage location (S3bucket) triggers an automated process via AWS Lambda (email is sent with is verified by Amazon SES). In addition, all logs details are diplayed on CloudWatch.


![AWS-Lambda-S3-flow](https://aws-bucket-caio.s3.sa-east-1.amazonaws.com/AWS-Lambda-S3-flow.png)


# Link to Video Explanation:

step by step:

  1) Create a S3 bucket
     
  3) Manage Roles on IAM (Identity Access Management) to access to S3 and SES
     
  5) Create the function on Lambda > Add the Trigger
     Why Using AWS Lambda? AWS Lambda is a no server needed tool with automatic scalability that has the flexibility to integrates with others services to run a Python code, and for this example, we will be using  boto3 lib to automate process of sending a emails notification when a S3 object is injested.
     
  7) Create a email identity at SES (Amazon Simple Email Service)
     
  9) Deploy Lambda Function
     
  11) Upload a file on S3 bucket
      
  13) Check at CloudWatch for log details
      Monitoring



Reference:
- https://docs.aws.amazon.com/pt_br/lambda/latest/dg/with-s3-example.html


