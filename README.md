# Website & Mobile starter project

This is a sample template for Website & Mobile starter project. This application has an API Gateway endpoint to update and view entries in a DynamoDB table.
Below is a brief explanation of what we have generated for you:

* `README.md` - this file
* `events/` - this directory contains sample events that you can use to test using SAM CLI
* `events/event-get-all-items.json` - local test event for `src/handlers/get-all-items.js` from SAM CLI (https://github.com/awslabs/aws-sam-cli)
* `events/event-get-by-id.json` - local test event for `src/handlers/get-by-id.js` from SAM CLI (https://github.com/awslabs/aws-sam-cli)
* `events/event-put-item.json` - local test event for `src/handlers/put-item.js` from SAM CLI (https://github.com/awslabs/aws-sam-cli)
* `src/` - this directory contains all source code
* `__tests__/` - this directory contains all test files
* `__tests__/unit` - this directory contains all Jest unit test files (https://jestjs.io/)
* `.gitignore` - Git uses it to determine which files and directories to ignore
* `package.json` - this file is a central repository of configuration for tools(https://docs.npmjs.com/files/package.json)
* `buildspec.yml` - this file is used by AWS CodeBuild to package your application for deployment to AWS Lambda (https://docs.aws.amazon.com/codebuild/latest/userguide/build-spec-ref.html)
* `template.yml` - this file contains the AWS Serverless Application Model (AWS SAM) used by AWS CloudFormation to deploy your application to AWS Lambda and the other AWS resources (https://docs.aws.amazon.com/lambda/latest/dg/serverless_app.html)
* `env.json` - this file is needed to test locally with SAM CLI. It contains values to override the environment variables that are already defined in your function template (https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-using-invoke.html)

# What's the next step?

## 0. Get your endpoint
1. Go to the Lambda Console
2. Select `Applications` on the left navigation panel
3. Select your Start Right application from the Applications list
4. Copy your endpoint from the `Endpoint details` card

## 1. Try putItemAPI associated with putItemFunction
1. Replace `<YOUR-ENDPOINT>` and run the following commands from your terminal. The commands added `{"id":"id1", "name":"name1"}` and `{"id":"id2", "name":"name2"}` to your database

```bash
curl -d '{"id":"id1", "name":"name1"}' -H "Content-Type: application/json" -X POST <YOUR-ENDPOINT>
```

```bash
curl -d '{"id":"id2", "name":"name2"}' -H "Content-Type: application/json" -X POST <YOUR-ENDPOINT>
```

## 2. Try GetAllItemsAPI associated with getAllItemsFunction
0. Make sure you finished the first step (added items to the DynamoDB table)
1. Replace `<YOUR-ENDPOINT>` and run the following command from your terminal. The command gets all items you just created

```bash
curl <YOUR-ENDPOINT>
```
2. You should see 2 items: `[{"id":"id1","name":"name1"},{"id":"id2","name":"name2"}]`

### If you want to check the values in the actual table:
1. Go to the Lambda Console
2. Select `Applications` on the left navigation panel
3. Select your Start Right application from the Applications list
4. Select `SampleTable` from the Resources list in the Resources card
5. This will redirect you to the table overview in the DynamoDB Console
6. Select `Items` tab
7. You should see 2 items in the table

    ---------------
    | id  | name  |
    ---------------
    | id1 | name1 |
    | id2 | name2 |
    ---------------

## 3. Try GetByIdAPI associated with getByIdFunction
0. Make sure you finished the first step (added items to the DynamoDB table)
1. Replace `<YOUR-ENDPOINT>` and run the following command from your terminal. The command gets the item which id is "id1"

```bash
curl <YOUR-ENDPOINT>/id1
```
2. You should see `{"id":"id1","name":"name1"}`

## Unit test
`Jest`(https://jestjs.io/) is used for testing and it is already added in `package.json` under `scripts`.
The following command to run the tests:

```bash
npm install
npm run test
```

# Run locally with SAM CLI
You can even test your Lambda functions on your machine!

## Requirements

* AWS CLI already configured with Administrator permission
* [NodeJS 10.x installed](https://nodejs.org/en/download/)
* [Docker installed](https://www.docker.com/community-edition)
* [Debug with VS Code](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-using-debugging-nodejs.html)

## Local development

### 0. Replace your table name in env.json
To develop locally, we need to specify the environment variables, because they're not read from Lambda.
1. Go to Lambda Console
2. Select `Applications` on the left navigation panel
3. Select your Start Right application from the Applications list
4. In the Resources table, copy the field "Physical ID" of the SampleTable. It will have this format: `<APPLICATION_NAME>-SampleTable-<RANDOM_STRING>`
5. Open `env.json` and replace "<TABLE-NAME>" with the table name you just copied

### 1. Start server at http://127.0.0.1:3000/

```bash
sam local start-api --env-vars env.json
```
More info: https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-using-invoke.html

### 2. Try putItemAPI associated with putItemFunction

**Invoking function locally through local API Gateway**

```bash
curl -d '{"id":"id5", "name":"name5"}' -H "Content-Type: application/json" -X POST http://127.0.0.1:3000/
```

You should see `{"id":"id5","name":"name5"}`

### 3. Try GetAllItemsAPI associated with getAllItemsFunction

```bash
curl http://127.0.0.1:3000/
```

You should at least see `[{"id":"id5","name":"name5"}]`

### 4. Try GetByIdAPI associated with getByIdFunction

```bash
curl http://127.0.0.1:3000/id5
```

You should see `{"id":"id5","name":"name5"}`

### 5. Learn more about SAM CLI
You can find more information and examples from [SAM CLI Documentation](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-test-and-debug.html).
