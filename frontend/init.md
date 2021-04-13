# Initialize the Web application

In that section, I will guide you to setup simple ReactJS application and integrate with AWS Amplify

## Create a New React App with UmiJS

You can use create-umi via `yarn create umi` or `npm create umi`. Recommend to use `yarn create`, to make sure use the latest boilerplate everytime

```shell
$ mkdir sample-amplify-app && cd sample-amplify-app
$ yarn create umi
```

Choose project

```shell
? Select the boilerplate type (Use arrow keys)
  ant-design-pro  - Create project with an layout-only ant-design-pro boilerplate, use together with umi block.
❯ app             - Create project with a simple boilerplate, support typescript.
  block           - Create a umi block.
  library         - Create a library with umi.
  plugin          - Create a umi plugin.
```

Confirm if you want to use TypeScript.

```shell
? Do you want to use typescript? (y/N)
```

Then, select the function you need, checkout plugin/umi-plugin-react for the detailed description.

```shell
? What functionality do you want to enable? (Press <space> to select, <a> to toggle all, <i> to invert selection)
❯◯ antd
 ◯ dva
 ◯ code splitting
 ◯ dll
```

Then install the dependencies manually

```shell
$ yarn
```

Finally, start local development server with yarn start

```shell
$ yarn start
```


This should fire up the newly created app in your browser

![text](https://gw.alipayobjects.com/zos/rmsportal/YIFycZRnWWeXBGnSoFoT.png)

## Change the Umi config

Create the folder `config` and the file `config.js` with content:

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
  ]
}
```