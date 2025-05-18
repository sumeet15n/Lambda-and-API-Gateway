# APIs with Lambda and API Gateway

## Introducing this Project!

In this project, I set up the logic layer of a three-tier web app.

### Services Used

- Lambda
- API Gateway

### Concepts Learnt

- Creating and deploying a Lambda function
- Understanding and writing Lambda function code
- Creating and deploying APIs
- Setting up API resources
- Setting up API methods

### Architecture Diagram

![Image](https://github.com/sumeet15n/Lambda-and-API-Gateway/blob/master/Screenshots/SS0.png)

---

## Project Guide

### Step 1: Create a Lambda function

First, I navigated to Lambda and set the region and set my region as the one closest to my geographical location (ap-south-1) to minimize the latency (best practice).

I created a Lambda function, named it 'RetrieveUserData, selected Node.js as the runtime, and selected x86_64 as the architecture.

Next, I copied the below code (while ensuring that the region is correct) into my Lambda function code and deployed the Lambda function.

```
// Import individual components from the DynamoDB client package
import { DynamoDBClient } from "@aws-sdk/client-dynamodb";
import { DynamoDBDocumentClient, GetCommand } from "@aws-sdk/lib-dynamodb";

const ddbClient = new DynamoDBClient({ region: 'ap-south-1' });
const ddb = DynamoDBDocumentClient.from(ddbClient);

async function handler(event) {
    const userId = event.queryStringParameters.userId;
    const params = {
        TableName: 'UserData',
        Key: { userId }
    };

    try {
        const command = new GetCommand(params);
        const { Item } = await ddb.send(command);
        if (Item) {
            return {
                statusCode: 200,
                body: JSON.stringify(Item),
                headers: {'Content-Type': 'application/json'}
            };
        } else {
            return {
                statusCode: 404,
                body: JSON.stringify({ message: "No user data found" }),
                headers: {'Content-Type': 'application/json'}
            };
        }
    } catch (err) {
        console.error("Unable to retrieve data:", err);
        return {
            statusCode: 500,
            body: JSON.stringify({ message: "Failed to retrieve user data" }),
            headers: {'Content-Type': 'application/json'}
        };
    }
}

export { handler };
```
A screenshot of the Lambda code is also shown below.

![Image](https://github.com/sumeet15n/Lambda-and-API-Gateway/blob/master/Screenshots/SS1.png)

### Step 2: Set up the API Gateway

I navigated to API Gateway and created a new REST API, with 'UserRequestAPI' as the name and 'Regional' as the API endpoint type.

A screenshot of the successfully created API is shown below.

![Image](https://github.com/sumeet15n/Lambda-and-API-Gateway/blob/master/Screenshots/SS2.png)

### Step 3: Set up an API resource

I created a new API resource for my API created earlier, with path '/' and name 'users'.

A screenshot of the successfully created API resource is shown below.

![Image](https://github.com/sumeet15n/Lambda-and-API-Gateway/blob/master/Screenshots/SS3.png)

### Step 4: Set up an API method

I created a new API method for my API resource 'users', selected 'GET' as the method type. selected 'Lambda fuction' as the integration type, enabled Lambda proxy integration, and selected the created Lambda function and the same region for Lambda.

A screenshot of the configuration page is shown below.

![Image](https://github.com/sumeet15n/Lambda-and-API-Gateway/blob/master/Screenshots/SS4.png)

### Step 5: Deploy the API

I deployed the API to a new stage with stage name 'prod', as shown in the below screenshot.

![Image](https://github.com/sumeet15n/Lambda-and-API-Gateway/blob/master/Screenshots/SS5.png)

### Step 6: Delete Resources

Finally, I deleted all the created resources after completing this project to avoid any further charges on AWS.

---

## Project Reflection

This project took me approximately 1.5 hours to complete. The most challenging part was understanding the Lambda function code, and the most rewarding part was to successfully create the API resource and method and deploy the API.

Big thanks to NextWork (https://www.nextwork.org/) for this project! I highly recommend this platform to anyone who wants to learn DevOps concepts and complete more projects like this one. Happy learning!