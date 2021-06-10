# Serverless Function on AWS

## How to use this template?

1. Click "Use this template" and create a new repo under the same organization

2. Edit the file `serverless.yml`

    * Change `service` to a meaningful namespace for all of your AWS resources (no spaces)
    
      > Example: `position-apy-monitor`
    
    * Change `PROJECT` to your AWS project name (with spaces and owner)

      > Example: `Base Assets Squad by Zlotin`

    * This template has two functions. If you don't need *writer* that is scheduled repeatedly and *reader* that is triggered by GET requests, edit `functions` to create the functions you need. See [serverless.com reference](https://www.serverless.com/framework/docs/providers/aws/guide/serverless.yml/) and [serverless.com examples](https://www.serverless.com/examples/).

3. Edit the file `handler.js` to add your logic

## What does this template provide?

* A *writer* function that is triggered every X minutes that is supposed to do various checks, crunch some data and write the result as JSON ready for consumption by clients (AJAX).

* A *reader* function that is triggered by GET requests and takes an example parameter. See the deploy log in Github Actions to get the actual URL endpoint on AWS.

* Ability to read and write persistent data to AWS EFS. You will receive your own directory specified in `process.env.HOME_DIR`.

* Automatic CI using Github Actions that will deploy your lambda functions to AWS on every commit to master. You don't need to open AWS console at all, not even for the first deployment.

## How is this template pre-configured for AWS?

The template relies on several shared resources on AWS that were already created manually. These include a VPC and the EFS instance. Both are designed to be shared by all lambda functions. You're not supposed to create any AWS resources manually.

Several AWS values and resource IDs are defined on the Github Organization level as organization secrets. The CI workflow pulls them from Github secrets and injects them as environment variables for the serverless.com deploy tool. These incluse the AWS credentials (an AWS user was created for the CI), ARN accesspoint for the EFS instance, a security group for EFS access and a subnet ID for EFS access.

If you need to monitor your lambda function or get execution/error logs, login to AWS console and look for Lambda functions under the region defined in `serverless-provider.yml`.
