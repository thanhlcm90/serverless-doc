# CORS with CDK

## Understanding CORS

There are two things we need to do to support CORS in our Serverless API.

1. Preflight OPTIONS requests

2. Respond with CORS headers

There’s a bit more to CORS than what we have covered here. So make sure to check out the [Wikipedia article for further details](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing).

If we don’t set the above up, then we’ll see something like this in our HTTP responses.

`No 'Access-Control-Allow-Origin' header is present on the requested resource`

And our browser won’t show us the HTTP response. This can make debugging our API extremely hard.

## Setup CORS in CDK

Please check my code in previous section [Configure Lambda and API Gateway in CDK](api.md)

```javascript
// defines an API Gateway REST API resource backed by our "hello" function.
const restApi = new apigw.LambdaRestApi(this, "LambdaRestApi", {
    restApiName: "API",
    handler: welcomeFunc,
    proxy: false,
    defaultCorsPreflightOptions: {
        allowOrigins: apigw.Cors.ALL_ORIGINS,
        allowMethods: apigw.Cors.ALL_METHODS,
        allowCredentials: true,
    },
});

// config CORS for 4xx error
new apigw.CfnGatewayResponse(this, "Default4XXResponse", {
    responseType: "DEFAULT_4XX",
    restApiId: restApi.restApiId, // reference to RestApi created earlier
    responseParameters: {
    "gatewayresponse.header.Access-Control-Allow-Origin": "'*'",
    },
});

// config CORS for 5xx error
new apigw.CfnGatewayResponse(this, "Default5XXResponse", {
    responseType: "DEFAULT_5XX",
    restApiId: restApi.restApiId, // reference to RestApi created earlier
    responseParameters: {
    "gatewayresponse.header.Access-Control-Allow-Origin": "'*'",
    },
});
```

In above code, we setup CORS in two place:

1. Setup CORS for `LambdaRestApi` via option `defaultCorsPreflightOptions` . We set default `allowCredentials: true` to allow credentials, `allowOrigins: apigw.Cors.ALL_ORIGINS` to allow all origins (*), and `allowMethods: apigw.Cors.ALL_METHODS` to allow all method (*)
2. Setup CORS for error response 4xx and 5xxx. If you don't do that, we will meet the issues the call API with incorrect or expired token.
