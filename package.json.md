# Packson.json example

> I WILL BE REDOING THIS FILE TO BE GENERIC AS OPPOSED TO SPECIFIC TO WEBPACK

## Package dot json links

- https://docs.npmjs.com/cli/v8/configuring-npm/package-json
- https://docs.npmjs.com/creating-a-package-json-file
- https://nodejs.org/en/knowledge/getting-started/npm/what-is-the-file-package-json/
- https://heynode.com/tutorial/what-packagejson/
- https://nodesource.com/blog/the-basics-of-package-json
- https://codesweetly.com/package-json-file-explained

## Package dot json scripts links

- https://docs.npmjs.com/cli/v8/using-npm/scripts
- https://www.knowledgehut.com/blog/web-development/package-json-scripts-node-js
- https://dev.to/paulasantamaria/mastering-npm-scripts-2chd
- https://deliciousbrains.com/npm-build-script/
- https://2ality.com/2022/08/npm-package-scripts.html

## From freeCodeCamp Back End APIS

### How to Use package json file

The package.json file is the center of any Node.js project or npm package. It stores information about your project, similar to how the `<head>` section of an HTML document describes the content of a webpage. It consists of a single JSON object where information is stored in key-value pairs. There are only two required fields; "name" and "version", but it’s good practice to provide additional information about your project that could be useful to future users or maintainers.

If you look at the file tree of your project, you will find the package.json file on the top level of the tree.

#### Author field

One of the most common pieces of information in this file is the author field. It specifies who created the project, and can consist of a string or an object with contact or other details. An object is recommended for bigger projects.

#### Description field

The next part of a good package.json file is the description field; where a short, but informative description about your project belongs.

If you some day plan to publish a package to npm, this is the string that should sell your idea to the user when they decide whether to install your package or not. However, that’s not the only use case for the description, it’s a great way to summarize what a project does. It’s just as important in any Node.js project to help other developers, future maintainers or even your future self understand the project quickly.

Regardless of what you plan for your project, a description is definitely recommended.

#### Keywords field

The keywords field is where you can describe your project using related keywords. Here's an example:

`"keywords": [ "descriptive", "related", "words" ],`

As you can see, this field is structured as an array of double-quoted strings.

#### License field

The license field is where you inform users of what they are allowed to do with your project.

Some common licenses for open source projects include MIT and BSD. License information is not required, and copyright laws in most countries will give you ownership of what you create by default. However, it’s always a good practice to explicitly state what users can and can’t do. Here's an example of the license field:

`"license": "MIT",`

### Version field

A version is one of the required fields of your package.json file. This field describes the current version of your project. Here's an example:

`"version": "1.2.0",`

In this section, packages your project requires are stored using the following format:

```json
"dependencies": {
  "package-name": "version",
  "express": "4.14.0"
}
```

### Manage npm Dependencies By Understanding Semantic Versioning

Versions of the npm packages in the dependencies section of your package.json file follow what’s called Semantic Versioning (SemVer), an industry standard for software versioning aiming to make it easier to manage dependencies. Libraries, frameworks or other tools published on npm should use SemVer in order to clearly communicate what kind of changes projects can expect if they update.

Knowing SemVer can be useful when you develop software that uses external dependencies...understanding of these numbers will save you from accidentally introducing breaking changes to your project without understanding why things that worked yesterday suddenly don’t work today. This is how Semantic Versioning works according to the official website:

`"package": "MAJOR.MINOR.PATCH"`

The MAJOR version should increment when you make incompatible API changes. The MINOR version should increment when you add functionality in a backwards-compatible manner. The PATCH version should increment when you make backwards-compatible bug fixes. This means that PATCHes are bug fixes and MINORs add new features but neither of them break what worked before. Finally, MAJORs add changes that won’t work with earlier versions.

```json
"dependencies": {
    "dependency1": "^major.minor.patch"
  },
```

### Use the Tilde-Character to Always Use the Latest Patch Version of a Dependency

To allow an npm dependency to update to the latest PATCH version, you can prefix the dependency’s version with the tilde (~) character

`"package": "~1.3.8"`

### Use the Caret-Character to Use the Latest Minor Version of a Dependency

If you were to use the caret (^) as a version prefix instead, npm would be allowed to update to any 1.x.x version. This would allow updates to any 1.x.x version of the package.

`"package": "^1.3.8"`

### Remove a package from Your Dependencies

what if you want to remove an external package that you no longer need...just remove the corresponding key-value pair for that package from your dependencies. This same method applies to removing other fields in your package.json as well.

