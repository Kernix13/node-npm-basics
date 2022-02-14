# Webpack config example

Notes on my [webpack.config.js file](https://github.com/Kernix13/travel-site/blob/master/webpack.config.js "Webpack config file") from my repo `travel-site`.

Lines 1-7 from the linked file above are just variables for some of the packages
```js
const currentTask = process.env.npm_lifecycle_event
const path = require('path')
const { CleanWebpackPlugin } = require('clean-webpack-plugin')
const MiniCssExtractPlugin = require('mini-css-extract-plugin')
const CssMinimizerPlugin = require('css-minimizer-webpack-plugin')
const HtmlWebpackPlugin = require('html-webpack-plugin')
const fse = require('fs-extra')
```

Lines 9-15 is a variable for PostCSS and most of the packages associated with it:
```js
const postCSSPlugins = [
  require("postcss-import"),
  require('postcss-mixins'),
  require("postcss-simple-vars"),
  require("postcss-nested"),
  require("autoprefixer")
]
```

Lines 17-23: This is from my notes and I do not fully understand it. I'll need to watch this lesson (63 – Preparing to go live pt 4) again. Looks like it is just copying the images.

> For the images. we only need to copy the images for our build task – for `config.plugins` (see below), have it so each plugin call is on its own line – add `new RunAfterComplie` – we will create that and it will be a new plugin for webpack. 

```js
class RunAfterCompile {
  apply(compiler) {
    compiler.hooks.done.tap('Copy images', function () {
      fse.copySync('./app/assets/images', './docs/assets/images')
    })
  }
}
```

Lines 25-28 is creating an object to find any `css` file and setting properties of `use` and `postcssOptions`. 

```js
let cssConfig = {
  test: /\.css$/i,
  use: ["css-loader?url=false", { loader: "postcss-loader", options: { postcssOptions: { plugins: postCSSPlugins } } }]
}
```
This is to  go in the `rules` array which is inside the `module` object. It gets 2 properties: 1) called `test` and it gets a RegEx, 2) then `use` which is an array for the packages with obejcts inside with properties of `loader` and `options` and `postcssOptions` which is an object that has a `plugins` property which calls the variable on line 9.

Lines 30-37 sets a variable called `pages` to an `fs-extra` method called `readdirSync` which returns an array of every file in the app folder: `filter` only returns html files, `map` will let us generate a new array based on the filter array. We don’t want just an array of file names, we want an array of multiple html webpack plugins – we want to use the webpack plugin once for each of out html templates. 

```js 
let pages = fse.readdirSync('./app').filter(function (file) {
  return file.endsWith('.html')
}).map(function (page) {
  return new HtmlWebpackPlugin({
    filename: page,
    template: `./app/${page}`
  })
})
```

Lines 39-57 is for a variable called `config`which is used for both `npm run dev` and `npm run build`. Any configuration that can be shared by dev and build will live in the var `config`. 

There is a lot going on here but it is using the main js file in the `entry` property, calling `pages` for the `plugins` property, then we have `module` > `rules` which has the `cssConfig` variable/object created on line 25. It laso has an object to test for JavaScript files, exclude `node_modules`, and use `babel-loader` and an `options` property which is an object with a single property of `presets` which is using the 2 babel presets from the `package.json` file (WHEW!)

```js
let config = {
  entry: './app/assets/scripts/App.js',
  plugins: pages,
  module: {
    rules: [
      cssConfig,
      {
        test: /\.js$/,
        exclude: /(node_modules)/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['@babel/preset-react', '@babel/preset-env']
          }
        }
      }
    ]
  }
}
```


Lines 59-75 and lines 77-95 are if statements to check for the `dev` and `build` scripts respectively. This is really nice. 

The `dev` code block calls the `config` object covered above via `config.output`, `config.devServer`, and `config.mode`. I assume output, devServer, and mode are methods but I'm not sure about that. (59 – preparing to go live part 1)

**dev**:

