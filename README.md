# ce8_assignment_2_14

Assignment 2.14

Answer the following:

1. Does SNS guarantee exactly once delivery to subscribers?

Answer:
   
SNS provides a best-effort delivery mechanism. It does not guarantee that a message will be delivered to a subscriber, nor does it support native mechanisms to ensure that messages are received only once. This model is suitable for applications where the occasional loss of messages is tolerable.

Reference: https://lumigo.io/aws-sns/aws-sns-vs-sqs-6-key-differences-limitations-how-to-choose/#:~:text=It%20does%20not%20guarantee%20that%20a%20message%20will,is%20tolerable.%20SQS%20offers%20more%20robust%20delivery%20guarantees.


2. What is the purpose of the Dead-letter Queue (DLQ)? This a feature available to SQS/SNS/EventBridge.

Answer:
   
Dead-letter Queue (DLQ) is an SQS queue for which a message that can’t be consumed or delivered successfully can be moved to, for troubleshooting and problem isolation and investigation and reprocessing purposes. Both Amazon SNS and EventBridge can be configured to move problematic messages or events to DLQ for further analysis and reprocessing. 

 
3. How would you enable a notification to your email when messages are added to the DLQ?

Answer:

Follow the following steps:
   
a. Create an AWS SNS Topic and SNS subscription and configure Protocol as Email and Endpoint as email address in the subscription. 

b. Create an AWS Lambda function that will be triggered by SQS DLQ activities. The Lambda function will point to the SNS Topic ARN to send pre-configured email if there are messages added to the DLQ.
Below is the sample Lambda function:

import boto3
client = boto3.client('sns')

def lambda_handler(event, context):
  response = client.publish(TopicArn='<SNS-topic-arn>',Message="DLQ has a new message")
  print("A message has just been added to DLQ")
  return(response)

c. Configure Lambda function trigger in the SQS DLQ and point to the Lambda function created in the above step.
   
