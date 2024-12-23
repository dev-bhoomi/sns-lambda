
#  SNS notifications on Lambda code execution 
This setup sends an SNS notification every time a Lambda function is executed. The notification can be delivered to email, SMS, or other endpoints via Amazon SNS.


## Key Feature

- Real-Time Notifications: Get instant updates whenever the Lambda function runs.
- Customizable Alerts: Configure the notification content to include details about the Lambda execution.
- Flexible Destinations: Send notifications to multiple endpoints like email, SMS, or applications.
- Secure and Reliable: Uses AWS IAM for secure interactions and ensures reliable delivery through SNS
## Steps

### **Step 1: Create an SNS Topic**

1. Go to the **AWS SNS Console**.
2. Click **Create Topic**.
    - Choose **Standard**.
    - Name your topic (e.g., `LambdaExecutionNotification`).
    - Click **Create Topic**.
3. **Create a Subscription**:
    - Select the topic.
    - Click **Create Subscription**.
    - Set **Protocol** to `Email` or another endpoint (e.g., SMS).
    - Enter the **email address** or endpoint.
    - Click **Create Subscription**.
4. **Confirm Subscription**:
    - Check your email and click the confirmation link to confirm the subscription.

---

### **Step 2: Add SNS Permissions to the Lambda Role**

1. Go to the **IAM Console**.
2. Select the role assigned to your Lambda function.
3. Attach a policy to allow publishing to the SNS topic:Replace:
    
    ```json
    json
    Copy code
    {
        "Effect": "Allow",
        "Action": "sns:Publish",
        "Resource": "arn:aws:sns:region:account-id:LambdaExecutionNotification"
    }
    
    ```
    
    - `region`: Your AWS region (e.g., `us-east-1`).
    - `account-id`: Your AWS account ID.
    - `LambdaExecutionNotification`: Your SNS topic name.

---

### **Step 3: Modify the Lambda Code to Publish to SNS**

1. Edit your Lambda function code to include SNS notifications:
    
    ```python
    import boto3
    
    def lambda_handler(event, context):
        sns_client = boto3.client('sns')
        topic_arn = "arn:aws:sns:region:account-id:LambdaExecutionNotification"
    
        # Publish a message to SNS
        response = sns_client.publish(
            TopicArn=topic_arn,
            Message="Lambda function executed successfully!",
            Subject="Lambda Execution Notification"
        )
    
        return {
            'statusCode': 200,
            'body': 'SNS Notification sent!'
        }
    
    ```
    
    Replace `region` and `account-id` with your AWS details.
    
2. Deploy the updated Lambda function.

---

### **Step 4: Test the Setup**

1. **Run the Lambda Function**:
    - Use the **Test** button in the Lambda Console or trigger the function with an event.
2. **Check Your Email or Endpoint**:
    - Verify you received the SNS notification after the Lambda function ran.
## Services
- AWS Lambda: Executes the function and triggers the notification.
- Amazon SNS (Simple Notification Service):
Sends notifications to the specified endpoints
