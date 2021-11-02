# How to setup a project with webpack + react + Eslint & prettier

In this tutorial i will guide you through the steps that will brings to the final result
by explaining the every step in order to obtain this

INSERT MP4 HERE

## initial setup

first let create a new folder

```sh
mkdir MyProject
cd MyProject
```

Now let's initialize the project with npm that allows us to manage our dependencies
```sh
npm init -y
```
use -y if you want to skip the wizard and to use default values that can be changes inside package.json

Let's now create our github repo so that we can push it later to github by running:
```sh
git init
```
let's start creating some folders, the structure is up to you but i suggest to organize things in this way:

```sh
mkdir src
mkdir assets
mkdir dist
```

- **src** is where all the codebase will be places
- **assets** is where you can place images, fonts etc
- **dist** is where the project will build and where the final bundle will be placed


Now you are ready to install all the needed dependencies

## initial webpack setup

Using *webpack* gives the power to 

let's start by installing in our dev dependencies **webpack** and **webpack-cli** and **webpack-dev-server** plus **html-webpack-plugin**

```sh
npm install -D webpack webpack-cli webpack-dev-server html-webpack-plugin babel-loader style-loader css-loader
```

[webpack-dev-server](https://www.npmjs.com/package/webpack-dev-server) is used for running your project in watch mode, and for every change in the codebase included in webpack your project will rebuild and reload automatically in the browser. This will save your mental sanity and boost your humor, since you do not have to hit "compile" every single change you do

let's also create the webpack config that will be used for development purposes

```sh
touch webpack-config.js
```

let's also create a `main.jsx` and an `index.html` inside **src** folder
```sh
cd src
touch main.jsx
touch index.html
cd ..
```

in the `index.html` put
```html
<!DOCTYPE html>

<head>
  <title> test </title>
</head>
<html>

<body>

  <div id="root">

  </div>

</body>

</html>
```

while in the main.jsx put 
```jsx
import React from "react";
import ReactDOM from "react-dom";


const App = () => {
  return <div> ChronosOutOfTime thanks so much</div>;
};

ReactDOM.render(<App />, document.querySelector("#root"));
```

now we just need to configure our webpack to run it when we run **npm start**

so in the webpack.config.js put:
```js
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  mode: "development",
  entry: "./src/main.jsx",
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "bundle.js",
    clean: true,
  },
  plugins: [
    new HtmlWebpackPlugin({
      title: "My Awesome Custom Project",
      template: "src/index.html",
    }),
  ],
  resolve: {
    extensions: [".js", ".jsx"],
  },
  module: {
    rules: [
      {
        test: /\.css$/i,
        use: ["style-loader", "css-loader"],
      },
      {
        test: /\.jsx?$/,
        loader: "babel-loader",
        exclude: /node_modules/,
        include: [path.join(__dirname, "src")],
      },
    ],
  },
  devServer: {
    hot: true,
    client: {
      progress: true,
    },
  },
};
```
## Install our preferred framework libs or other vanilla js deps (React)

In my case i chose React since it's widely used, but you can use different frameworks / libs, the purpose of this tutorial is the same.

```sh
npm install react react-dom lodash
```

And since we are using react we want to take advantage of writing out components with the **[jsx](https://reactjs.org/docs/introducing-jsx.html)** format hence we need to **[babelize](https://babeljs.io/docs/en/#jsx-and-react)** our code. 

let's install babel deps

```sh
npm install -D @babel/core @babel/preset-env @babel/preset-react babel-eslint babel-loader
```

and let's create our .babelrc

```sh
touch .babelrc
```

where we will put the following
```json
{
  "presets": [
    "@babel/preset-env",
    "@babel/preset-react"
  ]
}
```
## ESlint 

let's prepare eslint configuration and needed deps

```sh
npm install -D eslint eslint-config-prettier eslint-loader  eslint-plugin-prettier prettier
```

i also suggest if you are using vs code to install the extension [error lens](https://marketplace.visualstudio.com/items?itemName=usernamehw.errorlens)

let's create our eslint config

```sh
touch .eslintrc.js
```

and copy the following code:
```js
module.exports = {
  env: {
    commonjs: true,
    node: true,
    browser: true,
    es6: true,
    jest: true,
  },
  parserOptions: {
    extends: ["react:recommended", "eslint:recommended", "prettier"],
    parser: "@babel/eslint-parser",
    sourceType: "module",
    ecmaVersion: 2020,
    ecmaFeatures: {
      jsx: true,
    },
  },
  plugins: ["prettier"],
  // add your custom rules here
  rules: {
    "prettier/prettier": [
      "error",
      {
        endOfLine: "auto",
      },
    ],
  },
};
```

## but, Why prettier + eslint?

The reason is that [prettier](https://prettier.io/docs/en/why-prettier.html) does very well one thing, formatting your code.
While [ESlint](https://eslint.org/) does many things, it is great for checking your syntax of your code and to prevent errors

Let me share [this blog](https://blog.theodo.com/2019/08/why-you-should-use-eslint-prettier-and-editorconfig-together/) where i found the inspiration for this video

Now let's configure a settings for vs code that will be absolutely the cherry on the pie:

- open vs code settings (json) with CTRL + P
- add the following:
```json
"editor.codeActionsOnSave": {
  "source.fixAll.eslint": true
},
```

This tiny option is what allows you to get the magic

## Conclusions

So this is it, this project can be used with any framework you like, some changes may be needed in the .eslintrc.js and .babelrc

I hoped i helped anybody with this little demo and if you liked this you can subscribe:
- on my [youtube channel](https://www.youtube.com/channel/UCOQOna7UFhT2skDitsDXaLA/videos)
- on my [discord channel](https://discord.com/channels/760625029930156042/760625029930156045)