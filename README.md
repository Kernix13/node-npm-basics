# Node.js and NPM basic commands

This is a repo for basics on Node.js and NPM, as well as related subjects like using Webpack.

Node.js is a JavaScript environment with funcionality specific to Node.

1. Read files on hard drive
1. Create new files
1. It can make outgoing requests...
1. ...and listen for incoming requests

<div id="back-to-top"></div>

## Table of contents

- [Basic Nodejs commands](#basic-nodejs-commands)
- [Updating Node and npm](#updating-node-and-npm)
- [Common NPM packages and commands](#common-npm-packages-and-commands)
- [NPM Package installs](#npm-package-installs)
- [Webpack and webpack config commands](#webpack-and-webpack-config-commands)
- [npm install vs npm ci](#npm-install-vs-npm-ci)

## Basic Nodejs commands

Check to see if `node` is installed on your local machine. If you see a version number then it is installed. The second command gets you into a node environment where you can run node commands:

```js
node - v;
node;
```

**To update npm** to the latest version, run the following in the root directory:

```npm
npm i -g npm@8
```

To update Node.js check out the Usage section in the [nvm repo](https://github.com/nvm-sh/nvm#usage) or the [Node releases page](https://nodejs.org/en/blog/release/). I used the Node update wizard and that worked.

To install all the dependencies for a project run the following. This step will automatically read and process the `package.json` file:

```node
npm i
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
const fs = require(fs);
// to see entire list (on command line):
require('module').builtinModules;
```

You can use the `fs` module to read the content in a file and modify it. For example, wrap text in an HTML tag and then export that into a newly created `*.html` file (each function requires 3 parameters)

```js
readFile(a, b, c);
writeFile(a, b, c);
// example
fs.readFile('./content.txt', 'utf=8', function (err, data) {
  if (err) throw err;
  fs.writeFile('./index.html', `<h1>${data}</h1>`, function (err) {
    if (err) throw err;
    console.log('Successful');
  });
});
```

For `writeFile()`, `a` = location where you want to create the file so you can do `__dirname` for current folder: a = `__dirname + “/index.html”`.

Check out the [node documention](https://nodejs.org/dist/latest-v16.x/docs/api/ 'Node docs') for more information on all the modules and their functions.

**NOTE**: In the real world, 95% of the time there is no need to manually reasearch & use these core components of node. Instead you would use a community created package.

**_Use node to download an image from the web_**: in order to send out a request onto the internet we'll require another package – `https` if your url has the `s` otherwise `http`: `get(a,b)`

Where `a` = the url and `b` = a function which gets the `pipe()` function and the `createWriteStream` method:

```js
var myPhotoLocation =
  'https://raw.githubusercontent.com/LearnWebCode/welcome-to-git/master/images/dog.jpg';
https.get(myPhotoLocation, function (response) {
  response.pipe(fs.createWriteStream(__dirname + '/mydog.jpg'));
});
```

Also:

```js
tap();
fs.copySync();
```

<div align="right">&#8673; <a href="#back-to-top" title="Table of Contents">Back to Top</a></div>

## Updating Node and npm

> I am still uncertain of the correct way to update node but here are the notes I have for now.

To update npm to the latest version, run the following in the root directory:

```sh
npm i -g npm@8
```

To update Node.js check out the Usage section in the nvm repo or the Node releases page. I used the Node update wizard and that worked.

To install all the dependencies for a project run the following. This step will automatically read and process the package.json file:

```sh
npm i
```

Here are some links on the topic:

- [Update Node.js to Latest Version](https://phoenixnap.com/kb/update-node-js-version)
- [Update Node.js to the Latest Version in 2022](https://www.hostingadvice.com/how-to/update-node-js-latest-version/#windows)
- [How to Update NodeJS Version on Windows](https://codeforgeek.com/update-nodejs-version-windows-linux-macos/)
- [How to Easily Update Node.js](https://www.mend.io/free-developer-tools/blog/how-to-update-node-js-to-latest-version/#2_Updating_using_a_Node_version_manager_on_Windows)

<div align="right">&#8673; <a href="#back-to-top" title="Table of Contents">Back to Top</a></div>

## Common NPM packages and commands

Common packages you wll see: `webpack`, `express`, `mongodb`, `postcss`, `react-dom`, `lodash`, `normalize.css`, etc.

To create a `package.json` file run the following once when you start a project:

```js
npm init -y
// to install node_modules:
npm install package-name
// to restore the node_modules file after deleting it:
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
validator.isEmail('something-here');
```

<div align="right">&#8673; <a href="#back-to-top" title="Table of Contents">Back to Top</a></div>

## NPM Package installs

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

<div align="right">&#8673; <a href="#back-to-top" title="Table of Contents">Back to Top</a></div>

## Webpack and webpack config commands

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
  path: path.resolve(__dirname, 'app'),
};
```

Webpack requires an absolute path for the output, so you need to **require** in the node.js `path` package: `npm install path`. Then delete the `dist` folder, make a change in your js file then run `npm run build`. To have your file update automatically add:

```js
mode = watch: true
```

See [webpack.config.js.md](https://github.com/Kernix13/node-npm-basics/blob/master/webpack.config.js.md) for detailed notes for that file, and [package.json.md](https://github.com/Kernix13/node-npm-basics/blob/master/package.json.md) for notes on that file.

## npm install vs npm ci

From a stackoverflow post [What is the difference between "npm install" and "npm ci"?](https://stackoverflow.com/questions/52499617/what-is-the-difference-between-npm-install-and-npm-ci):

Use `npm install` to add new dependencies, and to update dependencies on a project. Usually, you would use it during development after pulling changes that update the list of dependencies but it may be a good idea to use `npm ci` in this case.

Use `npm ci` if you need a deterministic, repeatable build. For example during continuous integration, automated jobs, etc. and when installing dependencies for the first time, instead of `npm install`.

<ins>npm install</ins>

`npm install` is great for development and in the CI when you want to cache the `node_modules` directory. When to use this? You can do this if you are making a package for other people to use.

- Installs a package and all its dependencies.
- Dependencies are driven by `npm-shrinkwrap.json` and `package-lock.json` (in that order).
- without arguments: installs dependencies of a local module.
- Can install global packages.
- Will install any missing dependencies in node_modules.
- It may write to `package.json` or `package-lock.json`.
  - When used with an argument (`npm i packagename`) it may write to `package.json` to add or update the dependency.
  - when used without arguments, (`npm i`) it may write to `package-lock.json` to lock down the version of some dependencies if they are not already in this file.

<ins>npm ci</ins>

`npm ci` should be used when you are to test and release a production application (a final product, not to be used by other packages) since it is important that you have the installation be as deterministic as possible, this install will take longer but will ultimately make your application more reliable.

- Requires at least npm v5.7.1.
- Requires `package-lock.json` or `npm-shrinkwrap.json` to be present.
- Throws an error if dependencies from these two files don't match `package.json`.
- Removes `node_modules` and install all dependencies at once.
- It never writes to `package.json` or `package-lock.json`.
