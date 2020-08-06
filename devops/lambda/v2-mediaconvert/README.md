# Mediaconvert Image Extractor Lambda Pipeline Install

## Edit ENV VARS and export:
```
export AWS_ACCOUNT=(Current AWS Account ID)
export MCBUCKET=(S3 bucket to monitor and perform conversions in)
export MCENDPOINT=(MediaConvert Endpoint URL for your account - see guide at https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/emc-examples-getendpoint.html)
export ENV=(Current environment, e.g. dev/staging/production)
```

## Add the roles and policies
```
aws iam create-role --role-name mediaconvert-cloudformation-service-role --assume-role-policy-document file://policies/cloudformation-service-role-trust-policy.json
aws iam put-role-policy --role-name mediaconvert-cloudformation-service-role --policy-name mediaconvert-cloudformation-service-role-policy --policy-document file://policies/cloudformation-service-role-policy.json
aws iam create-role --role-name mediaconvert-codebuild-service-role --assume-role-policy-document file://policies/codebuild-service-role-trust-policy.json
aws iam put-role-policy --role-name mediaconvert-codebuild-service-role --policy-name mediaconvert-codebuild-service-role-policy --policy-document file://policies/codebuild-service-role-policy.json
aws iam create-role --role-name mediaconvert-codepipeline-service-role --assume-role-policy-document file://policies/codepipeline-service-role-trust-policy.json
aws iam put-role-policy --role-name mediaconvert-codepipeline-service-role --policy-name mediaconvert-codepipeline-service-role-policy --policy-document file://policies/codepipeline-service-role-policy.json
aws iam create-role --role-name mediaconvert-lambda-function-service-role --assume-role-policy-document file://policies/lambda-function-service-role-trust-policy.json
aws iam put-role-policy --role-name mediaconvert-lambda-function-service-role --policy-name mediaconvert-lambda-function-service-role-policy --policy-document file://policies/lambda-function-service-role-policy.json
aws iam attach-role-policy --role-name mediaconvert-lambda-function-service-role --policy-arn "arn:aws:iam::aws:policy/AWSElementalMediaConvertFullAccess"
aws iam attach-role-policy --role-name mediaconvert-lambda-function-service-role --policy-arn "arn:aws:iam::aws:policy/AmazonAPIGatewayInvokeFullAccess"
aws iam attach-role-policy --role-name mediaconvert-lambda-function-service-role --policy-arn "arn:aws:iam::aws:policy/AmazonS3FullAccess"
aws iam create-role --role-name mediaconvert-service-role --assume-role-policy-document file://policies/mediaconvert-service-role-trust-policy.json
aws iam put-role-policy --role-name mediaconvert-service-role --policy-name mediaconvert-service-role-policy --policy-document file://policies/mediaconvert-service-role-policy.json
```

## Create S3 build bucket:
```
s3cmd mb s3://scout-$ENV-v2-mediaconvert-builds
```

## Follow the Guide to create the Pipeline and CodeBuild project
https://medium.com/@aashari/integrate-github-repository-to-lambda-function-using-codepipeline-b3072ad822fd

## Name for Pipeline, CodeBuild, etc:
```
scout-$ENV-v2-mediaconvert
```

## CodeBuild ENV VAR:
```
codebuildBucketName - scout-$ENV-v2-mediaconvert-builds
```

## CodePipeline Deploy Parameter Overrides:
```
cat parameters-template.txt | envsubst
```

## Run the S3 Trigger script if needed:
```
configure-s3-lambda-notification.sh $MCBUCKET mediaconvert-scout-flair-video-image-extractor
```

## Test the function
```
s3cmd ls s3://$MCBUCKET/ | sort -nk1 | tail -n 5
s3cmd put ~/Dropbox/Work/Backgrounds/xwing1.mp4 s3://$MCBUCKET/
s3cmd ls s3://$MCBUCKET/ | sort -nk1 | tail -n 5
```

