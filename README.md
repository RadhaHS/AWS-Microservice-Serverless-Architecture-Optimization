
 <h1>Microservice-Serverless-Architecture-Optimization</h1>
This project sets up a serverless RESTful API through Amazon API Gateway, which connects to an AWS Lambda function to execute CRUD (Create, Read, Update, Delete) operations on a DynamoDB table. The architecture demonstrates a Serverless Microservice design aimed at achieving high scalability, cost efficiency, high performance and minimal maintenance effort. Optimization has been done with Lambda Power Tuning Tool.

<h2>Serverless Lab Design</h2>h2>

<h2>AWS Well-Architected Framework & Power Tuning Impact</h2>h2>
The 2 pillars of AWS Well Architected Framework - Cost Optimization and Performance Efficiency are directly impacted by Lambda Power Tuning Tool.

Pillar	How Lambda Power Tuning Contributes
Cost Optimization	Identifies the most cost-effective memory configuration, reducing overprovisioning and execution time costs.
Performance Efficiency	Balances CPU/memory to minimize latency and maximize throughput. Empirical testing replaces guesswork.

<h2>Steps followed for this project</h2>
1.	Created a custom policy for least privilege – to grant Dynamo DB Allow permissions and the logs
2.	Created Lambda IAM Role  - For performing operations for Lambda 
3.	Created Lambda function  - To perform CRUD operations logic with Python 3.13 runtime engine.
4.	Created Dynamo DB Table – With Table name and Partition Key -id (String)
5.	Created REST API on API Gateway – Created POST Method for the API with DynamoDB Resource
6.	Integrated the API with Lambda function.
7.	Deployed the API to prod environment
8.	Executed the solution with AWS Lambda Power Tune with Step functions.


<h2>Lambda Function </h2> –
The function serves as a dispatcher, directing incoming operations from the API Gateway payload to the corresponding DynamoDB action via boto3.
Lamda_function.py
<h2>Here is AWS Lambda Power Tuning Results chart and Analysis</h2> 
128 MB: $0.0000033 / execution — 1.5 s runtime
512 MB: $0.00000069 / execution — 0.08 s runtime

If higher memory results in more throughput (fewer concurrent executions), the effective total cost to handle a workload can be lower — Lambda Power Tuning reveals this balance




 

<img width="594" height="622" alt="image" src="https://github.com/user-attachments/assets/c1ff9dd8-2c10-43e9-a992-b846593c2c67" />