```js
if (currentTask == 'dev') {
  cssConfig.use.unshift('style-loader')
  config.output = {
    filename: 'bundled.js',
    path: path.resolve(__dirname, 'app')
  }
  config.devServer = {
    before: function (app, server) {
      server._watch('./app/**/*.html')
    },
    contentBase: path.join(__dirname, 'app'),
    hot: true,
    port: 3000,
    host: '0.0.0.0'
  }
  config.mode = 'development'
}
```

The `build` code block calls the `config` object via `config.output`, `config.mode`, `config.optimization`, and `config.plugins`. It also calls `cssConfig` and adds style-loader but I don't know how since the array is in `use.options.presets`.

`config.output` creates the hashed file names, `config.optimization` does something with the hash chunks and minimization, and `config.plugins` pushes the following: `clean-webpack-plugin`, `mini-css-extract-plugin`, and `RunAfterCompile`.

**build**:
```js
if (currentTask == 'build') {
  cssConfig.use.unshift(MiniCssExtractPlugin.loader)
  config.output = {
    filename: '[name].[chunkhash].js',
    chunkFilename: '[name].[chunkhash].js',
    path: path.resolve(__dirname, 'docs')
  }
  config.mode = 'production'
  config.optimization = {
    splitChunks: { chunks: 'all' },
    minimize: true,
    minimizer: [`...`, new CssMinimizerPlugin()]
  }
  config.plugins.push(
    new CleanWebpackPlugin(),
    new MiniCssExtractPlugin({ filename: 'styles.[chunkhash].css' }),
    new RunAfterCompile()
  )
}
```

Line 97: Why export it?

> Calling `module.exports` a configuration object and is no longer what we want this file to export – but don’t delete it, use as a ref because it works for our dev environment

```js 
module.exports = config
```

**Note**: I do not understand everything in this file.

## Important notes and questions

- Webpack bundles or packs together all of your files and dependencies so that they are easy to deliver to the visitors to your websites
- To start using webpack you need to install 2 packages: npm install webpack webpack-cli
- Create a new folder and it needs this exact name: `src`. It needs that exact name because that is where webpack by default will look for our source code. It will look for index.js in the src folder. 
- What is `npx`? 
- `npx webpack` creates a `dist` folder and a bundled file called `main.js` inside of it. No matter how many files we have or how many packages we import/require from npm, webpack bundles it all together in that 1 file that we can have our visitos download
- `run npx webpack` - what is this? Then `export default` and `import name from ...` 
- Use `npx webpack --watch` so you don’t have to keep running `npx webpack`
- **NOTE**: when you are writing client-side javascript like this, you do want to pay attention to the file size of your bundled file. Don’t obsess on the file size but keep it on your radar
  - Whenever you import a package that you are going to force the visitors of your site to download, be mindful of the file size – a better approach is if you know you are only going to certain features and not the whole package, them import just that
- `entry`: this is the entry into our application and for webpack to parse and bundle up the dependencies for
- `devServer`: { } it gets an object – need to tell it which port to run on 
- almost all professional development teams use some sort of CSS processing tool 
- `sass-loader` is a package that allows it to work with Webpack. The #1 reason he prefers SASS → MIXINS – specifically for media queries

## Second webpack.config file

This file is from his YouTube video called *webpack devServer, hot module replacement (live reload)*. Another good video of Brad's on Webpack is *React, Babel, Sass webPack*.

- `contentBase: path.resolve(__dirname, "dist"),`: which folder to use as the dev server preview
- to stop seeing the warning in the console about production mode, add a prop of `mode: "development"`
- "production" is for the live version of our website and will minify the file, and use short variable names, but it’s hard to debug while developing
- `hot: true` is to inject the js into the page making it even faster
- A more elegant way of calling our commands – `npm run dev` and `npm run build`

