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

Lines 9-15 is a variable for PostCSS and all the packages associated with it:
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

Temp notes: back into webpack.config, and in the module > rules > use we want to add another loader – but instead of adding it’s name like the other 2, we need to add an oblect > it gets a few options: 1. loader, holy shit on the next one – around 15:30

Lines 25-28 is creating an object to find any `css` file and setting properties of `use` and `postcssOptions`. 

```js
let cssConfig = {
  test: /\.css$/i,
  use: ["css-loader?url=false", { loader: "postcss-loader", options: { postcssOptions: { plugins: postCSSPlugins } } }]
}
```
This is to  go in the `rules` array which is inside the `module` object. It gets 2 properties: 1) called `test` and it gets a RegEx, 2) then `use` which is an array for the packages with obejcts inside with properties of `loader` and `options` and `postcssOptions` which is an object that has a `plugins` property which calls the variable on line 9.

Lines 30-37 sets a variable called `pages` to an fe-extra method called `readdirSync` which returns an array of every file in the app folder: `filter` only returns html files, `map` will let us generate a new array based on the filter array. We don’t want just an array of file names, we want an array of multiple html webpack plugins – we want to use the webpack plugin once for each of out html templates. 

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