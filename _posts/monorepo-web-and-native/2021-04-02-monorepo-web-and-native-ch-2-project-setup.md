---
date: 2021-04-02 13:00:00
tags: react reactnative typescript
serie: monorepo-web-and-native
title: Monorepo R+RN, Ch 2
display-title: Ch 2 - Project Setup
---

<p>So the first thing we’ll do, quite counterintuitively, is to deploy our application. Well, “Why deploy an empty application which doesn’t exist yet?“ you might ask. Why the heck not?</p>
<!--more-->
<ul>
    <li>It allows us to setup our continuous integration pipeline from the start.</li>
    <li>Also makes our application accessible to others for early feedback.</li>
</ul>

<p>We will need three things to get started.</p>
A source code repository hosting service. → Bitbucket
<br/>A URL endpoint from any CDN provider. → AWS S3
<br/>And the CI/CD solution of your choosing. → CircleCI
<br/>
<br/>

<u class="faded-highlight">Repo setup</u><br/><br/>
Let’s create a repository, my glorious project shall be called “monorepo-client”. Boring and nerdy, yet hits the spot. We can spice it up later when the project becomes a hit. Renaming repos is always fun.

Now that we have our remote repo setup, let’s clone it to our local, we will later connect this repo to CircleCI for CI/CD.

{% highlight console %}
$ git clone git@bitbucket.org:monorepo-template/monorepo-client-root.git
{% endhighlight %}

{% highlight console %}
$ cd monorepo-client-root
{% endhighlight %}

{% highlight console %}
$ npm init
{% endhighlight %}

<p>After we navigate to our cloned folder and run “npm init” we will be guided in creating a minimal package.json file that will look like this.</p>
<u class="faded-highlight no-underline">&lt;root>/package.json</u>

{% highlight json %}
    {
        "name": "monorepo-client-root",
        "version": "1.0.0",
        "description": "",
        "main": "index.js",
        "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1"
        },
        "repository": {
        "type": "git",
        "url": "git+ssh://git@bitbucket.org/monorepo-template/monorepo-client-root.git"
        },
        "author": "",
        "license": "ISC",
        "homepage": "https://bitbucket.org/monorepo-template/monorepo-client-root#readme"
    }
{% endhighlight %}
<br/>
<u class="faded-highlight">React app setup with Typescript Babel and Eslint</u><br/>
<p>We also create a directory for our React application, we will call this folder “web-client”. After you navigate to this folder, once again run “npm init”, this one we will configure with the scripts and dependencies for our web application.</p>

{% highlight console %}
$ mkdir web-client && cd web-client && npm init
{% endhighlight %}

<p>The next steps will be following the amazing tutorial by Carl Rippon, <a href="https://www.carlrippon.com/creating-react-app-with-typescript-eslint-with-webpack5/" target="_blank">“Creating a React app with TypeScript and ESLint with Webpack 5”</a>. The explanations he provides are so educational, I suggest you take the time to read it in full. If you are already comfortable with these concepts, the rest of our configuration till the deployment section are the steps highlighted in his post.</p>

<p>Install react, react-dom and typescript for our project.</p>

{% highlight console %}
$ npm i react react-dom
{% endhighlight %}

{% highlight console %}
$ npm i --save-dev typescript
{% endhighlight %}

{% highlight console %}
$ npm i --save-dev @types/react @types/react-dom
{% endhighlight %}

<p>Init the tsconfig.json file using the following command</p>

{% highlight console %}
$ npx tsc --init
{% endhighlight %}

<p>Edit the tsconfig.json to include the following.</p>

<u class="faded-highlight no-underline">&lt;root>/web-client/tsconfig.json</u>
{% highlight json linenos%}
   "compilerOptions": {
   "target": "es5",
   "module": "commonjs",                
   "lib": ["DOM","DOM.Iterable","ESNext"],
   "allowJs": true,          
   "jsx": "react",
   "isolatedModules": true,
   "resolveJsonModule": true,
   "strict": true,
   "moduleResolution": "node",
    "allowSyntheticDefaultImports": true,    
   "esModuleInterop": true,
    "skipLibCheck": true,                          
   "forceConsistentCasingInFileNames": true
}
{% endhighlight %}

