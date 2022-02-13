# Node.js and NPM basic commands

Node.js is a JavaScript environment with funcionality specific to Node.

1. Read files on out harddrive
1. create new files
1. It can make outgoing requests
1. And listen for incoming requests

## Basic Node.js commands

Check to see if `node` is installed on your local machine. If you see a version number then it is installed. The second command gets you into a node environment where you can run node commands:

```js
node -v
node
```
To exit note, do either of the following

```js
CTRL+C CTRL+C
.exit
```
Have node run a Javascript file (either version):

```js
node fileName.js
node fileName
```
Require in a file: in a separate file create a function. Then to export it use the 1st command; use the 2nd command to import it into another file

To create a file use:

```js
touch test.js
```

```js
module.exports = fxName;
const fxName = require('./fileName.js)
```
Importing modules that exist in the core of Node: e.g. File System (fs) https, assert, events, url, path, util, querystring

```js 
const fs = require(fs)
// to see entire list (on command line):
require('module').builtinModules
```
You can use the `fs` module to read the content in a file and modify it. For example, wrap text in an html tag and then export that into a newly created html file (each function requires 3 parameters)

```js 
fs.readFile('./content.txt', 'utf=8', function(err, data) {
  if(err) throw err
  fs.writeFile('./index.html', `<h1>${data}</h1>`, function(err) {
    if (err) throw err
    console.log("Successful)
  })
})
```

Also try `fs.createWriteStream`. Check out the [node documention](https://nodejs.org/dist/latest-v16.x/docs/api/ "Node docs") for more information on all the modules and their functions.

**NOTE**: In the eal world, 90+% of the time there is no need to manunally reasearch & use these core components of node. Instead you would use a community created package.

## Common NPM packages and commands

Common packages: `webpack`, `express`, `mongodb`, `postcss`, `react-dom`, `lodash`, `normalize.css`, etc.

To create a `package.json` file  run the following once when you start a project:

```js
npm init -y
// to install node_modules:
npm install
```

When you fork and clone a repo from say Github, it will **not** have a `node_modules` folder. But as long as it has a package.json file with the dependencies just run `npm install` to add the modules folder to your local project.

To install a package like `validator` and require it in a js file run:
```js
npm install validator
const validator = require('validator')
```
Where `-y` means **yes** to any questions node may ask. The `install` command creates a `node_modules` folder in your project. An example of avalidator function:

```js
validator.isEmail('something-here')
```

## Miscellaneaous commands:

I'll flesh these out later with notes but all these packages and commands are from the project I build from Brad Schiff's Udemy course called ***Git a Web Developer Job: Mastering the Modern Workflow***.

Package installs:

```js
npm install @babel/core 
npm install babel-loader
npm install @babel/preset-react
npm install @babel/preset-env
npm install react react-dom
npm install lodash
npm install normalize.css
npm install lodash
npm install lazysizes
npm install fs-extra --save-dev
npm install @babel/core @babel/preset-env babel-loader --save-dev
npm install axios
npm install @babel/preset-react --save-dev
npm install postcss-simple-vars postcss-nested autoprefixer --save-dev
npm install css-loader style-loader --save-dev
npm install postcss-loader --save-dev
npm i -D postcss postcss-cli
npm i -D postcss-import
npm run postcss:watch --watch
```

Webpack commands:

```js
create webpack.config.js
npm install webpack webpack-cli  --save-dev
npm install webpack-dev-server --save-dev
npm install html-webpack-plugin --save-dev
npm run build
npm run dev
watch: true
--save-dev
'css-loader?url=false'
"postcss:watch": "postcss src/style.css --dir public --watch"
```

Miscellaneous commands:

```js
__dirname + "/index.html"
pipe()
module watch

```