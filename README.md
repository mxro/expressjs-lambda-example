# Express.js AWS Lambda Sample Project

This sample project shows how an Express.js server can be deployed to AWS lambda. It contains the following features:

- Example Express.js server packaged up for deployment to AWS Lambda
- Monorepo setup using Yarn 2
- Ready for usage in VSCode
- TypeScript configured
- ESLint and Prettier configured
- All infrastructure defined in Terraform
- Configurable through JSON files

This project is deployed to [https://expressjs-lambda.examples.goldstack.party/](https://expressjs-lambda.examples.goldstack.party/).

For more information also see the blog post [Express.js on Lambda Getting Started](https://maxrohde.com/2021/02/21/express-js-on-lambda-getting-started/).

## Getting Started

To get started, checkout the project and then run in the project root:

```
yarn
```

The change to the directory for the lambda-express module and built it:

```
cd packages/lambda-express
yarn build:pack 
```

You can also run the API locally by running `yarn watch`.

To modify the project to deploy to your API, you need to configure:

### config/infra/aws/config.json

Provide a file as follows with AWS credentials (user should have an administrator policy, see [step by step instructions](https://docs.goldstack.party/docs/goldstack/configuration#how-to-get-aws-credentials-1)):

```json
{
  "users": [
    {
      "name": "awsUser",
      "type": "apiKey",
      "config": {
        "awsAccessKeyId": "your secret",
        "awsSecretAccessKey": "your access key",
        "awsDefaultRegion": "us-west-2"
      }
    }
  ]
}
```

### packages/lambda-express/goldstack.json

Modify this file to change it to the domain where you want to deploy your API:

```json
{
  "$schema": "./schemas/package.schema.json",
  "name": "lambda-express",
  "template": "lambda-express",
  "templateVersion": "0.1.19",
  "configuration": {},
  "deployments": [
    {
      "name": "prod",
      "configuration": {
        "lambdaName": "expressjs-lambda-getting-started",
        "apiDomain": "expressjs-lambda.examples.goldstack.party",
        "hostedZoneDomain": "goldstack.party"
      },
      "awsUser": "awsUser",
      "awsRegion": "us-west-2"
    }
  ]
}
```

## Deploy to AWS

Once you have updated the configuration files mentioned above, you can go into the `packages/lambda-express` folder and run:

```bash
# Setting up infrastructure
yarn infra up prod

# Deploy lambda
yarn deploy prod
```

Note that in order for these commands to work, you need to have [Docker](https://www.docker.com/) installed on your machine.

## Generate Starter Project

Rather than configuring the project manually as above, you can also use [Goldstack](https://goldstack.party) to generate a starter project for you. This can be configured using a visual step-by-step editor and will create a ZIP file for you, you can use to start your project.

This project has been generated using the Goldstack [Express.js + Lambda template](https://goldstack.party/templates/express-lambda). Further documentation for this template is a available [here]()