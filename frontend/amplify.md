# Configure AWS Amplify

In this section we are going to allow our users to login for our app. To do this we are going to start connecting the AWS resources that we created in the backend section.

To do this we’ll be using a library called [AWS Amplify](https://github.com/aws/aws-amplify). AWS Amplify provides a few simple modules (Auth, API, and Storage) to help us easily connect to our backend.

## Install AWS Amplify

Run the following command in your working directory

```shell
$ yarn add aws-amplify
```

## Configuration

Let’s first create a configuration file for our app that’ll reference all the resources we have created

Add the file `awsConfig.js` into your folder `src`

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
        name: 'Datacom POC API',
        endpoint: REACT_APP_API_ENDPOINT_AWS,
        region: REACT_APP_API_REGION_AWS
      },
    ],
  },
}

export default config

```

So, we need some `global varible` when run umi build. Please modify the umi config `config/config.js` following

```javascript
//ref: https://umijs.org/config/
export default {
  history: 'hash',
  sass: {},
  theme: 'src/theme.js',
  hash: true,
  plugins: [
    // ref: https://umijs.org/plugin/umi-plugin-react.html
    [
      'umi-plugin-react',
      {
        locale: {
          enable: true,
          default: 'ja-JP',
          baseNavigator: true,
          antd: true,
        },
        antd: true,
        dva: {
          hmr: true,
        },
        dynamicImport: {
          loadingComponent: 'components/LayoutComponents/Loader',
        },
        dll: {
          include: ['dva', 'dva/router', 'dva/saga', 'dva/fetch', 'antd/es'],
        },
        pwa: {
          manifestOptions: {
            srcPath: '.manifest.json',
          },
        },
      },
    ],
  ],
  define: {
    REACT_APP_REGION_AWS: 'ap-southeast-1',
    REACT_APP_USER_POOL_ID_AWS: 'ap-southeast-1_RivuYdwTo',
    REACT_APP_USER_POOL_WEB_CLIENT_ID_AWS: '45d1h4bciqlk6d3uu0vilhhfjq',
    REACT_APP_IDENTITY_POOL_ID_AWS: 'ap-southeast-1:52b58632-deba-4f82-b648-21ed0f9bec1a',
    REACT_APP_API_ENDPOINT_AWS: 'https://fjburztudh.execute-api.ap-southeast-1.amazonaws.com/prod',
    REACT_APP_API_REGION_AWS: 'ap-southeast-1',
  },
}
```

We need some variables that will be printed when deploy by CDK in the previous [Deploy Your Serverless Infrastructure document](../cdk/deploy.md)


```shell
DataComPocStackDev: deploying...
[0%] start: Publishing 2a7da9c0078815cc29a576511f6dac1953c243cc0a415edd8c86ed56943000c1:current
[50%] success: Published 2a7da9c0078815cc29a576511f6dac1953c243cc0a415edd8c86ed56943000c1:current
[50%] start: Publishing 577a52b9113e34cf42bf89d042a0e9b36d3417c592e6953fd14de3f860f50079:current
[100%] success: Published 577a52b9113e34cf42bf89d042a0e9b36d3417c592e6953fd14de3f860f50079:current
DataComPocStackDev: creating CloudFormation changeset...
[█████████████████████████████·····························] (15/30)



 ✅  DataComPocStackDev

Outputs:
DataComPocStackDev.ApidataComPOCLambdaRestApiEndpoint3D8B1823 = https://fjburztudh.execute-api.ap-southeast-1.amazonaws.com/prod/
DataComPocStackDev.CognitoAuthenticatedRoleName0DCC2D8C = DataComPocStackDev-CognitoCognitoAuthRoleCognitoDe-9IG7Y0L57KOA
DataComPocStackDev.CognitoIdentityPoolId42D6FEAB = ap-southeast-1:52b58632-deba-4f82-b648-21ed0f9bec1a
DataComPocStackDev.CognitoUserPoolClientId2F6CFE90 = 45d1h4bciqlk6d3uu0vilhhfjq
DataComPocStackDev.CognitoUserPoolId622CD4B2 = ap-southeast-1_RivuYdwTo
DataComPocStackDev.DynamoTableArn478698E3 = arn:aws:dynamodb:ap-southeast-1:964867551044:table/DataComPOCTable
DataComPocStackDev.DynamoTableName4C29EEE6 = DataComPOCTable
DataComPocStackDev.S3AttachmentsBucketNameC3FD403A = datacompocstackdev-s3uploads570493ed-7g8l8a76eviy