<p>Create a src folder within “web-client” and add our entry point there with index.html.</p>

<u class="faded-highlight no-underline">&lt;root>/web-client/src/index.html</u>
{% highlight html linenos%}
<!DOCTYPE html>
<html>
    <head>
    <meta charset="utf-8" />
    <title>Template React App</title>
    </head>
<body>
    <div id="root"></div>
</body>
</html>
{% endhighlight %}

<p>Create index.tsx in the same folder with the following content.</p>

<u class="faded-highlight no-underline">&lt;root>/web-client/src/index.tsx</u>
{% highlight react linenos%}
import React from "react";
import ReactDOM from "react-dom";
‍
const App = () => (
 <h1>Testing out automated deployments.</h1>
);
‍
ReactDOM.render(
 <React.StrictMode>
   <App />
 </React.StrictMode>,
 document.getElementById("root")
);
{% endhighlight %}

<p>We need Babel to turn our Typescript code to Javascript so we can deploy our React application. Let’s install it.</p>

{% highlight console %}
$ npm i --save-dev @babel/core @babel/preset-env @babel/preset-react @babel/preset-typescript @babel/plugin-transform-runtime @babel/runtime
{% endhighlight %}

<p>Create .babelrc to configure Babel.</p>

<u class="faded-highlight no-underline">&lt;root>/web-client/.babelrc</u>
{% highlight json linenos%}
{
   "presets": [
     "@babel/preset-env",
     "@babel/preset-react",
     "@babel/preset-typescript"
   ],
   "plugins": [
     [
       "@babel/plugin-transform-runtime",
       {
         "regenerator": true
       }
     ]
   ]
 }
{% endhighlight %}

<p>Now that we configured Babel and Typescript, we might just as well add linting to our project. Let’s install and configure ESLint.</p>

{% highlight console %}
$ npm i --save-dev eslint eslint-plugin-react eslint-plugin-react-hooks @typescript-eslint/parser @typescript-eslint/eslint-plugin
{% endhighlight %}

<p>Create .eslintrc.json in the web-client directory.</p>

<u class="faded-highlight no-underline">&lt;root>/web-client/.eslintrc.json</u>
{% highlight json linenos%}
{
   "parser": "@typescript-eslint/parser",
   "parserOptions": {
     "ecmaVersion": 2018,
     "sourceType": "module"
   },
   "plugins": [
     "@typescript-eslint",
     "react-hooks"
   ],
   "extends": [
     "plugin:react/recommended",
     "plugin:@typescript-eslint/recommended"
   ],
   "rules": {
     "react-hooks/rules-of-hooks": "error",
     "react-hooks/exhaustive-deps": "warn",
     "react/prop-types": "off"
   },
   "settings": {
     "react": {
       "pragma": "React",
       "version": "detect"
     }
   }
 }
{% endhighlight %}


<br/>
<u class="faded-highlight">Webpack setup</u><br/>
<p>Adding Webpack</p>

{% highlight console %}
   $ npm i --save-dev webpack webpack-cli @types/webpack
{% endhighlight %}

{% highlight console %}
   $ npm install --save-dev webpack-dev-server @types/webpack-dev-server
{% endhighlight %}

{% highlight console %}
   $ npm install --save-dev babel-loader
{% endhighlight %}

{% highlight console %}
   $ npm install --save-dev html-webpack-plugin
{% endhighlight %}


<p>To be able to use .ts extension with webpack configuration files, install ts-node</p>

{% highlight console %}
   $ npm install --save-dev ts-node
{% endhighlight %}
Let’s also add typechecking and linting capabilities to webpack.
{% highlight console %}
    $ npm install --save-dev fork-ts-checker-webpack-plugin @types/fork-ts-checker-webpack-plugin
{% endhighlight %}
{% highlight console %}
    $ npm install --save-dev eslint-webpack-plugin
{% endhighlight %}

