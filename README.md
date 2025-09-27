### Static Website CI/CD to S3 with CodePipeline

# Step 1: Create an S3 Bucket.
Create S3 Bucket for (Target for the Website) and update the bucket resource policy to allow traffic from Cloudfront.
Bucklet:
<img width="1671" height="830" alt="image" src="https://github.com/user-attachments/assets/61028b8c-f3bd-488d-95d0-a02a3a56e458" />

Bucket Policy:
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::angular-static-app01/*"
        },
        {
            "Sid": "AllowCloudFrontServicePrincipal",
            "Effect": "Allow",
            "Principal": {
                "Service": "cloudfront.amazonaws.com"
            },
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::angular-static-app01/*",
            "Condition": {
                "ArnLike": {
                    "AWS:SourceArn": "arn:aws:cloudfront::598010641694:distribution/E2EN0C2FDNN7LA"
                }
            }
        }
    ]
}

# Step 2: Building the Pipeline.

<img width="1895" height="788" alt="image" src="https://github.com/user-attachments/assets/f565d375-9d6b-4002-bb1f-bc285f46c7be" />

Setting up BuildSpec file.

version: 0.2
phases:
  install:
    runtime-versions:
      nodejs: 20
    commands:
      - echo Installing source NPM dependencies...
      - npm install -g @angular/cli

  pre_build :
    commands:
      - echo Installing source NPM dependencies...
      - npm install

  build:
    commands:
      - echo Build started on `date`
      - echo Building the Angular app...
      - ng build -c production

artifacts:
    files:
      - '**/*'
    base-directory: dist/my-angular-project  

 # Step 3 : Setting up Cloudfront Distribution Setup

 <img width="1901" height="723" alt="image" src="https://github.com/user-attachments/assets/6510aebe-a025-4659-871b-c36ecabf0a14" />

Error handling
<img width="1907" height="551" alt="image" src="https://github.com/user-attachments/assets/55573fbf-ac2f-4810-82dd-4a15db1f03a1" />

Invalidation : 
Invalidations are used to clear the Cache 

### Final Working Application on hitting the cloudfront domain:
<img width="1858" height="809" alt="image" src="https://github.com/user-attachments/assets/b13d72a3-565e-4a8f-b2ee-f067fabb7203" />


