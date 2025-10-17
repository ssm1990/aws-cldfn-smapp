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
aws cloudformation create-stack \
  --stack-name static-web-pipeline \
  --template-body file://pipeline.yml \
  --parameters ParameterKey=GitHubOwner,ParameterValue=your-username \
               ParameterKey=GitHubRepo,ParameterValue=your-repo-name \
  --capabilities CAPABILITY_IAM
```

## Configuration

Update the following parameters in `pipeline.yml`:
- `GitHubOwner`: Your GitHub username(ssm1990)
- `GitHubRepo`: Your repository name(aws-cldfn-smapp)

## Features

- Automated deployment on code changes
- HTTPS redirect via CloudFront
- Static website hosting on S3
- Build automation with CodeBuild

## Prerequisites

- AWS CLI configured
- GitHub repository
- AWS account with appropriate permissions