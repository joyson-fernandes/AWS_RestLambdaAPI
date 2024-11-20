# AWS Serverless Employee Management System (CRUD)

This project demonstrates how to set up an (CRUD) AWS serverless architecture using AWS Lambda, DynamoDB, and API Gateway to manage employee data.

## Prerequisites

- An AWS account
- AWS CLI installed and configured (optional)
- Basic knowledge of AWS services

## Step 1: Create a DynamoDB Table

1. **Log in to AWS Management Console**: Navigate to the [AWS Management Console](https://aws.amazon.com/console/).

2. **Navigate to DynamoDB**:
   - In the AWS Management Console, search for **DynamoDB** and select it.

3. **Create a New Table**:
   - Click on **Create Table**.
   - **Table Name**: `employee_info`
   - **Primary Key**: 
     - **Partition Key**: `employeeid` (String)
   - Leave other settings as default, or adjust them as necessary.
   - Click on **Create Table**.

4. **Confirm Table Creation**:
   - Wait a few moments for the table to create, then verify that it appears in your list of DynamoDB tables.

## Step 2: Create an IAM Role for Lambda

1. **Navigate to IAM**:
   - In the AWS Management Console, search for **IAM** and select it.

2. **Create a New Role**:
   - Click on **Roles** on the left sidebar.
   - Click on **Create role**.

3. **Select Trusted Entity**:
   - Select **AWS Service**, then choose **Lambda** from the list.
   - Click **Next: Permissions**.

4. **Attach Policies**:
   - Search for and attach the following policies:
     - **AWSLambdaBasicExecutionRole** (for CloudWatch logs)
     - **AmazonDynamoDBFullAccess** (for full access to DynamoDB)
   - Click on **Next: Tags**.

5. **Add Tags** (optional):
   - You can add tags for resource tracking, then click on **Next: Review**.

6. **Name Your Role**:
   - Give your role a name, e.g., `LambdaDynamoDBRole`.
   - Click on **Create role**.

## Step 3: Create a Lambda Function

1. **Navigate to Lambda**:
   - In the AWS Management Console, search for **Lambda** and select it.

2. **Create a New Function**:
   - Click on **Create function**.
   - Select **Author from scratch**.
   - **Function Name**: `EmployeeFunction`
   - **Runtime**: Select `Python 3.x` (latest version).
   - **Permissions**: Choose the existing role you created (`LambdaDynamoDBRole`).
   - Click on **Create Function**.

3. **Add Lambda Function Code**:
   - In the code editor, enter the from lambda_function.py
 
4. **Deploy the Lambda Function**:
   - After entering your code, click **Deploy** to save your changes.

## Step 4: Create an API Gateway

1. **Navigate to API Gateway**:
   - In the AWS Management Console, search for **API Gateway** and select it.

2. **Create a New API**:
   - Click on **Create API**.
   - Select **REST API** (Build).
   - Choose **New API**.
   - **API Name**: `EmployeeAPI`.
   - Click on **Create API**.

3. **Create Resources and Methods**:
   - Under your new API, click on **Actions** > **Create Resource**.
     - **Resource Name**: `employee` (path will be `/employee`).
   - Ensure that the **Enable CORS** option is checked, if available, or you will handle it later.
   - Click **Create Resource**.
   
   - With the `employee` resource selected, click on **Actions** > **Create Method**.
     - Choose **POST** from the dropdown and click the checkmark.
     - For the integration type, select **Lambda Function**.
     - **Lambda Function**: Enter the name of your Lambda function (`EmployeeFunction`).
     - Click on **Save** and then confirm the permissions if prompted.

4. **Enable CORS (for the POST method)**:
   - While still in the `employee` resource, click on **Actions** and select **Enable CORS**.
   - In the CORS configuration dialog:
     - For **Access-Control-Allow-Origin**, enter `*` or specify your frontend's origin.
     - For **Access-Control-Allow-Headers**, make sure to include headers like `Content-Type`, `X-Amz-Date`, `Authorization`, etc.
     - For **Access-Control-Allow-Methods**, include methods such as `GET`, `POST`, `OPTIONS`, etc.
   - Click on **Enable CORS and replace existing CORS headers**.

5. **Deploy the API**:
   - Click on **Actions** and select **Deploy API**.
   - Choose a deployment stage (you may create a new stage called `production` if one does not exist).
   - Click on **Deploy**.

## Step 5: Test the API

1. **Use Postman or cURL** to test your API endpoints:
   - For the **Save Employee** operation:
   ```bash
   curl -X POST https://your-api-gateway-url.amazonaws.com/production/employee \
   -H "Content-Type: application/json" \
   -d '{"employeeid": "103", "job_title": "Business Analyst", "full_name": "Schoen Fernandes", "Salary": 1500000}'
