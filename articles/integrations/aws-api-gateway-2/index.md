---
description: blah blah
---

# Secure AWS API Gateway Endpoints Using Custom Authorizers

With AWS, you can create powerful, serverless, highly scalable APIs and applications using [Lambda](https://aws.amazon.com/lambda/), [API Gateway](https://aws.amazon.com/api-gateway/), and a JavaScript client for the front-end.

A serverless application runs custom code as a compute service without the need to maintain an operating environment to host your service. Instead, a service like [AWS Lambda](https://aws.amazon.com/lambda/) or [webtask.io](https://webtask.io) executes your code on your behalf.

The API Gateway extends the capabilities of Lambda by adding a service layer in front of your Lambda functions to extend security, manage input and output message transformations, and provide capabilities like throttling and auditing. A serverless approach simplifies your operational demands, since concerns like scaling out and fault tolerance are now the responsibility of the compute service that is executing your code.

This tutorial will show you how to set up your API with API Gateway, create and configure your Lambda functions (including the custom authorizers) to secure your API endpoints, and implement the authorization flow so that your users can retrieve the access tokens needed to gain access to your API from Auth0.

More specifically, the custom authorizers will:

1. Confirm that the OAuth 2.0 token has been passed via the `authorization` header of the request to access the API
2. Verify the RS256 signature of the OAuth 2.0 token using a public key obtained via a JWKS endpoint
3. Ensure the token has the required Issuer `iss` and Audience `aud` claims

To that end, this tutorial will be divided into the following sections.

* [Step 1 - Set up the AWS API Gateway](/integrations/aws-api-gateway/part-1)
* [Step 2 - Secure and Deploy the Amazon API Gateway](/integrations/aws-api-gateway/part-2)
* [Step 3 - Build the Client Application](/integrations/aws-api-gateway/part-3)

## How API Gateway Custom Authorizers Work

[According to Amazon](http://docs.aws.amazon.com/apigateway/latest/developerguide/use-custom-authorizer.html), an API Gateway custom authorizer is a "Lambda function you provide to control access to your API using bearer token authentication strategies, such as OAuth or SAML."

Whenever someone (or some program) attempts to call your API, API Gateway checks to see if there's a custom authorizer configured for the API.

If **there is a custom authorizer for the API**, API Gateway calls the custom authorizer and provides the authorization token extracted from the request header received.

You can use the custom authorizer to implement different types of authorization strategies, including [JWT](/jwt) verification, to return IAM policies authorizing the request. If the policy returned is invalid or if the permissions are denied, the API call fails.

For a valid policy, API caches the returned policy, associating it with the incoming token and using it for the current and subsequent requests. You can configure the amount of time for which the policy is cached. The default value is 300 seconds, and the maximum length of caching is 3600 seconds (you can also set the value to 0 to disable caching).

## Before You Begin

Before beginning this tutorial, you'll need to [sign up for an AWS account](https://portal.aws.amazon.com/gp/aws/developer/registration/index.html). This grants you access to all of the AWS features we'll use in this tutorial, including API Gateway and Lambda. All new members receive twelve months of free tier access to AWS.

### Keep Reading

* [Get Ready to Use Amazon API Gateway](http://docs.aws.amazon.com/apigateway/latest/developerguide/setting-up.html)

* [Build an API to Expose a Lambda Function](http://docs.aws.amazon.com/apigateway/latest/developerguide/getting-started.html)

* [Creating an API in Amazon API Gateway](http://docs.aws.amazon.com/apigateway/latest/developerguide/how-to-create-api.html)

* [Use API Gateway Custom Authorizers](http://docs.aws.amazon.com/apigateway/latest/developerguide/use-custom-authorizer.html)

* [Secure API Gateway Endpoints with Custom Authorizers](https://cloudacademy.com/amazon-web-services/labs/api-gateway-custom-authorizers-lambda-54/)

<%= include('./_stepnav', {
 next: ["1. Part 1", "/integrations/aws-api-gateway-2/part-1"]
}) %>