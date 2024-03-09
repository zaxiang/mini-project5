# miniproj5: Serverless Rust Microservice 
Project Overview:
- Create a Rust AWS Lambda function
- Implement a simple service
- Connect to a database

This project develops a serverless function on AWS Lambda utilizing Rust and Cargo Lambda, integrating with a database - AWS DynamoDB. This project focuses on leveraging AWS Lambda's capabilities, integrating with AWS API Gateway, and utilizing DynamoDB. The objective is to create a Lambda function capable of returning the number of students in a class given the an ap class name.


## Procedure
1. install Rust and Cargo Lambda, then in the desired folder, do `cargo lambda new <YOUR-PROJECT-NAME>`
2. inside the `<YOUR-PROJECT-NAME>/src` folder, update the code in files `main.rs` to achieve the functionality of returning the number of students in each ap class.
2. inside the directory just created update the dependencies in `Cargo.toml` to have:
```
[dependencies]
aws-config = { version = "1.1.7", features = ["behavior-version-latest"] }
aws-sdk-dynamodb = "1.16.1"
lambda_http = "0.10.0"
tokio = { version = "1", features = ["full"] }
```
3. Go to AWS Console and navigate to `IAM`. Create a new user and grant access (the same precesure as in the previous mini-projects):
![](user.png) 

4. Under Security Sredentials section, generate an access key for API access. Then, set up environment variables so that cargo lambda knows which AWS account and region to deploy to by typing these two lines in the terminal inside the rust project directory:
```
export AWS_ACCESS_KEY_ID="your_access_key_here"
export AWS_SECRET_ACCESS_KEY="your_secret_key_here"
```
5. Go back to AWS Console and navigate to `DynamoDB`. Create a table using the correct table name as written in the `main.rs` file and put the the name of primary key of the table in the `Partition key` box. We could manually add items to the table. TODO: there should be a faster way of doing it though.
![](table.png) 

6. Build the project by running `cargo lambda build --release`
7. Deploy the project by running `cargo lambda deploy`

8. Go to AWS Console and navigate to `Lambda`, inside `Function`: If cargo lambda deploy success, you may see the deployed lambda function on AWS, the lambda function should be ready and shown under the list, click on the function name (in my case: `serverless_rust_microservice`), under `Configuration`, click `Permission`, then click the `link` under your Role name

9. attach policies `AWSDynamoDBFullAccess` and `AWSLambdaBasicExecutionRole` to your role.

10. link  Lambda function with AWS API Gateway: click on `Add Trigger`, under the "choose a source", choose `API Gateway`, then choose `create a new API`, then choose `REST API`, click Add.

11. click on the link next to `API Gateway`, inside the Trigger tab, then create a new resource for your API's endpoint. For the created resource, implement an ANY method and associate it with your Lambda function. Click deploy, find stages and find your invoke URL.

![](lambda.png) 

12. click on the link after `API endpoint`: it will take you to a new page that looks like this:
![](initial_page.png) 

13. attach `?ap_class=american-history` at the end of that url, the it will returns the number of students in ap american-history class. 
![](api_endpoint.png) 