<p>Now configure webpack.dev.config.ts for development environment</p>
<u class="faded-highlight no-underline">&lt;root>/web-client/webpack.dev.config.ts</u>
{% highlight typescript linenos%}
import path from "path";
import webpack from "webpack";
import * as webpackDevServer from 'webpack-dev-server';
import HtmlWebpackPlugin from "html-webpack-plugin";
import ESLintPlugin from "eslint-webpack-plugin";
import ForkTsCheckerWebpackPlugin from 'fork-ts-checker-webpack-plugin';
‍
const config: webpack.Configuration = {
 mode: "development",
 output: {
   publicPath: "/",
 },
 entry: "./src/index.tsx",
 module: {
   rules: [
     {
       test: /\.(ts|js)x?$/i,
       exclude: /node_modules/,
       use: {
         loader: "babel-loader",
         options: {
           presets: [
             "@babel/preset-env",
             "@babel/preset-react",
             "@babel/preset-typescript",
           ],
         },
       },
     },
   ],
 },
 resolve: {
   extensions: [".tsx", ".ts", ".js"],
 },
 plugins: [
   new HtmlWebpackPlugin({
     template: "src/index.html",
   }),
   new webpack.HotModuleReplacementPlugin(),
   new ForkTsCheckerWebpackPlugin({
     async: false
   }),
   new ESLintPlugin({
     extensions: ["js", "jsx", "ts", "tsx"],
   }),
 ],
 devtool: "inline-source-map",
 devServer: {
   contentBase: path.join(__dirname, "build"),
   historyApiFallback: true,
   port: 4000,
   open: true,
   hot: true
 },
};

export default config;
{% endhighlight %}

<p>Configuring for production with slight differences.</p>
<u class="faded-highlight no-underline">&lt;root>/web-client/webpack.prod.config.ts</u>
{% highlight typescript linenos%}
import path from "path";
import webpack from "webpack";
import HtmlWebpackPlugin from "html-webpack-plugin";
import ForkTsCheckerWebpackPlugin from "fork-ts-checker-webpack-plugin";
import ESLintPlugin from "eslint-webpack-plugin";
import { CleanWebpackPlugin } from "clean-webpack-plugin";

const config: webpack.Configuration = {
 mode: "production",
 entry: "./src/index.tsx",
 output: {
   path: path.resolve(__dirname, "build"),
   filename: "[name].[contenthash].js",
   publicPath: "",
 },
 module: {
   rules: [
     {
       test: /\.(ts|js)x?$/i,
       exclude: /node_modules/,
       use: {
         loader: "babel-loader",
         options: {
           presets: [
             "@babel/preset-env",
             "@babel/preset-react",
             "@babel/preset-typescript",
           ],
         },
       },
     },
   ],
 },
 resolve: {
   extensions: [".tsx", ".ts", ".js"],
 },
 plugins: [
   new HtmlWebpackPlugin({
     template: "src/index.html",
   }),
   new ForkTsCheckerWebpackPlugin({
     async: false,
   }),
   new ESLintPlugin({
     extensions: ["js", "jsx", "ts", "tsx"],
   }),
   new CleanWebpackPlugin(),
 ],
};

export default config;
{% endhighlight %}

<p>CleanWebpackPlugin plugin will help us clear out the build folder at the start of the bundling.</p>
{% highlight console %}
    $ npm install --save-dev clean-webpack-plugin
{% endhighlight %}

<p>Add the below scripts to your package.json</p>
<u class="faded-highlight no-underline">&lt;root>/web-client/package.json</u>
{% highlight json linenos%}
    ...
  "scripts": {
    "start": "webpack serve --config webpack.dev.config.ts",
    "build": "webpack --config webpack.prod.config.ts",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
{% endhighlight %}

<p>Now that we configured our template app, we are ready to deploy.</p>