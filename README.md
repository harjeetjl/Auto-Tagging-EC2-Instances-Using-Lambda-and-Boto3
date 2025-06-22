
# EC2 Auto-Tagging on Launch Using AWS Lambda and Boto3

This solution introduces an automated mechanism to tag newly launched EC2 instances with relevant metadata, such as the launch date and the identity of the user or role that initiated the launch. It utilizes AWS Lambda, Boto3, and Amazon CloudWatch Events (EventBridge).

---

## Purpose

The primary goal is to streamline instance auditing and tracking by ensuring that all EC2 instances launched in the account carry the following tags:

- `LaunchDate`: The date when the instance was launched.
- `LaunchedBy`: The IAM user or assumed role responsible for launching the instance.

---

## Components

The project includes the following files and resources:

- `LambdaAutoTagEC2Role_harjeet.py` – Contains the Lambda function code used for tagging EC2 instances.
- `README.md` – Documentation for setup, configuration, and testing.

---

## Pre-requisites

Before beginning the setup, ensure you have:

- An active AWS account
- Permissions to perform the following:
  - Launch EC2 instances
  - Create Lambda functions and IAM roles
  - Manage CloudWatch Event rules
- Python 3.x runtime (used in the Lambda function)

---

## Step-by-Step Setup

### 1. IAM Role for Lambda Execution

1. Navigate to **IAM > Roles > Create Role**.
2. Select **AWS Lambda** as the trusted entity.
3. Attach the `AmazonEC2FullAccess` managed policy.
4. Assign a role name, for example: `harjeet_LambdaAutoTagEC2Role`.

---

### 2. Deploy the Lambda Function

1. Open the **AWS Lambda Console** and select **Create Function**.
2. Choose:
   - Runtime: Python 3.x
   - Execution role: Use the role created in the previous step
3. Upload or paste the Lambda code from the reference:
   [LambdaAutoTagEC2Role_harjeet.py](https://github.com/harjeetjl/Auto-Tagging-EC2-Instances-Using-Lambda-and-Boto3/blob/main/LambdaAutoTagEC2Role_harjeet.py)
4. Ensure the Lambda has appropriate permissions to be invoked by CloudWatch by attaching a resource-based policy if required.

---

### 3. Configure CloudWatch Event Rule

1. Open **CloudWatch > Rules > Create Rule**.
2. Set the event source as **Event Pattern**.
3. Use the following event pattern to capture EC2 state changes:
   ```json
   {
     "source": ["aws.ec2"],
     "detail-type": ["EC2 Instance State-change Notification"],
     "detail": {
       "state": ["running"]
     }
   }
   ```
4. Set the target as the Lambda function deployed in the previous step.
5. Name the rule `AutoTagEC2Launch_harjeet` and enable it.

---

## Validation Procedure

After the configuration is complete:

1. Launch a new EC2 instance.
2. Wait approximately 30–60 seconds.
3. Navigate to the EC2 dashboard and select the instance.
4. Under the **Tags** section, confirm the presence of the following:
   - `LaunchDate` with the current date
   - `LaunchedBy` with the IAM user name or `Unknown` if not identifiable

---

## Screenshots

- EC2 instance creation showing applied tags
- Lambda function code and trigger settings
- IAM role configuration and attached policies
- CloudWatch Event rule configuration
- EC2 instance with tags applied after Lambda execution

