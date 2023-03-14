# Week 3 — Decentralized Authentication

## COGNITO

Install AWS Amplify in frontend folder, with the following command

```
npm i aws-amplify --save

```

### Create a Cognito users pool in the AWS Console

- Located in Amazon Cognito
- Create a news user pool
- Use default Cognito User pool
- For signin options use user name and email
- next
- Password policies leave as default
- For Multi-factor Authentication choose no MFA, 
- User account recovery leave as default, self service recovery
- Delivery method of account recovery use email, other options may incur cost
- Configure sign up experience leave default, self service sign up
- Allow cognito to verify
- use email
- Required attributes
  -name 
  -user name
- Configure message delivery check send email with cognito
- Intergrate your app
- User pool name - cruddur-user-pool
- dont use cognito hosted ui
- app type select public client
- app client name - cruddur
- review and create user pool

### CONFIGURE AMPLIFY

Go into your Frontend-react-js >> src >> App.js

Paste at the end of the import codes the following

```
import { Amplify } from 'aws-amplify';

```

After the imports paste the code:

```
Amplify.configure({
  "AWS_PROJECT_REGION": process.env.REACT_AWS_PROJECT_REGION,
  "aws_cognito_region": process.env.REACT_APP_AWS_COGNITO_REGION,
  "aws_user_pools_id": process.env.REACT_APP_AWS_USER_POOLS_ID,
  "aws_user_pools_web_client_id": process.env.REACT_APP_CLIENT_ID,
  "oauth": {},
  Auth: {
    // We are not using an Identity Pool
    // identityPoolId: process.env.REACT_APP_IDENTITY_POOL_ID, // REQUIRED - Amazon Cognito Identity Pool ID
    region: process.env.REACT_AWS_PROJECT_REGION,           // REQUIRED - Amazon Cognito Region
    userPoolId: process.env.REACT_APP_AWS_USER_POOLS_ID,         // OPTIONAL - Amazon Cognito User Pool ID
    userPoolWebClientId: process.env.REACT_APP_AWS_USER_POOLS_WEB_CLIENT_ID,   // OPTIONAL - Amazon Cognito Web Client ID (26-char alphanumeric string)
  }
});

```

In the docker-compose.yml file add the code at the frontend-react-js enviroment variables
-user pool and client ID are found in the AWS Conginto Console


```
REACT_AWS_PROJECT_REGION: "${AWS_DEFAULT_REGION}"
REACT_APP_AWS_COGNITO_REGION:"${AWS_DEFAULT_REGION}"
REACT_APP_AWS_USER_POOLS_ID:"us-east-1_8UnSKONjw"
REACT_APP_CLIENT_ID:"35a1pqum0qrq9liej8kloq3ufb"

```


