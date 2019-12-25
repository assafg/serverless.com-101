## Install the Serverless CLI

```bash
npm i -g serverless
```

----

## Create a project from a template

```bash
serverless create --template aws-nodejs --path <myService>
```

```
"aws-clojure-gradle", "aws-clojurescript-gradle", "aws-nodejs", "aws-nodejs-typescript", "aws-alexa-typescript", "aws-nodejs-ecma-script", "aws-python", "aws-python3"
"aws-groovy-gradle", "aws-java-maven", "aws-java-gradle", "aws-kotlin-jvm-maven", "aws-kotlin-jvm-gradle", "aws-kotlin-nodejs-gradle", "aws-scala-sbt", "aws-csharp"
"aws-fsharp", "aws-go", "aws-go-dep", "aws-go-mod", "aws-ruby", "aws-provided"
"tencent-go", "tencent-nodejs", "tencent-python", "tencent-php"
"azure-nodejs"
"cloudflare-workers", "cloudflare-workers-enterprise", "cloudflare-workers-rust"
"fn-nodejs", "fn-go"
"google-nodejs", "google-python", "google-go"
"kubeless-python", "kubeless-nodejs"
"openwhisk-java-maven", "openwhisk-nodejs", "openwhisk-php", "openwhisk-python", "openwhisk-ruby", "openwhisk-swift"
"spotinst-nodejs", "spotinst-python", "spotinst-ruby", "spotinst-java8"
"twilio-nodejs"
"aliyun-nodejs"
"plugin"
"hello-world"
```

---

## handler.js

```javascript
'use strict';

module.exports.hello = async event => {
  return {
    statusCode: 200,
    body: JSON.stringify(
      {
        message: 'Go Serverless v1.0! Your function executed successfully!',
        input: event,
      },
      null,
      2
    ),
  };

  // Use this code if you don't use the http event with the LAMBDA-PROXY integration
  // return { message: 'Go Serverless v1.0! Your function executed successfully!', event };
};

```

---

serverless.yml

```yaml
# Welcome to Serverless!
#
# This file is the main config file for your service.
# It's very minimal at this point and uses default values.
# You can always add more config options for more control.
# We've included some commented out config examples here.
# Just uncomment any of them to get that config option.
#
# For full config options, check the docs:
#    docs.serverless.com
#
# Happy Coding!

service: myservice
# app and org for use with dashboard.serverless.com
#app: your-app-name
#org: your-org-name

# You can pin your service to only deploy with a specific Serverless version
# Check out our docs for more details
# frameworkVersion: "=X.X.X"

provider:
  name: aws
  runtime: nodejs10.x

# you can overwrite defaults here
#  stage: dev
#  region: us-east-1

# you can add statements to the Lambda function's IAM Role here
#  iamRoleStatements:
#    - Effect: "Allow"
#      Action:
#        - "s3:ListBucket"
#      Resource: { "Fn::Join" : ["", ["arn:aws:s3:::", { "Ref" : "ServerlessDeploymentBucket" } ] ]  }
#    - Effect: "Allow"
#      Action:
#        - "s3:PutObject"
#      Resource:
#        Fn::Join:
#          - ""
#          - - "arn:aws:s3:::"
#            - "Ref" : "ServerlessDeploymentBucket"
#            - "/*"

# you can define service wide environment variables here
#  environment:
#    variable1: value1

# you can add packaging information here
#package:
#  include:
#    - include-me.js
#    - include-me-dir/**
#  exclude:
#    - exclude-me.js
#    - exclude-me-dir/**

functions:
  hello:
    handler: handler.hello
#    The following are a few example events you can configure
#    NOTE: Please make sure to change your handler code to work with those events
#    Check the event documentation for details
#    events:
#      - http:
#          path: users/create
#          method: get
#      - websocket: $connect
#      - s3: ${env:BUCKET}
#      - schedule: rate(10 minutes)
#      - sns: greeter-topic
#      - stream: arn:aws:dynamodb:region:XXXXXX:table/foo/stream/1970-01-01T00:00:00.000
#      - alexaSkill: amzn1.ask.skill.xx-xx-xx-xx
#      - alexaSmartHome: amzn1.ask.skill.xx-xx-xx-xx
#      - iot:
#          sql: "SELECT * FROM 'some_topic'"
#      - cloudwatchEvent:
#          event:
#            source:
#              - "aws.ec2"
#            detail-type:
#              - "EC2 Instance State-change Notification"
#            detail:
#              state:
#                - pending
#      - cloudwatchLog: '/aws/lambda/hello'
#      - cognitoUserPool:
#          pool: MyUserPool
#          trigger: PreSignUp
#      - alb:
#          listenerArn: arn:aws:elasticloadbalancing:us-east-1:XXXXXX:listener/app/my-load-balancer/50dc6c495c0c9188/
#          priority: 1
#          conditions:
#            host: example.com
#            path: /hello

#    Define function environment variables here
#    environment:
#      variable2: value2

# you can add CloudFormation resource templates here
#resources:
#  Resources:
#    NewResource:
#      Type: AWS::S3::Bucket
#      Properties:
#        BucketName: my-new-bucket
#  Outputs:
#     NewOutput:
#       Description: "Description for the output"
#       Value: "Some output value"

```