Stack ARN:
arn:aws:cloudformation:ap-southeast-1:964867551044:stack/DataComPocStackDev/f7fb4a40-983d-11eb-8d7e-06850023b7d0
```

You’ll notice that:

1. `REACT_APP_REGION_AWS` , `REACT_APP_API_REGION_AWS` is region that you deploy your stack

2. `REACT_APP_USER_POOL_ID_AWS` is equal `CognitoUserPoolId622CD4B2`

3. `REACT_APP_USER_POOL_WEB_CLIENT_ID_AWS` is equal `CognitoUserPoolClientId2F6CFE90`

4. `REACT_APP_IDENTITY_POOL_ID_AWS` is equal `CognitoUserPoolId622CD4B2`

5. `REACT_APP_API_ENDPOINT_AWS` is equal `ApidataComPOCLambdaRestApiEndpoint3D8B1823`

## Add AWS Amplify

Import it by adding the following to the header of your `src/index.js`

```javascript
import { Amplify } from 'aws-amplify';
import config from './awsConfig';

Amplify.configure(config)
```

A couple of notes here.

- Amplify refers to Cognito as `Auth` and API Gateway as `API`
- The `mandatorySignIn` flag for `Auth` is set to true because we want our users to be signed in before they can interact with our app
- The `name: "Datacom POC API"` is basically telling Amplify that we want to name our API. Amplify allows you to add multiple APIs that your app is going to work with. In our case our entire backend is just one single API.
- The `Amplify.configure()` is just setting the various AWS resources that we want to interact with. It isn’t doing anything else special here beside configuration. So while this might look intimidating, just remember this is only setting things up.

## Login with Cognito

The login code itself is relatively simple.

```javascript
import { Auth } from "aws-amplify";

export async function login(email, password) {
    try {
        await Auth.signIn(email, password);
        alert("Logged in");
    } catch (e) {
        alert(e.message);
    }
}
```

## Load user session

```javascript
import { Auth } from "aws-amplify";

export async function currentAccount() {
  try {
    const user = await Auth.currentUserInfo()
    return user
  } catch (e) {
    console.error(e)
    return null
  }
}
```

We implement the Redux Store to store auth session state for whole application

```javascript
import { Auth } from "aws-amplify";
import config from './awsConfig'

export async function currentAccount() {
  try {
    const user = await Auth.currentUserInfo()
    return user
  } catch (e) {
    console.error(e)
    return null
  }
}

export default {
  namespace: 'user',
  state: {
    id: '',
    email: '',
    username: '',
    authorized: false,
  },
  reducers: {
    SET_STATE: (state, { payload }) => ({ ...state, ...payload })
  },
  effects: {
    *LOAD_CURRENT_ACCOUNT(_, saga) {
      const response = yield saga.call(currentAccount)
      if (response) {
        yield saga.put({
          type: 'SET_STATE',
          payload: {
            id: response.id,
            email: response.attributes.email,
            username: response.attributes.email,
            authorized: true,
          },
        })
      }
    },
  },

  subscriptions: {
    setup: ({ dispatch }) => {
      Amplify.configure(config)
      dispatch({
        type: 'LOAD_CURRENT_ACCOUNT',
      })
    },
  },
}
```

## Logout with Cognito

Our `logout` function should now look like this.

```javascript
import { Auth } from "aws-amplify";

export async function logout() {
  await Auth.signOut()
}
```