```js
const currentTask = process.env.npm_lifecycle_event
const path = require("path")
const MiniCssExtractPlugin = require("mini-css-extract-plugin")
const HtmlWebpackPlugin = require("html-webpack-plugin")
const { CleanWebpackPlugin } = require("clean-webpack-plugin")
const WebpackManifestPlugin = require("webpack-manifest-plugin")

const config = {
  entry: "./app/app.js",
  output: {
    filename: "myBundle.[hash].js",
    path: path.resolve(__dirname, "dist")
  },
  plugins: [new HtmlWebpackPlugin({ template: "./app/index.html" })],
  mode: "development",
  devtool: "eval-cheap-source-map",
  devServer: {
    port: 8080,
    contentBase: path.resolve(__dirname, "dist"),
    hot: true
  },
  module: {
    rules: [
      {
        test: /\.scss$/,
        use: ["style-loader", "css-loader", "sass-loader"]
      },
      {
        test: /\.js$/,
        exclude: /(node_modules|bower_components)/,
        use: {
          loader: "babel-loader",
          options: {
            presets: [["@babel/preset-env", { "useBuiltIns": "usage", "corejs": 3, "targets": "defaults" }], "@babel/preset-react"]
          }
        }
      }
    ]
  }
}

if (currentTask == "build") {
  config.mode = "production"
  config.module.rules[0].use[0] = MiniCssExtractPlugin.loader
  config.plugins.push(new MiniCssExtractPlugin({ filename: "main.[hash].css" }), new CleanWebpackPlugin(), new WebpackManifestPlugin())
}

module.exports = config
```

### Folder structure

| symbol   | Meaning                      |
| :------- | :--------------------------- |
| /      | = Root directory               |
| .      | = this location                |
| ..     | = up a directory               |
| ./     | = current directory            |
| ../    | = parent of current directory  |
| ../../ | = Two directories backwards    |

## Netlify notes

To set things up for Netlify we will set up our build process to be completely automated. You won’t need a `dist` or `docs` folder on your computer, just work with your `app` folder. And any time you push your changes up, Netlify will run our build process for us and generate the public folder on its own. 

**QUESTION**: Do you HAVE to set up your project with webpack and a build process to use it on Netlify?

It an alternative to traditional web hosting – they host our files and make out site available to the public. You have a few diff ways to create your first site with netlify:

1. Drag & drop a folder from your computer
1. Click New site from Git, choose that.

Then choose where your git repo lives – Authorize Netlify – choose all repos – click Install. Next – Pick a repository. Under Basic build settings, in the Build cmd field type npm run build. Also check Publish directory field then click Deploy site. You’ll see a randomly generated name. Then that message changes to a link - when you push changes to your repo, netlify detects the change and runs `npm run build`.

Becaause we deleted our docs folder, our github pages site will no longer work. Even though Netlify will do the builds for us, there may be a time in the future where we need to adjust our build process - you really only want your source code in the repo. So go into `gitignore`, add `docs/` below `node_modules/`. 

More notes about Functions on Netlify and Netlify > Deploys tab. 

## Another webpack file

I think this may be from Kevin Powell' video but I'm not positive of that:

```js
// this is just a practice file
const path = require('path');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

module.exports = {
  mode: 'development',
  context: path.resolve(__dirname, "assets"),
  output: {
    path: path.resolve(__dirname, 'assets/dist'),
    filename: 'main.bundle.js',
  },
  watch: true,
  plugins: [
    new MiniCssExtractPlugin()
  ],
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          MiniCssExtractPlugin.loader,
          {
            loader: 'css-loader',
            options: {
              importLoaders: 1
            }
          },
          {
            loader: 'postcss-loader',
            options: {
              ident: 'postcss',
              plugins: [
                require('tailwindcss'),
                require('autoprefixer')
              ]
            }
          }
        ]
      },
    ]
  },
};
```

And for what it is worth, here is code from Kevin's `postcss.config.css`:

```js
module.exports = {
  plugins: [
    require('postcss-import'),
    require('postcss-preset-env')({ stage: 1 }),
    require('cssnano'),
  ]
}
```