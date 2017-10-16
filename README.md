# HOW TO
# webpack
&nbsp;
### [Webpack 2: The Complete Developer's Guide](https://www.udemy.com/webpack-2-the-complete-developers-guide)


&nbsp;
## 01 Start a project

* Create the project directory.

```
$ mkdir js_modules
$ cd js_modules
```

* Start npm. Enter default values for the package.json.

```
$ npm init
```
**package.json**
```
{
  "name": "js_modules",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}
```

* Add the *src* folder and the *index.js* and *sum.js* files.

**index.js: requires sum.js and calls it with the numbers 10 and 5**

```
const sum = require('./sum');

const total = sum(10,5);
console.log(total);
```

**sum.js: adds a and b**

```
const sum = (a,b) => a+b;

module.exports = sum;
```



&nbsp;
## 02 Add webpack

* Add webpack.

```
$ npm install --save-dev webpack
```

The current webpack version is added in the dev dependencies of the project in *package.json*.
```
"devDependencies": {
    "webpack": "^3.7.1"
  }
```


* Add the webpack configuration file *webpack.config.js*.

Choose *build* as the output folder and *bundle.js* as the name of the output file.

Node provides the *path* module and the *resolve* method.

**webpack.config.js**
```
const path = require('path');

const config = {
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'build'),
    filename: 'bundle.js'
  }
};

module.exports =  config;
```



* Add the *index.html* file.


**index.html**
```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title></title>
  </head>
  <body>
    <h3>Number 15 should be displayed in the browser's console.</h3>
    <script src="build/bundle.js"></script>
  </body>
</html>
```

* Add the `"build": "webpack"` line in the *scripts* section of *package.json*.

**package.json**
```
{
  "name": "js_modules",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "build": "webpack",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^3.7.1"
  }
}
```

* Now we can run the build script.

```
$ npm run build
```
If we open *index.html* in a browser, we can now see the result *15* in the console output of the browser.



&nbsp;
## 03 Add babel

* Install the babel loaders.

```
$ npm install --save-dev babel-loader babel-core babel-preset-env

```

* In *webpack.config.js*, add the *module* property which contains the *rules* array. The loader rules dictate that the *babel-loader* gets loaded and applied to files ending with the *.js* extension.

**webpack.config.js**
```
const path = require('path');

const config = {
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'build'),
    filename: 'bundle.js'
  },
  module: {
    rules: [
      {
        use: 'babel-loader',
        test: /\.js$/
      }
    ]
  }
};

module.exports =  config;
```

* Add the *.babelrc* file and define the babel-presets.

**.babelrc**
```
{
  "presets" : ["babel-preset-env"]
}
```

* Build again.
```
$ npm run build
```



The *bundle.js* file previously contained
```
/***/ (function(module, exports) {

const sum = (a,b) => a+b;
```
Now it contains
```
var sum = function sum(a, b) {
  return a + b;
};
```
proving that babel has been applied successfully.
