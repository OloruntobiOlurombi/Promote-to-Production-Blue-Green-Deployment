# Topic: Promote-to-Production-Blue-Green-Deployment

### Overview

> In this project we will write a set of jobs that promotes a new environment to production and decommissions the old environment in an automated process without any downtime.

#### Prerequisites

- You should have finished the previous project below:

> Project: Remote Control Using Ansible,
> Project: Infrastructure Creation,
> Project: Config and Deployment, and
> Project: Rollback

#### Instructions

> There are a few manual steps we need to take to "prime the pump". These manual steps enable steps that will be automated later.

##### Let The Fun Begins!!!

### STEPS

##### Step 1: Create an S3 bucket (Manually)

> Create a public S3 Bucket (e.g., mybucket644752792305) manually. If you need help with this, follow the instructions found in the documentation.

> Create a simple HTML page (version 1) and name it index.html. It could be as simple as:

```
<!DOCTYPE html>
<html>
  <head>
      <title>Version 1</title>
  </head>

  <body>
      <h1>Hello World - version 1</h1>
  </body>
</html>
```

> Upload the index.html page to your bucket. Follow these steps if you need help. Make sure you can browse to the home page.

> Enable the Static website hosting in that bucket and set index.html as default page of the website. See the snapshot below.

<img width="1141" alt="Screen Shot 2022-07-21 at 10 59 18 AM" src="https://user-images.githubusercontent.com/40290711/180282460-5d43afe4-a12b-495a-84f6-127c7ee1e870.png">


> Attach a bucket policy 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::Bucket-Name/*"
            ]
        }
    ]
}
```

##### Step 2: Create a Cloudformation Stack (Manually)

> Use a Cloudformation template to create a [Cloudfront Distribution](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-cloudfront-distribution.html), and later in this exercise, use the same template to modify the Cloudfront distribution.

> Cloudformation is a service that allows creating any AWS resource using the YAML template files. In other words, it supports Infrastructure as a Code (IaC). Cloudfront is a network of servers that caches the content, such as website media, geographically close to consumers. Cloudfront is a Content Delivery Network (CDN) service.

> Create a Cloudformation template file named ***cloudfront.yml*** with the following contents:
```
Parameters:
# Existing Bucket name
  PipelineID:
    Description: Existing Bucket name
    Type: String
Resources:
  CloudFrontOriginAccessIdentity:
    Type: "AWS::CloudFront::CloudFrontOriginAccessIdentity"
    Properties:
      CloudFrontOriginAccessIdentityConfig:
        Comment: Origin Access Identity for Serverless Static Website
  WebpageCDN:
    Type: AWS::CloudFront::Distribution
    Properties:
      DistributionConfig:
        Origins:
          - DomainName: !Sub "${PipelineID}.s3.amazonaws.com"
            Id: webpage
            S3OriginConfig:
              OriginAccessIdentity: !Sub "origin-access-identity/cloudfront/${CloudFrontOriginAccessIdentity}"
        Enabled: True
        DefaultRootObject: index.html
        DefaultCacheBehavior:
          ForwardedValues:
            QueryString: False
          TargetOriginId: webpage
          ViewerProtocolPolicy: allow-all
Outputs:
  PipelineID:
    Value: !Sub ${PipelineID}
    Export:
      Name: PipelineID
```
> Execute the cloudfront.yml template above with the following command: 

```
aws cloudformation deploy \
--template-file cloudfront.yml \
--stack-name production-distro \
--parameter-overrides PipelineID="${S3_BUCKET_NAME}" \ # Name of the S3 bucket you created manually.
```
> In the command above, replace ${S3_BUCKET_NAME} with the actual name of your bucket (e.g., mybucket644752792305). Also, note down the stack name - production-distro for the future. You will need the cloudfront.yml file in the automation steps later again.

<img width="681" alt="Screen Shot 2022-07-20 at 5 55 15 PM" src="https://user-images.githubusercontent.com/40290711/180286720-aaecf32e-8bb3-4c86-bef7-e9b3ccf28f28.png">
> Execute the cloudfront.yml template file that will create a CloudFront distribution (CDN) based on the bucket name mentioned against PipelineID parameter

