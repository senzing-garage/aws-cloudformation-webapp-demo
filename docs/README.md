# aws-cloudformation-webapp-demo

## Synopsis

The
[aws-cloudformation-webapp-demo](https://github.com/Senzing/aws-cloudformation-webapp-demo)
cloudformation template deploys Senzing using an AWS Cloudformation template.
Before deploying this Cloudformation template,
[aws-cloudformation-database-cluster](https://github.com/Senzing/aws-cloudformation-database-cluster)
must be deployed.

## Overview

The `aws-cloudformation-webapp-demo` demonstration is an AWS Cloudformation template that creates the following resources:

1. AWS infrastructure
    1. Elastic IP address
    1. IAM Roles, Policies, and Certificates
    1. Loadbalancers
    1. Logging
    1. NAT Gateway
    1. Routes
    1. Security Groups
    1. Subnets
1. AWS services
    1. AWS Cognito
    1. AWS Elastic Container Service (ECS) Fargate
    1. AWS Simple Queue Service (SQS)
1. Senzing services
    1. Senzing API server
    1. Senzing Entity Search Web App
    1. Senzing Redoer
    1. Senzing Stream-Loader
    1. Senzing Xterm

The following diagram shows the relationship of the docker containers in this docker composition.
Arrows represent data flow.

![Image of architecture](architecture.png)

This docker formation brings up the following docker containers:

1. *[senzing/entity-web-search-app](https://github.com/Senzing/entity-search-web-app)*
1. *[senzing/redoer](https://github.com/Senzing/redoer)*
1. *[senzing/senzing-api-server](https://github.com/Senzing/senzing-api-server)*
1. *[senzing/stream-loader](https://github.com/Senzing/stream-loader)*
1. *[senzing/xterm](https://github.com/Senzing/docker-xterm)*

GitHub repository for
[aws-cloudformation-webapp-demo](https://github.com/Senzing/aws-cloudformation-webapp-demo).

### Contents

1. [Preamble](#preamble)
    1. [Legend](#legend)
1. [Expectations](#expectations)
1. [Demonstrate using AWS Console](#demonstrate-using-aws-console)
1. [Using deployment](#using-deployment)
1. [Additional topics](#additional-topics)
1. [Parameters](#parameters)
1. [Outputs](#outputs)

## Preamble

At [Senzing](http://senzing.com),
we strive to create GitHub documentation in a
"[don't make me think](https://github.com/Senzing/knowledge-base/blob/main/WHATIS/dont-make-me-think.md)" style.
For the most part, instructions are copy and paste.
Whenever thinking is needed, it's marked with a "thinking" icon :thinking:.
Whenever customization is needed, it's marked with a "pencil" icon :pencil2:.
If the instructions are not clear, please let us know by opening a new
[Documentation issue](https://github.com/Senzing/aws-cloudformation-webapp-demo/issues/new?template=documentation_request.md)
describing where we can improve.   Now on with the show...

### Legend

1. :thinking: - A "thinker" icon means that a little extra thinking may be required.
   Perhaps there are some choices to be made.
   Perhaps it's an optional step.
1. :pencil2: - A "pencil" icon means that the instructions may need modification before performing.
1. :warning: - A "warning" icon means that something tricky is happening, so pay attention.

## Expectations

- **Time:** Budget 40 minutes to get the demonstration up-and-running.
- **Background knowledge:** This repository assumes a working knowledge of:
  - [AWS Cloudformation](https://github.com/Senzing/knowledge-base/blob/main/WHATIS/aws-cloudformation.md)

## Demonstrate using AWS Console

### Launch AWS Cloudformation

1. :warning: **Warning:** This Cloudformation deployment will accrue AWS costs.
   With appropriate permissions, the
   [AWS Cost Explorer](https://aws.amazon.com/aws-cost-management/aws-cost-explorer/)
   can help evaluate costs.
1. Visit [AWS Cloudformation with Senzing template](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=senzing-basic&templateURL=https://s3.amazonaws.com/public-read-access/aws-cloudformation-webapp-demo/cloudformation.yaml)
1. At lower-right, click on "Next" button.
1. In **Specify stack details**
    1. In **Stack name**
        1. Choose a stack name that is unique to you and 21 characters or less.  (Several resource types have a limit of 32 character names. The CFT uses the stack name and an 11 character suffix to name resources uniquely.)
    1. In **Parameters**
        1. In **Senzing installation**
            1. Accept the End User License Agreement.
            1. Optionally, choose a version of Senzing to install.
            1. Optionally, add a license string.
        1. In **Identify existing resources**
            1. Enter the stack name of the previously deployed
               [aws-cloudformation-database-cluster](https://github.com/Senzing/aws-cloudformation-database-cluster)
               Cloudformation stack
               Example:  `senzing-db`
        1. In **Security**
            1. Provide the email address for the administrative user.  Example: `me@example.com`
            1. Provide the permitted IP address block allowed to connect using CIDR notation.  Note: to open the installation to any IP address use: `0.0.0.0/0`.  For more on CIDR, see [Classless Inter-Domain Routing](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing)
        1. In **Security responsibility**
            1. Understand the nature of the security in the deployment.
            1. Once understood, enter "I AGREE".
    1. At lower-right, click "Next" button.
1. In **Configure stack options**
    1. At lower-right, click "Next" button.
1. In **Review senzing stack**
    1. Near the bottom, in **Capabilities**
        1. Check ":ballot_box_with_check: I acknowledge that AWS CloudFormation might create IAM resources."
    1. At lower-right, click "Create stack" button.

## Using deployment

1. Visit [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation/home).
    1. Make sure correct AWS region is selected.
1. Wait until the senzing stack status is `CREATE_COMPLETE`.
    1. Senzing formation takes about 10 minutes to fully deploy.
    1. May have to hit the refresh button a few times to get updated information.
1. Click on the senzing stack.
1. Click on "Outputs" tab.
1. Open the "0penFirst" value in a new web browser tab or window.
    1. Because this uses a self-signed certificate, a warning will come up in your browser.  Simply continue.
    1. In the "Sign in with your email and password" dialog box, enter the *UserName* and *UserInitPassword*
       values seen in the "Output" tab of the "senzing-basic" stack.  This is a one-time password.
    1. In **Change Password**, enter a new password.

## Additional topics

1. [How to load AWS Cloudformation queue](https://github.com/Senzing/knowledge-base/blob/main/HOWTO/load-aws-cloudformation-queue.md)

### Review AWS Cloudformation

The AWS resources created by the
[cloudformation.yaml](https://github.com/Senzing/aws-cloudformation-webapp-demo/blob/main/cloudformation.yaml)
template can be see in the [AWS Management Console](https://console.aws.amazon.com).

1. CloudFormation
    1. [Stacks](https://console.aws.amazon.com/cloudformation/home?#/stacks)
1. CloudWatch
    1. [Log groups](https://console.aws.amazon.com/cloudwatch/home?#logsV2:log-groups)
1. Cognito
    1. [UserPool](https://console.aws.amazon.com/cognito/users/#/pool/u)
1. Elastic Compute Cloud (EC2)
    1. [Load Balancers](https://console.aws.amazon.com/ec2/v2/home?#LoadBalancers:)
    1. [Network interfaces](https://console.aws.amazon.com/ec2/v2/home?#NIC)
    1. [Target groups](https://console.aws.amazon.com/ec2/v2/home?#TargetGroups:)
1. Elastic Container Service (ECS)
    1. [Clusters](https://console.aws.amazon.com/ecs/home?#/clusters)
    1. [Task Definitions](https://console.aws.amazon.com/ecs/home?#/taskDefinitions)
1. Identity and Access Management (IAM)
    1. Certificates
    1. [Policies](https://console.aws.amazon.com/iam/home?#/policies)
    1. [Roles](https://console.aws.amazon.com/iam/home?#/roles)
1. Lambda
    1. [Functions](https://console.aws.amazon.com/lambda/home?#/functions)
1. Relational Data Service (RDS)
    1. [Databases](https://console.aws.amazon.com/rds/home?#databases:)
    1. [Parameter groups](https://console.aws.amazon.com/rds/home?#parameter-groups:)
    1. [Subnet groups](https://console.aws.amazon.com/rds/home?#db-subnet-groups-list:)
1. Route53
    1. [RecordSet](https://console.aws.amazon.com/route53/v2/hostedzones#)
1. Simple Queue Service (SQS)
    1. [Queues](https://console.aws.amazon.com/sqs/v2/home)
1. System Manager Agent (SSM)
    1. [Parameter store](https://console.aws.amazon.com/systems-manager/parameters)
1. Virtual Private Cloud (VPC)
    1. [Elastic IP addresses](https://console.aws.amazon.com/vpc/home?#Addresses:)
    1. [Endpoints](https://console.aws.amazon.com/vpc/home?#Endpoints:)
    1. [Internet gateways](https://console.aws.amazon.com/vpc/home?#igws)
    1. [NAT gateways](https://console.aws.amazon.com/vpc/home?#NatGateways:)
    1. [Network ACLs](https://console.aws.amazon.com/vpc/home?#acls)
    1. [Route Tables](https://console.aws.amazon.com/vpc/home?#RouteTables)
    1. [Security Groups](https://console.aws.amazon.com/vpc/home?#SecurityGroups)
    1. [Subnets](https://console.aws.amazon.com/vpc/home?#subnets)
    1. [VPCs](https://console.aws.amazon.com/vpc/home?#vpcs)

### View results

1. Visit [AWS Cloudformation console](https://console.aws.amazon.com/cloudformation/home).
1. Choose appropriate "Stack name"
1. Choose "Outputs" tab.
    1. For descriptions of outputs, visit [Outputs](#outputs) further down this page.

## Parameters

Technical information on AWS Cloudformation parameters can be seen at
[Parameters](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/parameters-section-structure.html).

### AcceptEula

1. **Synopsis:**
   To use the Senzing code, you must agree to the End User License Agreement (EULA).
   This step is intentionally tricky to ensure that you make a conscious effort to accept the EULA.
1. **Required:** Yes
1. **Type:** String
1. **Allowed values:** See [SENZING_ACCEPT_EULA](https://github.com/Senzing/knowledge-base/blob/main/lists/environment-variables.md#senzing_accept_eula).
1. **Default:** None

### CidrInbound

1. **Synopsis:** A Classless Inter-Domain Routing
   ([CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing))
   value used to limit access to the system.
   This restricts the inbound traffic to requests from specified IP ranges.
   Examples:
    1. A system with the value `0.0.0.0/0` allows access from anywhere.
       Because of its "wide-open" nature, it is considered to be insecure.
    1. A system with the value `45.26.129.0/24` will allow access from IP addresses in the range `45.26.129.0` to `45.26.129.255`
    1. A system with the value `45.26.129.200/32` will allow access from a single IP address `45.26.129.200`.
1. **Required:** Yes
1. **Type:** String
1. **Allowed pattern:** Letters and numbers. Specifically: `'(?:\d{1,3}\.){3}\d{1,3}(?:/\d\d?)?'`
1. **Allowed values:** String in IPv4 [CIDR format](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing).
1. **Example:** 45.26.129.200/32
1. **Default:** None

### CognitoAdminEmail

1. **Synopsis:**
   An email address of the person administrating this Cloudformation.
   The email address will be used when email is sent to additional users via the
   [AWS Cognito web console](https://console.aws.amazon.com/cognito/users/#/pool/u).
1. **Required:** Yes
1. **Type:** String
1. **Allowed values:**
    1. A string in email format.
1. **Example:** `me@example.com`
1. **Default:** None

### DatabaseStack

1. **Synopsis:**
   The name of the cloudformation stack deployed with the
   [aws-cloudformation-database-cluster](https://github.com/Senzing/aws-cloudformation-database-cluster)
   cloudformation template.
   The DatabaseStack exported output values are used by the
   [aws-cloudformation-webapp-demo](https://github.com/Senzing/aws-cloudformation-webapp-demo).
1. **Required:** Yes
1. **Type:** String
1. **Example:** `senzing-db`
1. **Default:** None

### SecurityResponsibility

1. **Synopsis:**
   The Senzing webapp demo AWS Cloudformation uses
   [AWS Cognito](https://aws.amazon.com/cognito/) for authentication,
   and HTTPS (using a self-signed certificate) for encrypted network traffic
   to expose services through a single, internet-facing AWS Elastic Load Balancer.

   To enable additional security measures for the deployment in your specific environment,
   you'll need to consult with your AWS administrator.
   Examples of additional security measures:
    - [AWS Route53](https://aws.amazon.com/route53/) with genuine X.509 certificate
    - [AWS Web Application Firewall (WAF)](https://aws.amazon.com/waf/)
    - [AWS Shield](https://aws.amazon.com/shield/)
    - [AWS Firewall Manager](https://aws.amazon.com/firewall-manager/)
    - [Amazon API Gateway](https://aws.amazon.com/api-gateway/)
    - Restrictive value for [CidrInbound](#cidrinbound)
1. **Required:** Yes
1. **Type:** String
1. **Allowed values:**
    1. "I AGREE"
1. **Default:** None

### SenzingLicenseAsBase64

1. **Synopsis:**
   To ingest more than 100,000 records, a Senzing license is required.
   A binary version of the Senzing license, `g2.lic`, is not usable as a parameter in the text entry field.
   Instead, a [Base64](https://en.wikipedia.org/wiki/Base64) representation of the information is needed.
   An example of how to produce base64 from `g2.lic` on Linux and macOS:

   ```console
   base64 /opt/senzing/etc/g2.lic
   ```

   Copy the entire output from the command and paste into the text entry field.
1. **Required:** Yes if ingesting more than 100,000 records, otherwise no.
1. **Type:** String
1. **Allowed pattern:** Empty or Base64 characters. Specifically `^$|[^-A-Za-z0-9+/=]|=[^=]|={3,}$`
1. **Allowed values:** Base64 encoded string
1. **Example:**

   ```console
   AQAAADgCAAAAAAAAU2VuemluZwAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
   AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
   AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
   AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
   AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAARGVtbyBFeHBpcmVkAAAAAAAA
   AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAADIwMjAtMTItMTYA
   AAAAAAAAAAAARVZBTAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
   AAAAAAAAAAAAAAAAAAAAAFNUQU5EQVJEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
   AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAKCGAQAAAAAAMTk3Ni0wMS0wMQAAAAAAAAAAAABN
   T05USExZAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
   AAAAAAAARkdIST5XYOZ90kbyAbU7wM7XvPCwq/FgORZIekwFMg8zi3tCD0V5+12q72aqk0E6JOct
   +cPAq/T50N5Pf5nvJZ6TaW3TzQbnH/z5f/ALsWLydE2DPNvq3HuAjkjZpg2h7mb4OUqorGxDI9RX
   TX8hPjzYrBfMdOgl1DlRBVG36WwdpB8AnSfaegbYU+U/vfof+ff6mJk8gzPg+OGPwg21/S6i2TT4
   RbTCSYP/TpfXyJGE6dbQWEC9rFhYuWq3mFF3z7zFEcmxpNfZuBtYsxni8P3sDZ706RA+wcQF7TVg
   giJoK03W8kd6mk3X+fvc4ARJo9RarYInsAvSHKlr1KpxeebuirfqgSz+uEW6pqOD1fV0oHnFncdf
   jV2k2CqmIfThB/ONQcn/4/EIlhdzXqxSlXAGz6C7ApHq6xUCdLILx/NfdUEypHIfyabrpXKOKOPx
   zekhGztEzB0gSJNebEa++EKxHDOc1Sc0YD9q9KvcaGSPTjlCJeaNhufg9Sz/iXZMP+d4Vkp+Bn6p
   mfUPG7tKharEoRChUNfRms8wVyNxmz6LRw5Uy14Dlodd0LyBQRB9Tx8FVYMh5AElwjbQOoDOIRvi
   IQIGsUNp/ZkP7PdBxc/b9o3rjUsZCzyCtP+jflZSqMenzXCsTI1Xay6On2wSVwQdJ1/2eIwKEfCF
   hj4DZlY5+jSo
   ```

1. **Default:** None

### SenzingVersion

1. **Synopsis:**
   The version of Senzing installed onto the AWS Elastic File System.
   More information at [Senzing API Version History](https://senzing.com/releases/#api-releases).
1. **Required:** Yes
1. **Type:** Choice
1. **Default:** Latest version in the list.

## Outputs

### 0penFirst

1. **Synopsis:**
   An alias for [UrlWebApp](#urlwebapp).
   Since it's one of the first things to look at, it is listed first.
1. **Details:**
   It is listed first in alphabetical order because the name "cheats" and uses a zero instead of a capital "o".

### AccountID

1. **Synopsis:**
   The identifier of the AWS account used to create the cloudformation stack.
1. **Details:**
   This information will match the
   [AWS Management Console](https://console.aws.amazon.com/console/home)
   user dropdown "My Account" value.

### CertificateArn

1. **Synopsis:**
   Amazon Resource Name (ARN) of certificate used for SSL support.
1. **Details:**
   More information at
   [AWS LoadBalancer Console](https://console.aws.amazon.com/ec2/v2/home#LoadBalancers).
   Select a load balancer, view the "Listeners" tab, then click "View/edit certificates".

### Host

1. **Synopsis:**
   The hostname of the loadbalancer that is a proxy to all of the services.
1. **Details:**
   More information at [AWS Load Balancers console](https://console.aws.amazon.com/ec2/v2/home?#LoadBalancers:).

### QueueDeadLetter

1. **Synopsis:**
   The queue to which records that are not able to be ingested into Senzing Engine are sent.
   In otherwords, if the JSON message is malformed or Senzing denied inserting into the Senzing Engine.
1. **Details:**
   More information at [AWS SQS Console](https://console.aws.amazon.com/sqs/v2/home?#/queues).

### QueueInput

1. **Synopsis:**
   The queue from which records are ingested into Senzing Engine.
   In otherwords, this is the queue where records are sent to be inserted into the Senzing Engine.
1. **Details:**
   More information at [AWS SQS Console](https://console.aws.amazon.com/sqs/v2/home?#/queues).

### QueueOutput

1. **Synopsis:**
   The queue that is populated with responses from inserting records into the Senzing Engine.
   This is commonly called "WithInfo" information and is not enabled by default.
1. **Details:**
   More information at [AWS SQS Console](https://console.aws.amazon.com/sqs/v2/home?#/queues).

### SubnetPublic1

1. **Synopsis:**
   The first of two public subnets created.
1. **Details:**
   See the subnet having a Name in the form `{StackName}-ec2-subnet-public-1` in the
   [AWS Virtual Private Cloud console](https://console.aws.amazon.com/vpc/home?#subnets:).

### SubnetPublic2

1. **Synopsis:**
   The second of two public subnets created.
1. **Details:**
   See the subnet having a Name in the form `{StackName}-ec2-subnet-public-2` in the
   [AWS Virtual Private Cloud console](https://console.aws.amazon.com/vpc/home?#subnets:).

### UrlPrivateApiServer

1. **Synopsis:**
   A URL showing how to reach the
   [Senzing API Server](https://github.com/Senzing/senzing-api-server)
   directly from within the same VPC.

### UrlPrivateApiServerHeartbeat

1. **Synopsis:**
   A URL showing how to reach the
   [Senzing API Server](https://github.com/Senzing/senzing-api-server)
   directly from within the same VPC.
   The `/heartbeat` URI path simply demonstrates that the API server is responding.

### UrlWebApp

1. **Synopsis:**
   A URL showing how to reach the
   [Senzing Entity Search Web App](https://github.com/Senzing/entity-search-web-app).

### UrlXterm

1. **Synopsis:**
   A URL showing how to reach the
   [Senzing Xterm](https://github.com/Senzing/docker-xterm).
1. **Usage:**
   From this Linux terminal, `G2Command.py`, `G2Explorer.py`, `G2ConfigTool.py`,
   can be run.

### UserInitPassword

1. **Synopsis:**
   The one-time password for the [UserName](#username).
1. **Details:**
   When the one-time password is used, the user is prompted for a new password.
   Once a new password is submitted, the one-time password has no value.

### UserName

1. **Synopsis:**
   The user name submitted for the [CognitoAdminEmail](#cognitoadminemail).
   It is the initial user created to access the system.
1. **Details:**
   To add users, see [UserPool](#userpool)

### UserPool

1. **Synopsis:**
   The specific [UserPool](https://console.aws.amazon.com/cognito/users/#/pool/u) URL.
   It can be used to add, manage, or delete users for this Cloudformation.
