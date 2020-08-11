# DLPservices

## VTDLP Services overview

<img src="images/DLP.png" width="300">

This CloudFormation currently deploys ID Minting service and Resolution service. 
* [ID Minting service](https://github.com/vt-digital-libraries-platform/mint)
* [Resolution service](https://github.com/vt-digital-libraries-platform/resolution-service)

### Deploy VTDLP Services using CloudFormation stack
#### Step 1: Launch CloudFormation stack
[![Launch Stack](https://cdn.rawgit.com/buildkite/cloudformation-launch-stack-button-svg/master/launch-stack.svg)](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?&templateURL=https://vtdlp-dev-cf.s3.amazonaws.com/8ed815b6ecf9d231cc07d862d50e85ef.template)

Click *Next* to continue

#### Step 2: Specify stack details

| Name | Description |
|:---  |:------------|
| Stack name | any valid name |
| NSTableName | a DynamoDB table |
| NOIDNAA | a valid string. e.g. 53696 |
| NOIDScheme | ark:/ |
| NOIDTemplate | a valid string. e.g. eeddeede |
| Image404 | 404 Image URL |
| REGION | a valid AWS region. e.g. us-east-1  |

#### Step 3: Configure stack options
Leave it as is and click **Next**

#### Step 4: Review
Make sure all checkboxes under Capabilities section are **CHECKED**

Click *Create stack*

### Deploy VTDLP Resolution Service application using SAM CLI

To use the SAM CLI, you need the following tools.

* SAM CLI - [Install the SAM CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html)
* [Python 3 installed](https://www.python.org/downloads/)
* Docker - [Install Docker community edition](https://hub.docker.com/search/?type=edition&offering=community)

To build and deploy your application for the first time, run the following in your shell:

```bash
sam build --use-container
```

Above command will build the source of the application. The SAM CLI installs dependencies defined in `requirements.txt`, creates a deployment package, and saves it in the `.aws-sam/build` folder.

To package the application, run the following in your shell:
```bash
sam package --output-template-file packaged.yaml --s3-bucket BUCKETNAME
```
Above command will package the application and upload it to the S3 bucket you specified.

Run the following in your shell to deploy the application to AWS:
```bash
sam deploy --template-file packaged.yaml --stack-name STACKNAME --s3-bucket BUCKETNAME --parameter-overrides 'NSTableName=DDBTableName Region=us-east-1 Image404=https://images/404.jpg' --capabilities CAPABILITY_IAM CAPABILITY_AUTO_EXPAND --region us-east-1
```