----

## Offline work

```bash
npm init
npm install serverless-offline --save-dev
```

```yaml
plugins:
  - serverless-offline
```


----

## Debugging

```bash
export SLS_DEBUG=* && node --inspect /usr/local/bin/serverless offline -s dev
```

In chrome: `chrome://inspect`


---

## Create a new handler function to add/update an endpoint

---

```js
// DB.js
exports.updateEndpoint = endpoint => {
    return new Promise((resolve, reject) => {
        console.log("endpoint", endpoint);
        resolve();
    })
};

// handler.js
const DB = require('./DB');

module.exports.updateEndpoint = async (event, context) => {
    context.callbackWaitsForEmptyEventLoop = false;

    const endpoint = JSON.parse(event.body);

    // Store in MongoDB
    await DB.updateEndpoint(endpoint);

    return {
        statusCode: 201,
        body: JSON.stringify(
            {
                message: 'ok',
            },
            null,
            2
        ),
    };
};

```
---
## Link the function in serverless.yaml

```yaml
...
functions:
  updateEndpoint:
    handler: handler.updateEndpoint
    events:
      - http:
          path: endpoint/update
          method: post
...
```


---
## Adding a DB (MongoDB)

Create a MongoDB account with [Atlas](https://cloud.mongodb.com/)

1. Register
2. Create a FREE `Starter Cluster`
3. Wait for the cluster to start (1-3 minutes)
4. Connect to cluster
   1. Whitelist your ip
   2. Create a user
   3. choose "Connect Your App"
   4. Copy connection url

---

## Create a Database and Collection

> Clusters -> Collections -> "add my own data"

Create a new DB and collection

```
database: 'Dashboard-<your-name>'
collection: 'endpoints'
```
---

save the collection string as an env variable

```yaml
// serverless.yaml
provider:
  name: aws
  runtime: nodejs12.x

# you can overwrite defaults here
  stage: fs-demo-assaf
  region: eu-central-1
  environment:
    MONGODB_URI: ${env:MONGODB_URI}

```

---

```
# Mac
export MONGODB_URI=mongodb+srv://dbadmin:<password>@cluster0-takpx.mongodb.net/test?retryWrites=true&w=majority

# Windows
set MONGODB_URI mongodb+srv://dbadmin:<password>@cluster0-takpx.mongodb.net/test?retryWrites=true&w=majority
```

---

## Connecting to the DB

```
npm i mongodb
```

```js
const MongoClient = require('mongodb').MongoClient;
const uri = process.env.MONGODB_URI; // Atlas connection string
const client = new MongoClient(uri, { useNewUrlParser: true });

exports.updateEndpoint = async endpoint => {
    return new Promise((resolve, reject) => {
        client.connect(err => {
            if (err) {
                reject(err);
            }
            const collection = client
                .db('dashboard-assaf')
                .collection('endpoints');

            collection.insertOne(endpoint, err => {
                if (err) {
                    console.log(err);
                    reject(err);
                }
                resolve();
                client.close();
            });
        });
    });
};

```
---

## Get endpoints function

Write a function that retrievs endpoints

hint: add to serverless.yaml
```yaml
functions:
  getEndpoints:
  handler: handlers.getEvents
  events:
    - http:
        path: endpoints
        method: get
        cors: true

```

---

## Deployment (to AWS)

1. Set up AWS credentials ([docs](https://serverless.com/framework/docs/providers/aws/guide/credentials/))
2. `$ serverless deploy`

