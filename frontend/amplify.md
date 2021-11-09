# Configure AWS Amplify

In this section, I will guild you to setup the API request using Amplify API

## Configuration API Gateway with Cognito authentcation

We modify the file `awsConfig.js` to config API work with Cognito for OAuth 2.0

```javascript
import { Auth } from 'aws-amplify'

const config = {
  Auth: {
    region: REACT_APP_REGION_AWS,
    identityPoolRegion: REACT_APP_REGION_AWS,
    userPoolId: REACT_APP_USER_POOL_ID_AWS,
    identityPoolId: REACT_APP_IDENTITY_POOL_ID_AWS,
    userPoolWebClientId: REACT_APP_USER_POOL_WEB_CLIENT_ID_AWS,
    mandatorySignIn: true,
    storage: localStorage,
  },
  API: {
    endpoints: [
      {
        name: 'API',
        endpoint: REACT_APP_API_ENDPOINT_AWS,
        region: REACT_APP_API_REGION_AWS,
        custom_header: async () => ({
          Authorization: `Bearer ${(await Auth.currentSession()).getIdToken().getJwtToken()}`,
        }),
      },
    ],
  },
}

export default config

```

So, the secret point is `custom_header`. We add the custom header `Authorization` with `JwtToken`

## Implement the API service model

After add authentication config to API, we can write API request following

### GET Request sample

```javascript
export async function fileList(params) {
  return API.get('API', '/file', {
    queryStringParameters: params,
  })
}
```

> Please remember using correct API Gateway name `API` that we used when created the API Gateway service

### DELETE Request sample

```javascript
export async function deleteFile(params) {
  return API.del('API', '/file', {
    queryStringParameters: params,
  })
}
```

### POST Request sample

```javascript
export async function downloadFile(params) {
  return API.post('API', '/download', {
    body: params,
  })
}
```

## CORS problem

If you meet the CORS problem with API Gateway, please read again the following document:

1. [Setup CORS via manual way](../manual/cors.md)
2. [Setup CORS via CDK](../cdk/cors.md)