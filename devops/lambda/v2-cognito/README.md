# v2-cognito Lambda Pipeline Install

## Follow the Guide on Confluence

[The guide on Confluence](https://drum.atlassian.net/wiki/spaces/DEV/pages/568459274/Install+CI+CD+for+v2-mediaconvert+with+CodePipeline+CodeBuild+and+CloudFormation) offers step-by-step instructions for setting up the CodePipeline.

## Edit ENV VARS and export:
```
export AWS_ACCOUNT=(Current AWS Account ID)
export ENV=(Current environment, e.g. dev/staging/production)
export SESADDR=(address you want to send email from)
```

## Add the roles and policies
```
aws iam create-role --role-name v2-cognito-cloudformation-service-role --assume-role-policy-document file://policies/cloudformation-service-role-trust-policy.json
aws iam put-role-policy --role-name v2-cognito-cloudformation-service-role --policy-name v2-cognito-cloudformation-service-role-policy --policy-document file://policies/cloudformation-service-role-policy.json
aws iam create-role --role-name v2-cognito-codebuild-service-role --assume-role-policy-document file://policies/codebuild-service-role-trust-policy.json
aws iam put-role-policy --role-name v2-cognito-codebuild-service-role --policy-name v2-cognito-codebuild-service-role-policy --policy-document file://policies/codebuild-service-role-policy.json
aws iam create-role --role-name v2-cognito-codepipeline-service-role --assume-role-policy-document file://policies/codepipeline-service-role-trust-policy.json
aws iam put-role-policy --role-name v2-cognito-codepipeline-service-role --policy-name v2-cognito-codepipeline-service-role-policy --policy-document file://policies/codepipeline-service-role-policy.json
aws iam create-role --role-name v2-cognito-lambda-function-service-role --assume-role-policy-document file://policies/lambda-function-service-role-trust-policy.json
aws iam put-role-policy --role-name v2-cognito-lambda-function-service-role --policy-name v2-cognito-lambda-function-service-role-policy --policy-document file://policies/lambda-function-service-role-policy.json
```

## Create S3 build bucket:
```
s3cmd mb s3://scout-$ENV-v2-cognito-builds
```

## Name for Pipeline, CodeBuild, etc:
```
scout-$ENV-v2-cognito
```

## CodeBuild ENV VAR:
```
codebuildBucketName - scout-$ENV-v2-cognito-builds
```

## CodePipeline Deploy Parameter Overrides:
```
cat parameters-template.txt | envsubst
```

## Add appropriate triggers to Cognito User Pool to test the function
```
Details TBD, maybe
```

