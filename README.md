## Table of Contents
1.  [Introduction](#introduction)
2.  [Getting Started](#getting-started)
3.  [Software Installation](#software-installation)
4.  [Running Chofer locally](#running-chofer-locally)
5.  [Current Software Versions](#current-software-versions)
6.  [Infrastructure automation](#infrastructure-automation)
  
## Introduction
- This ReadMe will guide you through the installation process, so that you can run the Chofer application on your local machine.  
- The instructions below help you run Chofer locally using a containerless method. If you want to run using containers, follow this guide instead: [Running locally with Containers]

  **If you have any problems with installation, please contact Zareen Subah at zareen.subah@toyota.com, or reach out ot one of your team members.**
  
  **For any issues regarding your Toyota laptop, reach out to the Toyota Help Desk at https://tmna.service-now.com/1ts, or call 1-855-349-9898**

## Getting Started 
1.  Get Admin Rights for your local machine, via a 1TS Request.  You can do this by calling the Toyota Help Desk at 1-855-349-9898
2.  For the majority of the installation process, you will be using Windows Powershell in Administrator mode to run the installation commands.  
    - To run a Powershell as an Administrator, you will first need to gain admin access to your device, then right click the Windows Powershell icon, and select 'Run as Administrator'.
3. If you are using a Mac, you will be using home brew for managing all installations.


## Software Installation
#### 1. Node.js
-- First make sure you are using Node.js with an Active LTS Release, currently **v18**.
-- This is made easy with a version manager such as [nvm](https://github.com/nvm-sh/nvm) which allows for version switching.
```bash
# Installing a new version of node.js
$ nvm install 18.12.0
> Downloading and installing node v18.12.0...
> nvm use 18.12.0 
> Now using node v18.12.0 (npm v8.19.4)

# Checking your version
$ node --version
> v18.12.0
```
If you have not already installed NVM on your local machine, you can click this link to download. 
https://github.com/coreybutler/nvm-windows#readme

#### 2. Yarn
-- If you have not already installed Yarn on your local machine, you can open up an Admin Powershell, and enter the following command to install and upgrade Yarn:
```
npm install --global yarn
```
-- You can then check that Yarn has been installed properly by running:
```
  yarn --version
```
-- If you have any installation issues, see the documentation here: https://classic.yarnpkg.com/en/docs/install/#windows-stable

#### 3.  Postgres 14
- Use the following link and install PostgresSQL Version 14.9 for your machine:
-- https://www.enterprisedb.com/downloads/postgres-postgresql-downloads
  

## Running Chofer locally
#### First, Now you can install the project dependecies using the following command:
  ```
  yarn install
  ...
  ✨  Done in 53.96s.
  ```

**Note: canvas install**
##### You may find that the `yarn install` command above fails being unable to install the _canvas_ package. This is caused from the Backstage upgrade moving from node v14 to v18. If so, you need to install the following dependencies manually. Once done, re-running `yarn install` should complete successfully.

##### Dependencies: `pkg-config, cairo, pango, libpng, jpeg, giflib, librsvg`

##### If you use homebrew to help manage dependencies, you can execute the following command to easily install these:
##### `brew install pkg-config cairo pango libpng jpeg giflib librsvg`.
&nbsp;
  
#### Next, Create an empty `.env` file within the the ace-self-service-mono project folder.  Make sure that the `.env` file is located at the root of the project folder. Copy the contents below, and paste them into the `.env` file that you just created.  Add appropriate values/tokens instead of XXX 

#### Please Note: 
- **You will need to be added to the correct Azure AD group, and will need the proper Azure AD credentials in your .env file to be able to authenticate yourself to Chofer locally. To do so, please contact zareen.subah@toyota.com when you reach this step**
- **Update your credentials for DATABASE_PASSWORD by entering the password from pgAdmin**
- **Update your credentials for GITHUB_TOKEN by entering your PAT generated from GitHub**
- **Make sure to get your programmatic keys for AWS credentials every day as they expire daily**
- **Ensure the AWS credentials (AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY, AWS_SESSION_TOKEN) are written in all CAPS, as they are case sensitive and will error out**
- **Refer to [GitHub Token Permissions] to add the necessary permissions. You do not need all permissions.**

```dotenv
# common
# common part
FRONTEND_BASE_URL=http://localhost:3000
BACKEND_BASE_URL=http://localhost:7007
ENVIRONMENT=development

#used by docker only. 
FRONTEND_PORT=3000

AWS_CAM_ACCOUNTS_URL=XXX
AWS_CAM_API_KEY=XXX
AWS_CAM_CONTACTS_URL=XXX
AWS_CAM_USER_URL=XXX

# AWS integration
AWS_SQS_NOTIFICATION_URL=https://sqs.us-east-1.amazonaws.com/873364552094/chofer-test-integration
AWS_CHARGEBACK_URL=https://m2simjm8a9.execute-api.us-west-2.amazonaws.com/sandbox/v1
AWS_OPEN_SEARCH_ENDPOINT = https://vpc-ace-chofer-nonprod-z3mdmozinv4lxhj7uv65drgitq.us-west-2.es.amazonaws.com
AWS_SECURITY_SCORECARD_URL=https://zwdqepozkb.execute-api.us-west-2.amazonaws.com/ace-nonprod-svc/security/search
BUSINESS_ORGUNIT_URL=https://zwdqepozkb.execute-api.us-west-2.amazonaws.com/ace-nonprod-svc/orgunit


# AWS credentials
# used for local development, obtain your set from britive app on
# TMNA's https://myapplications.microsoft.com/
# NOTE:Keys have to be upper case only, value is as-is
AWS_ACCESS_KEY_ID=XXX
AWS_SECRET_ACCESS_KEY=XXX
AWS_SESSION_TOKEN=XXX

S3_BUCKET_NAME=XXX
S3_REGION=XXX

# THESE should correspond to your local `docker-compose` file for local dev
DATABASE_HOST=127.0.0.1
DATABASES_PORT=5432
DATABASE_USER=postgres
DATABASE_PASSWORD=XXX
TIMEOUT=1000000
LOG_LEVEL=debug

# GH interaction section
GITHUB_TOKEN=XXX
AUTH_BACKEND_TOKEN_KEY=XXX

# AZURE AD auth
AUTH_MICROSOFT_CLIENT_ID=XXX
AUTH_MICROSOFT_TENANT_ID=XXX
AUTH_MICROSOFT_CLIENT_SECRET=XXX

# datadog - null is mandatory for local if you dont have actual keys
DATADOG_APPLICATION_ID=null
DATADOG_CLIENT_TOKEN=null

SEARCH_INDEX_INTERVAL=720
```

##### Build using yarn does tsc and build
```
yarn build
...
✨  Done in 14.86s.
```
#### Run unit tests
```
yarn run test
...
Test Suites: 45 passed, 45 total ( this number should increase every sprint )
Tests:       130 passed, 130 total  ( this number should increase with each checkin )
Snapshots:   0 total
Time:        9.742 s, estimated 16 s
Ran all test suites in 11 projects.
✨  Done in 11.01s.

```
#### Running the frontend
```
yarn run dev:fe-local
```

If the commands work correctly, you should be able to view the Chofer Sign In page at localhost:3000 in your browser.

#### Running the Backend
Now open a new terminal and run the backend with this command:
```bash
yarn run dev:be-local
```
Return to your web browser, where the front end should still be running on localhost:3000.  You should now be able to click on the "Sign in with Microsoft Azure" button, and log in to Chofer with your Toyota email and password.  


## Current Software Versions
Node.js: 18.12.0
Yarn: 1.22.19
Docker: 20.10.8
Docker-Compose: 1.29.2
Minikube: 1.24.0
Postgres: 14.9
&nbsp;

## Infrastructure automation
Please see [Infrastructure automation] for details.

**Resources**

- [aws_caller_identity](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/caller_identity)
- [aws_ecr_repository](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/ecr_repository)
- [aws_iam_policy](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_policy)
- [aws_iam_policy_document](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/iam_policy_document)
- [aws_iam_role_policy_attachment](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role_policy_attachment)

- - [aws_iam_role_policy_attachment]

