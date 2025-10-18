# AWS CloudFormation Static Web App

A simple static web application deployed on AWS using S3, CloudFront, and automated CI/CD pipeline with CodePipeline and CodeBuild.

## Architecture

- **S3 Bucket**: Hosts static website files
- **CloudFront**: CDN for global content delivery
- **CodePipeline**: Automated CI/CD pipeline
- **CodeBuild**: Build and deployment automation
- **GitHub**: Source code repository

## Files

- `index.html` - Main web page
- `style.css` - Styling
- `script.js` - JavaScript functionality
- `infrastructure.yml` - CloudFormation template for S3 + CloudFront
- `pipeline.yml` - CloudFormation template for CI/CD pipeline
- `buildspec.yml` - CodeBuild configuration

## Deployment

### 1. Deploy Infrastructure
```bash
aws cloudformation create-stack \
  --stack-name static-web-infrastructure \
  --template-body file://infrastructure.yml
```

### 2. Deploy Pipeline
```bash
aws cloudformation create-stack `
  --stack-name static-web-pipeline `
  --template-body file://pipeline.yml `
  --parameters `
    ParameterKey=GitHubOwner,ParameterValue=ssm1990 `
    ParameterKey=GitHubRepo,ParameterValue=aws-cldfn-smapp `
    ParameterKey=ConnectionArn,ParameterValue=arn:aws:codeconnections:us-east-1:314129306412:connection/7cb1995b-1b7e-4fa1-8b87-697b6d903563 `
    ParameterKey=WebsiteBucketName,ParameterValue=<PASTE_BUCKET_NAME_HERE> `
  --capabilities CAPABILITY_NAMED_IAM `
  --region us-east-1
```

## Configuration

Update the following parameters in `pipeline.yml`:
- `GitHubOwner`: Your GitHub username(ssm1990)
- `GitHubRepo`: Your repository name(aws-cldfn-smapp)

## Verify CodeStar Connection & Redeploy

### Quick checklist
- Ensure the CodeStar connection shows status **Available** in the AWS Console (Developer Tools → Connections). If it is **Pending**, complete the GitHub handshake.
- Confirm the connection, the pipeline, and your CLI region are all **us-east-1**.
- Make sure the pipeline's role is the one created by `pipeline.yml` and now includes `codestar-connections:UseConnection` on the provided `ConnectionArn`.
- If using a different account for the connection, add a resource policy on the connection to allow the pipeline role to `codestar-connections:UseConnection`.

### Redeploy/update the pipeline stack
```bash
aws cloudformation update-stack \
  --stack-name static-web-pipeline \
  --template-body file://pipeline.yml \
  --parameters \
    ParameterKey=GitHubOwner,ParameterValue=ssm1990 \
    ParameterKey=GitHubRepo,ParameterValue=aws-cldfn-smapp \
    ParameterKey=ConnectionArn,ParameterValue=arn:aws:codeconnections:us-east-1:314129306412:connection/7cb1995b-1b7e-4fa1-8b87-697b6d903563 \
  --capabilities CAPABILITY_NAMED_IAM

### Retry the pipeline
```bash
aws codepipeline start-pipeline-execution --name static-web-pipeline
```

## Features

- Automated deployment on code changes
- HTTPS redirect via CloudFront
- Static website hosting on S3
- Build automation with CodeBuild

## Prerequisites

- AWS CLI configured
- GitHub repository
- AWS account with appropriate permissions
