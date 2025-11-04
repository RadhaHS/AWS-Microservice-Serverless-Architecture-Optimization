
 <h1>Microservice-Serverless-Architecture-Optimization</h1>
This project sets up a serverless RESTful API through Amazon API Gateway, which connects to an AWS Lambda function to execute CRUD (Create, Read, Update, Delete) operations on a DynamoDB table. The architecture demonstrates a Serverless Microservice design aimed at achieving high scalability, cost efficiency, high performance and minimal maintenance effort. Optimization has been done with Lambda Power Tuning Tool.

<h2>Serverless Lab Design</h2>
<img width="1135" height="512" alt="Picture1" src="https://github.com/user-attachments/assets/52d4871c-05f5-47e1-935e-f50f427188dc" />


<h2>AWS Well-Architected Framework & Power Tuning Impact</h2>
The 2 pillars of AWS Well Architected Framework - Cost Optimization and Performance Efficiency are directly impacted by Lambda Power Tuning Tool.
<img width="1714" height="318" alt="tablepicture" src="https://github.com/user-attachments/assets/b7ec28cb-d7e7-477a-857d-503c6d549e66" />

<h2>Lambda Function </h2> 
The function serves as a dispatcher, directing incoming operations from the API Gateway payload to the corresponding DynamoDB action via boto3.

<pre> from __future__ import print_function
import boto3
import json

print('Loading function')

def lambda_handler(event, context):
    '''Provide an event that contains the following keys:

      - operation: one of the operations in the operations dict below
      - tableName: required for operations that interact with DynamoDB
      - payload: a parameter to pass to the operation being performed
    '''
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
        raise ValueError('Unrecognized operation "{}"'.format(operation)) </pre>

<h2>Here is AWS Lambda Power Tuning Results chart and Analysis</h2> 
<img width="2506" height="1083" alt="Powertunetool" src="https://github.com/user-attachments/assets/6769e578-4d7d-455e-bb1e-61fe9f7e1088" />

At 128 MB, the function costs approximately $0.0000033 per execution and takes about 1.5 seconds to complete.
At 512 MB, the same function costs only $0.00000069 per execution and finishes in roughly 0.08 seconds.

Conclusion: Increasing memory doesn’t always increase cost — in this case, more memory resulted in dramatically faster execution and a lower overall cost per invocation. Data-driven tuning helps identify the sweet spot where performance and cost are both optimized.





 