## Boilerplate package json file

Here is an example of the boilerplate file when you run `npm init -y`:

```json
{
  "name": "npm-basics",
  "version": "1.0.0",
  "description": "Repo for Node.js, NPM, Webpack, and package.json notes",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/Kernix13/node-npm-basics.git"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "bugs": {
    "url": "https://github.com/Kernix13/node-npm-basics/issues"
  },
  "homepage": "https://github.com/Kernix13/node-npm-basics#readme"
}
```

Notes on my [package.json file](https://github.com/Kernix13/travel-site/blob/master/package.json 'package.json file') from my repo `travel-site`.

## Scripts object

Use `npm run dev` during development and `npm run build` when you want to go live and push to Github.

```json
"scripts": {
    "dev": "webpack serve",
    "build": "webpack",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
```

`"dev": "webpack serve"` is so you don't have to install webpack globally. `"build": "webpack",` creates the `dist` folder with the bundled file `main.js`.

## Dependencies object

By default, NPM simply installs a package under `node_modules`. When you're trying to install dependencies for your app/module, you would need to first install them, and then add them to the `dependencies` section of your `package.json` file.

However, adding `--save-dev` to the command `npm install package-name` adds the third-party package to the package's development dependencies. `--save` adds the third-party package to the package's dependencies. It will be installed together with the package whenever someone runs npm install package.

```json
"dependencies": {
    "axios": "^0.21.1",
    "lazysizes": "^5.3.0",
    "lodash": "^4.17.20",
    "normalize.css": "^8.0.1",
    "react": "^17.0.1",
    "react-dom": "^17.0.1"
  },
```

Description of the packages:

1. **axios**: We could use the browser’s _fetch_ functionality to send off an asynchronous request but instead use the 3rd party package called Axios. It’s cleaner and easier to work with than the _fetch api_. It makes sending off an asynchronous request easy.
1. **lazysizes**: It is a lazy loading library which does not negatively affect your SEO score with Google – the images are not hidden from the crawler.
1. **lodash**: Used to throttle the scroll call. It’s a package we want to bundle / send along to our visitors – an actual direct dependency of our application. Lodash makes JavaScript easier by taking the hassle out of working with arrays, numbers, objects, strings, etc: 1. Iterating arrays, objects, & strings, 2. Manipulating & testing values, 3. Creating composite functions
1. **normalize.css**: The way postcss handles imports. Add an import for it in the main style sheet. It is an alternative to css resets - use it on all of your projects. It preserves useful defaults and tries to normalize styles across browsers, instead of removing styles. If a browser does not adhere to the standard styles, Normalize aims to modify them to make styles consistent - only normalize needs the .css in the import statement in the main css file, none of the others do.
1. **react** and **react-dom**: react is the core of the library and react-dom is specifically for the web browser environment. A common package is `create-react-app`.
1. **react-dom**:

### Code snippet examples for the above packages

You do not need `--save-dev` for any of these packages because they are a direct dependency of our app that we want visitors to have to download.

**Axios**: use `npm install axios`:

```js
sendRequest() {
    Axios.post('https://gallant-hamilton-e0c06d.netlify.app/.netlify/functions/secret-area', { password: this.field.value }).then(response => {
      this.form.remove()
      this.contentArea.innerHTML = response.data
    }).catch(() => {
      this.contentArea.innerHTML = `<p class="client-area__error">That secret phrase is not correct. Try again.</p>`
      this.field.value = ''
      this.field.focus()
    })
  }
```

`sendRequest()` is a method we created where we will communicate with our cloud function. `Axios.post()` will result in a promise. `post()` gets 2 args: 1) the url you want to send a request to, 2) is an object (check postman for the property).

---

**Lazysizes**: just import it, you don’t need to name it, use `npm install lazysizes`:

```js
import 'lazysizes';
```

Add the following class to all `img` tags:

```html
<img
  class="lazyload"
  sizes="(min-width: 970px) 976px, 100vw"
  data-srcset="assets/images/first-trip-low-res.jpg 565w, assets/images/first-trip.jpg 976w, assets/images/first-trip-hi-dpi.jpg 1952w"
  alt="Couple walking down a street."
/>
```

---

**Lodash**: use `npm install lodash`:

```js
// just import throttle instead of entire library
import throttle from 'lodash/throttle';
import debounce from 'lodash/debounce';
```

---

**Normalize.css**: use `npm install normalize.css`:

```css
@import 'normalize.css';
```

---

**react**: use `npm install react react-dom`:

```js
import React from 'react';
import ReactDOM from 'react-dom';
```

