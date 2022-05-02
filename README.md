# Node.js and NPM basic commands

Node.js is a JavaScript environment with funcionality specific to Node.

1. Read files on hard drive
1. Create new files
1. It can make outgoing requests...
1. ...and listen for incoming requests

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

To create a file use:
```js
touch test.js
```
Require in a file: in a separate file create a function. Then to export it use the 1st command; use the 2nd command to import it into another file

Export example:
```js
module.exports = fxName;
const fxName = require('./fileName.js)
```
Importing modules that exist in the core of Node: e.g. File System (fs) https, assert, events, url, path, util, querystring, ...

```js 
const fs = require(fs)
// to see entire list (on command line):
require('module').builtinModules
```
You can use the `fs` module to read the content in a file and modify it. For example, wrap text in an HTML tag and then export that into a newly created `*.html` file (each function requires 3 parameters)

```js 
readFile(a, b, c) 
writeFile(a, b, c) 
// example
fs.readFile('./content.txt', 'utf=8', function(err, data) {
  if(err) throw err
  fs.writeFile('./index.html', `<h1>${data}</h1>`, function(err) {
    if (err) throw err
    console.log("Successful")
  })
})
```
For `writeFile()`, `a` = location where you want to create the file so you can do `__dirname` for current folder: a = `__dirname + “/index.html”`.

Check out the [node documention](https://nodejs.org/dist/latest-v16.x/docs/api/ "Node docs") for more information on all the modules and their functions.

**NOTE**: In the real world, 95% of the time there is no need to manually reasearch & use these core components of node. Instead you would use a community created package.

***Use node to download an image from the web***: in order to send out a request onto the internet we'll require another package – `https` if your url has the `s` otherwise `http`: `get(a,b)` 

Where `a` = the url and `b` = a function which gets the `pipe()` function and the `createWriteStream` method:

```js
var myPhotoLocation = "https://raw.githubusercontent.com/LearnWebCode/welcome-to-git/master/images/dog.jpg";
https.get(myPhotoLocation, function (response) {
  response.pipe(fs.createWriteStream(__dirname + "/mydog.jpg"));
});
```

Also: 
```js
tap()
fs.copySync()
```

## Common NPM packages and commands

Common packages you wll see: `webpack`, `express`, `mongodb`, `postcss`, `react-dom`, `lodash`, `normalize.css`, etc.

To create a `package.json` file  run the following once when you start a project:

```js
npm init -y
// to install node_modules:
npm install package-name
<!--  -->to restore the node_modules file after deleting it:
npm init
```

Where `-y` means **yes** to any questions node may ask. The `install` command creates a `node_modules` folder in your project. 

When you fork and clone a repo from say Github, it will **not** have a `node_modules` folder. But as long as it has a package.json file with the dependencies just run `npm install` to add the modules folder to your local project.

To install a package like `validator` and require it in a js file run:
```js
npm install validator
const validator = require('validator')
```
An example of a validator function:

```js
validator.isEmail('something-here')
```

## Miscellaneaous commands:

I'll flesh these out later with notes but all these packages and commands are from the project I build from Brad Schiff's Udemy course called ***Git a Web Developer Job: Mastering the Modern Workflow***.

## NPM Package installs:

**PostCSS**: 
```js
npm install postcss-simple-vars postcss-nested autoprefixer --save-dev
npm install css-loader style-loader --save-dev
npm install postcss-loader --save-dev
npm i -D postcss postcss-cli
npm i -D postcss-import
npm run postcss:watch --watch
```

**Babel & React**:
```js
npm install react react-dom
npm install babel-loader
npm install @babel/preset-react --save-dev
npm install @babel/core @babel/preset-env babel-loader --save-dev
```

**Miscellaneous**:
```js
npm install lodash
npm install normalize.css
npm install lazysizes
// to search for any html file:
npm install fs-extra --save-dev
npm install axios
```

## Webpack and webpack.config.js commands:

Out of the box without any config, webpack only understands js files. To start using webpack you need to install 2 packages:

```js
npm install webpack webpack-cli  --save-dev
```
Then you need to create a config file in your root directory called `webpack.config.js`. In it you tell webpack what you want it to do – you only need one property: `entry`. Then type the path to the file you want to bundle. 

To tell webpack how to process and bundle the file(s), you need to include `module.exports =` then the obejct `{…}`. 

Now go into `package.json` and look for a property called `scripts`. You can create an **npm script** which you can run from the command line:

```js
"scripts": {
    "dev": "webpack serve",
    "build": "webpack",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
// To run that, on command line use:
npm run dev
// or 
npm run build
```

I don't think `serve` is supposed to be part of the `dev` value.

`npm run build` creates a `dist` folder and a new file called `main.js`. To change the file name (`main.js`) or for the bundled file to go into a different folder other than `dist`, use: 

```js
output = {
    filename: 'bundled.js',
    path: path.resolve(__dirname, 'app')
  }
```

Webpack requires an absolute path for the output, so you need to **require** in the node.js `path` package: `npm install path`. Then delete the `dist` folder, make a change in your js file then run `npm run build`. To have your file update automatically add:

```js
mode = watch: true
```
See [webpack.config.js.md](https://github.com/Kernix13/node-npm-basics/blob/master/webpack.config.js.md) for detailed notes for that file, and [package.json.md](https://github.com/Kernix13/node-npm-basics/blob/master/package.json.md) for notes on that file.
