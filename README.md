
 Microservice-Serverless-Architecture-Optimization
This project sets up a serverless RESTful API through Amazon API Gateway, which connects to an AWS Lambda function to execute CRUD (Create, Read, Update, Delete) operations on a DynamoDB table. The architecture demonstrates a Serverless Microservice design aimed at achieving high scalability, cost efficiency, high performance and minimal maintenance effort. Optimization has been done with Lambda Power Tuning Tool.

Serverless Lab Design
 

This is a microservice architecture built using serverless components on AWS.
Here’s how a request flows through the system:
1.	User calls an API endpoint → e.g. GET https://api.myshop.com/orders/123
2.	API Gateway receives the request, authenticates it, and routes it to the correct Lambda (getOrderById).
3.	Lambda Function executes your business logic — retrieves order data from DynamoDB.
4.	DynamoDB returns the data to Lambda.
5.	Lambda returns the result back through the API Gateway.
6.	User receives a JSON response.
AWS Well-Architected Framework & Power Tuning Impact
The AWS Well-Architected Framework is a set of best practices, design principles, and architectural guidelines developed by Amazon Web Services (AWS) to help cloud architects build secure, high-performing, resilient, and efficient infrastructure for their applications.

AWS Lambda Power Tuning is an open-source AWS Step Functions state machine that helps you find the optimal memory–performance configuration for your Lambda functions. It automatically runs a given Lambda function across various memory (and thus CPU) configurations, measures execution time and cost, and visualizes the trade-offs.

The 2 pillars of AWS Well Architected Framework - Cost Optimization and Performance Efficiency are directly impacted by Lambda Power Tuning Tool.

Pillar	How Lambda Power Tuning Contributes
Cost Optimization	Identifies the most cost-effective memory configuration, reducing overprovisioning and execution time costs.
Performance Efficiency	Balances CPU/memory to minimize latency and maximize throughput. Empirical testing replaces guesswork.


Steps followed for this project:
1.	Created a custom policy for least privilege – to grant Dynamo DB Allow permissions and the logs
2.	Created Lambda IAM Role  - For performing operations for Lambda 
3.	Created Lambda function  - To perform CRUD operations logic with Python 3.13 runtime engine.
4.	Created Dynamo DB Table – With Table name and Partition Key -id (String)
5.	Created REST API on API Gateway – Created POST Method for the API with DynamoDB Resource
6.	Integrated the API with Lambda function.
7.	Deployed the API to prod environment
8.	Executed the solution with AWS Lambda Power Tune with Step functions.

Lambda Function –
from __future__ import print_function
import boto3
import json
print('Loading function')
def lambda_handler(event, context):
    '''Provide an event that contains the following keys:
      - operation: one of the operations in the operations dict below
      - tableName: required for operations that interact with DynamoDB
      - payload: a parameter to pass to the operation being performed
    ''”
    #print("Received event: " + json.dumps(event, indent=2))
    operation = event['operation']
    if 'tableName' in event:
        dynamo = boto3.resource('dynamodb').Table(event['tableName'])
    operations = {
        'create': lambda x: dynamo.put_item(**x),
        'read': lambda x: dynamo.get_item(**x),
        'update': lambda x: dynamo.update_item(**x),
        'delete': lambda x: dynamo.delete_item(**x),
        'list': lambda x: dynamo.scan(**x),
        'echo': lambda x: x,
        'ping': lambda x: 'pong'
    }
    if operation in operations:
        return operations[operation](event.get('payload'))
    else:
        raise ValueError('Unrecognized operation "{}"'.format(operation))

Here is AWS Lambda Power Tuning Results chart and Analysis - 
 
•	128 MB costs $0.0000033/execution but take 1.5 seconds.
•	512 MB costs less $0.00000069/execution and take only 0.08 seconds.
If higher memory results in more throughput (fewer concurrent executions), the effective total cost to handle a workload can be lower — Lambda Power Tuning reveals this balance
Validated the solution with API endpoint URL running on Postman by inserting new records.

Postman Performance Test Report
 



 

<img width="594" height="622" alt="image" src="https://github.com/user-attachments/assets/c1ff9dd8-2c10-43e9-a992-b846593c2c67" />