---

**react-dom**: See above for the npm command:

```js
ReactDOM.render(
  <MyAmazingComponent />,
  document.querySelector('#my-react-example')
);
```

`render()` gets 2 arguments: 1) a component that you want to render to the page, 2) the element on the page you want to render to.

## Dev dependencies

These differ from the `dependencies` object by ... (not sure)

```json
{
  "devDependencies": {
    "@babel/core": "^7.12.13",
    "@babel/preset-env": "^7.12.13",
    "@babel/preset-react": "^7.12.13",
    "autoprefixer": "^10.2.4",
    "babel-loader": "^8.2.2",
    "clean-webpack-plugin": "^3.0.0",
    "css-loader": "^5.0.1",
    "css-minimizer-webpack-plugin": "^1.2.0",
    "fs-extra": "^9.1.0",
    "html-webpack-plugin": "^5.0.0",
    "mini-css-extract-plugin": "^1.3.5",
    "postcss-import": "^14.0.0",
    "postcss-loader": "^5.0.0",
    "postcss-mixins": "^7.0.2",
    "postcss-nested": "^5.0.3",
    "postcss-simple-vars": "^6.0.3",
    "style-loader": "^2.0.0",
    "webpack": "^5.20.1",
    "webpack-cli": "^4.5.0",
    "webpack-dev-server": "^3.11.2"
  }
}
```

Hee are the npm commands for all of those:

```js
npm install @babel/core @babel/preset-env babel-loader --save-dev
npm install @babel/preset-react --save-dev
npm install postcss-simple-vars postcss-nested autoprefixer --save-dev
npm install clean-webpack-plugin --save-dev
npm install css-loader style-loader --save-dev
npm install fs-extra --save-dev
npm install html-webpack-plugin --save-dev
npm install css-minimizer-webpack-plugin
npm install mini-css-extract-plugin --save-dev
npm i -D postcss-import
npm install postcss-import --save-dev
npm install postcss-loader --save-dev
npm install postcss-mixins --save-dev
pm install --save-dev webpack-cli
npm install webpack-dev-server --save-dev
```

They are added automatically when you use `npm install package-name`. Here is a breakdown of the packages and what they do:

1. **@babel/core**: not sure
1. **@babel/preset-env**: a preset that allows you to use the latest JavaScript without needing to micromanage which syntax transforms are needed by your target environment(s)
1. **@babel/preset-react**: Browsers do not understand `jsx`, but this tool is designed for `jsx`. USe the react babel tool for both your `dev` and `build` workflows.
1. **autoprefixer**: one of the most popular CSS processors, used to parse CSS and add vendor prefixes to CSS rules using values from Can I Use
1. **babel-loader**: allows transpiling JavaScript files using Babel and webpack. Use it to work with `jsx` files.
1. **clean-webpack-plugin**: For chunkhash...
1. **css-loader**: use if a file ends in `.css`. `css-loader` lets webpack understand/bundle css, `style-loader` actually uses/applies the css. Without this in out webpack.config file we would get an error after trying to import a css file.
1. **css-minimizer-webpack-plugin**: uses `cssnano` to optimize and minify your CSS
1. **fs-extra**: for multiple html pages, adds file system methods that aren't included in the native `fs` module
1. **html-webpack-plugin**: simplifies creation of HTML files to serve your bundles
1. **mini-css-extract-plugin**: extract the css out of the main bundle - if you make a change to your css users don’t have to download the entire bundle again
1. **postcss-import**: import is a native css feature but you don’t want the browser to have to download multiple css files – instead we want webpack and postcss to replace that line with the contents of the file. Wow in the main css file you can import your partial css files – you use `@import` but it is not a css import which slows things down
1. **postcss-loader**: the post css package for webpack, adds PostCSS support
1. **postcss-mixins**: configure our postcss setup to support mixins
1. **postcss-nested**: unwraps nested rules the way Sass does
1. **postcss-simple-vars**: plugin for Sass-like variables
1. **style-loader**: see `css-loader`
1. **webpack**: bundler
1. **webpack-cli**: CLI for webpack. Webpack CLI provides the interface of options webpack uses in its configuration file. The CLI options override options passed in the configuration file
1. **webpack-dev-server**: Updates the browser on changes, auto refresh with changes to the html and no refresh needed for css and js changes. Also to view our site on any device connected to the same wifi/network as the computer we are working on

Code snippets:

```js
// for clean-webpack-plugin
[chunkhash].js[chunkhash].css;
// postcss
require('postcss-import');
require('postcss-mixins');
```
