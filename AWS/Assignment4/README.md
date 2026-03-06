# Assignment 4: Serverless API with Lambda, IAM, and API Gateway

## Objective
This Assignment demonstrates a hands-on, production-like serverless workflow on AWS. The goal was to build a simple REST API that accepts a `POST` request, processes it with a Lambda function, and stores the data in a DynamoDB table, following the principle of least privilege for IAM permissions.

## Architecture Diagram 

![Architecture Diagram](/AWS/Assignment4/Architechture-diagram4.png)

---

## Implementation Steps

### 1. DynamoDB Table Creation
First, a DynamoDB table named `students` was created to serve as the data store.

*   **Partition Key**: `id` (String)
*   **Capacity Mode**: On-demand

This setup ensures that we only pay for the write operations performed, which is ideal for workloads with unpredictable traffic.

![DynamoDB Table Setup](/AWS/Assignment4/Screenshot/DynamoDB.png)

### 2. IAM Permissions (Least Privilege)
Before creating the Lambda function, a dedicated IAM execution role was configured to ensure the function only has the permissions it absolutely needs.

The role includes two policies:
1.  **`AWSLambdaBasicExecutionRole`**: A managed policy that allows the function to write logs to Amazon CloudWatch.
2.  **`LambdaStudentPutItemPolicy`**: A custom inline policy that grants `dynamodb:PutItem` permission *only* on the `students` table.

This approach prevents the function from performing any unauthorized actions.

![IAM Role with Attached Policies](/AWS/Assignment4/Screenshot/iam.png)
![Custom PutItem Policy Details](/AWS/Assignment4/Screenshot/policy.png)

### 3. Lambda Function
A Python-based Lambda function was created to process incoming requests. The function's logic is as follows:
1.  Generate a unique `UUID` to serve as the primary key for the new database item.
2.  Capture the current timestamp.
3.  Store the request's payload, along with the generated ID and timestamp, into the `student` DynamoDB table.
4.  Return a structured JSON success message, including the `id` of the newly created item.
5.  Includes basic error handling and logging for debugging.

![Lambda Function Code](/AWS/Assignment4/Screenshot/Lambdafunction.png)

### 4. API Gateway REST API
Finally, an API Gateway was configured to provide a public HTTP endpoint.

*   **Endpoint**: A `POST` method was created on the `/submit` resource.
*   **Integration**: The endpoint is configured with a Lambda Proxy integration, which forwards the entire request to the Lambda function.
*   **CORS**: Cross-Origin Resource Sharing (CORS) was enabled 
*   **Deployment**: The API was deployed to a `prod` stage to make it publicly accessible.

![API Gateway Endpoint Configuration](/AWS/Assignment4/Screenshot/api.png)

---

## Testing and Validation
The entire workflow was tested by sending a `POST` request to the deployed API endpoint using Postman.

#### Postman Request
A successful request was sent with a sample JSON payload, and the API returned a `200 OK` status with the expected success message and the unique ID of the stored item.

![Postman Test Success](/AWS/Assignment4/Screenshot/POSTMAN.png)

#### DynamoDB Verification
After the test, the `student` table was checked to confirm that the data was successfully written. The new item, identified by its unique ID, contained the correct payload and timestamp.

![DynamoDB Item Details (JSON)](/AWS/Assignment4/Screenshot/DynamoDB-1.png)
![DynamoDB Table View](/AWS/Assignment4/Screenshot/DynamoDB-2.png)

#### CloudWatch Logs
The CloudWatch logs for the Lambda function were reviewed to confirm successful execution. The logs showed the request and report IDs, along with metrics for execution duration and memory usage, which are crucial for monitoring and debugging.

![CloudWatch Execution Logs](/AWS/Assignment4/Screenshot/cloudwatch-logs.png)

**Learnings:**

• The core serverless workflow: API Gateway receives a request, triggers a Lambda function, which then interacts with other services like DynamoDB.
• The critical role of IAM in the serverless model, specifically creating an execution role with least-privilege permissions for Lambda.    
• How to use CloudWatch Logs to effectively debug Lambda function errors.


**Challenge:** My Lambda function was timing out or returning a `502` error through API Gateway.

**Solution:** Checked the CloudWatch Logs and found a permissions error. The Lambda execution role was missing the `dynamodb:PutItem` permission to write to my table. Adding it to the IAM policy resolved the issue.
