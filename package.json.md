# Packason.json example

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
Notes on my [package.json file](https://github.com/Kernix13/travel-site/blob/master/package.json "package.json file") from my repo `travel-site`.

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

1. **axios**: We could use the browser’s *fetch* functionality to send off an asynchronous request but instead use the 3rd party package called Axios. It’s cleaner and easier to work with than the *fetch api*. It makes sending off an asynchronous request easy.
1. **lazysizes**: You do not need to include `--save-dev` at the end because it’s not a workflow package. It is a lazy loading library which does not negatively affect your SEO score with Google – the images are not hidden from the crawler. 
1. **lodash**: Used to throttle the scroll call. We do not need the `--save-dev` part because lodash is not a package that makes our workflow or dev process easier, it’s a package we want to bundle / send along to our visitors – an actual direct dependency of our application. Lodash makes JavaScript easier by taking the hassle out of working with arrays, numbers, objects, strings, etc:  1. Iterating arrays, objects, & strings, 2. Manipulating & testing values, 3. Creating composite functions
1. **normalize.css**: The way postcss handles imports. Add an import for it in the main style sheet. It is an alternative to css resets - use it on all of your projects. It preserves useful defaults and tries to normalize styles across browsers, instead of removing styles. If a browser does not adhere to the standard styles, Normalize aims to modify them to make styles consistent - only normalize needs the .css in the import statement in the main css file, none of the others do.
1. **react**: 
1. **react-dom**: 

###  Code snippet examples for the above packages

Axios: use `npm install axios`: 

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
`sendRequest()` is a method we created where we will communicate with our cloud function. `Axios.post()` will result in a promise. `post()` gets 2 args: 1) the url you want to send a request to,  2) is an object (check postman for the property).

Lazysizes: just import it, you don’t need to name it, use `npm install lazysizes`:

```js
import 'lazysizes'
```

Add the following class to all `img` tags:

```html
<img class="lazyload" sizes="(min-width: 970px) 976px, 100vw" data-srcset="assets/images/first-trip-low-res.jpg 565w, assets/images/first-trip.jpg 976w, assets/images/first-trip-hi-dpi.jpg 1952w" alt="Couple walking down a street.">
```

Lodash: use `npm install lodash`:

```js
// just import throttle instead of entire library
import throttle from 'lodash/throttle'
import debounce from 'lodash/debounce'
```

Normalize.css: use 'npm install normalize.css`:

```css
@import 'normalize.css';
```

## Dev dependencies

These differ from the `dependencies` object by ...

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

They are added automatically when you use `npm install package-name`. Here is a breakdown of the packages and what they do:

1. **@babel/core**: 
1. **@babel/preset-env**: 
1. **@babel/preset-react**: 
1. **babel-loader**: 
1. **autoprefixer**: 
1. **babel-loader**: 
1. **clean-webpack-plugin**: 
1. **css-loader**: 
1. **css-minimizer-webpack-plugin**: 
1. **fs-extra**: 
1. **html-webpack-plugin**: 
1. **mini-css-extract-plugin**: 
1. **postcss-import**: 
1. **postcss-loader**: 
1. **postcss-mixins**: 
1. **postcss-nested**: 
1. **postcss-simple-vars**: 
1. **style-loader**: 
1. **webpack**: 
1. **webpack-cli**: 
1. **webpack-dev-server**: 