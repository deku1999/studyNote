webpack依赖node环境，vuecli依赖webpack。

# webpack

## 简介

### 1.1webpack是什么

webpack是一种前端资源构建工具，一个静态模块打包器(module bundler)。在webpack看来，前端的所有资源文件(js/json/css/img/less/..)都会作为模块处理。它将根据模块的依赖关系进行静态分析，打包生成对应的静态文件。

### 1.2webpack五个核心概念

**Entry**

入口：指示 webpack 以哪个文件为入口起点开始打包，分析构建内部依赖图。

**Output**

输出(Output)：指示 webpack 打包后的资源 bundles 输出到哪里去，以及如何命名。

**Loader**

Loader：让 webpack 能够去处理那些非 JS 的文件，比如样式文件、图片文件(因为webpack 自身只理解JS，json)

**Plugins**

插件(Plugins)：可以用于执行范围更广的任务。插件的范围包括，从打包优化和压缩，
一直到重新定义环境中的变量等。

**Mode**

模式(Mode)：指示 webpack 使用相应模式的配置。

| 选项        | 描述                                                         | 特点                       |
| ----------- | ------------------------------------------------------------ | -------------------------- |
| development | 会将 DefinePlugin 中 process.env.NODE_ENV 的值设置为 development。启用 NamedChunksPlugin 和 NamedModulesPlugin。 | 能让代码本地调试运行的环境 |
| production  | 会将 DefinePlugin 中 process.env.NODE_ENV 的值设置为 production。启用 FlagDependencyUsagePlugin, FlagIncludedChunksPlugin, ModuleConcatenationPlugin, NoEmitOnErrorsPlugin, OccurrenceOrderPlugin, SideEffectsFlagPlugin 和 TerserPlugin。 | 能让代码优化上线运行的环境 |

## webpack初体验

### 初始化配置

1.初始化package.json：npm init

2.下载安装webpack：（webpack4以上的版本需要全局/本地都安装webpack-cli)

```
npm install webpack webpack-cli -g //全局
npm install webpack webpack-cli -D //局部，开发环境
```

### 编译打包应用

创建 src 下的 js 等文件后，不需要配置 webpack.config.js 文件，在命令行就可以编译打包。（因为只有js文件）

指令：

- 开发环境：`webpack ./src/index.js -o ./build/built.js --mode=development`

  webpack会以 ./src/index.js 为入口文件开始打包，打包后输出到 ./build/built.js 整体打包环境，是开发环境

- 生产环境：`webpack ./src/index.js -o ./build/built.js --mode=production`

  webpack会以 ./src/index.js 为入口文件开始打包，打包后输出到 ./build/built.js 整体打包环境，是生产环境

结论：

1. webpack 本身能处理 js/json 资源，不能处理 css/img 等其他资源
2. 生产环境和开发环境将 ES6 模块化编译成浏览器能识别的模块化，但是不能处理 ES6 的基本语法转化为 ES5（需要借助 loader）
3. 生产环境比开发环境多一个压缩 js 代码

## 开发环境基本配置

webpack.config.js 是 webpack 的配置文件。

作用: 指示 webpack 干哪些活（当你运行 webpack 指令时，会加载里面的配置）

所有构建工具都是基于 nodejs 平台运行的，模块化默认采用 commonjs。

开发环境配置主要是为了能让代码运行。主要考虑以下几个方面：

- 打包样式资源
- 打包 html 资源
- 打包图片资源
- 打包其他资源
- devServer

下面是一个简单的开发环境webpack.confg.js配置文件

```javascript
// resolve用来拼接绝对路径的方法
const { resolve } = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin') // 引用plugin

module.exports = {
  // webpack配置
  entry: './src/js/index.js', // 入口起点
  output: {
    // 输出
    // 输出文件名
    filename: 'js/build.js',
    // __dirname是nodejs的变量，代表当前文件的目录绝对路径
    path: resolve(__dirname, 'build'), // 输出路径，所有资源打包都会输出到这个文件夹下
  },
  // loader配置
  module: {
    rules: [
      // 详细的loader配置
      // 不同文件必须配置不同loader处理
      {
        // 匹配哪些文件
        test: /\.less$/,
        // 使用哪些loader进行处理
        use: [
          // use数组中loader执行顺序：从右到左，从下到上，依次执行(先执行css-loader)
          // style-loader：创建style标签，将js中的样式资源插入进去，添加到head中生效
          'style-loader',
          // css-loader：将css文件变成commonjs模块加载到js中，里面内容是样式字符串
          'css-loader',
          // less-loader：将less文件编译成css文件，需要下载less-loader和less
          'less-loader'
        ],
      },
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader'],
      },
      {
        // url-loader：处理图片资源，问题：默认处理不了html中的img图片
        test: /\.(jpg|png|gif)$/,
        // 需要下载 url-loader file-loader
        loader: 'url-loader',
        options: {
          // 图片大小小于8kb，就会被base64处理，优点：减少请求数量（减轻服务器压力），缺点：图片体积会更大（文件请求速度更慢）
          // base64在客户端本地解码所以会减少服务器压力，如果图片过大还采用base64编码会导致cpu调用率上升，网页加载时变卡
          limit: 8 * 1024,
          // 给图片重命名，[hash:10]：取图片的hash的前10位，[ext]：取文件原来扩展名
          name: '[hash:10].[ext]',
          // 问题：因为url-loader默认使用es6模块化解析，而html-loader引入图片是conmonjs，解析时会出问题：[object Module]
          // 解决：关闭url-loader的es6模块化，使用commonjs解析
          esModule: false,
          outputPath: 'imgs',
        },
      },
      {
        test: /\.html$/,
        // 处理html文件的img图片（负责引入img，从而能被url-loader进行处理）
        loader: 'html-loader',
      },
      // 打包其他资源(除了html/js/css资源以外的资源)
      {
        // 排除html|js|css|less|jpg|png|gif文件
        exclude: /\.(html|js|css|less|jpg|png|gif)/,
        // file-loader：处理其他文件
        loader: 'file-loader',
        options: {
          name: '[hash:10].[ext]',
          outputPath: 'media',
        },
      },
    ],
  },
  // plugin插件的配置
  plugins: [
    // html-webpack-plugin：默认会创建一个空的html文件，自动引入打包输出的所有资源（JS/CSS）
    // 需要有结构的HTML文件可以加一个template
    new HtmlWebpackPlugin({
      // 复制这个./src/index.html文件，并自动引入打包输出的所有资源（JS/CSS）
      template: './src/index.html',
    }),
  ],
  // 开发模式，如果不配置默认是以生产环境打包
  mode: 'development',
    
 //devServer这个配置也可以不配，直接使用默认即可。   
  // 开发服务器 webpack-dev-server：用来自动化，不用每次修改后都重新输入webpack打包一遍（自动编译，自动打开浏览器，自动刷新浏览器）
  // 特点：只会在内存中编译打包，不会有任何输出（不会在build文件夹中输出，而是存在于内存中，停止时自动释放）
  devServer: {
    // 运行代码的目录，一般构建到哪里就用哪里
    contentBase: resolve(__dirname, 'build'),
    // 启动gzip压缩
    compress: true,
    // 端口号
    port: 3000,
    // 自动打开浏览器
    open: true,
  },
}
```

建议在package.json中配置webpack打包dev-server的快捷方式

```json
"scripts": {
    "build": "webpack",
    "start": "webpack serve --open"
  }
```

- loader 和 plugin 的不同：（plugin 一定要先引入才能使用）

   loader：1. 下载 2. 使用（配置 loader）

   plugins：1.下载 2. 引入 3. 使用

## 生产环境基本配置

所需安装的插件和loader

```javascript
//html
html-loader html-webpack-plugin
//css
css-loader less-loader	mini-css-extract-plugin optimize-css-assets-webpack-plugin
//less
less-loader less
//eslint
eslint eslint-loader eslint-config-airbnb-base eslint-plugin-import
//babel
@babel/core @babel/preset-env babel-loader core-js
//postcss
postcss postcss-loader postcss-preset-env
```

```js
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')
// 单独生成css文件的loader
const MiniCssExtractPlugin = require('mini-css-extract-plugin')
const OptimizeCssAssetsWebpackPlugin = require('optimize-css-assets-webpack-plugin')
// 提取css和less文件中相同的loader，减少代码量
const commonCssLoader = [ MiniCssExtractPlugin.loader,
    'css-loader',
    // 引用postcss
    {
        loader: 'postcss-loader',
        options: {
            postcssOptions: {
                plugins: [
                    [
                        'postcss-preset-env',
                        {
                            //最新的两个浏览器版本
                            browers: 'last 2 versions'
                        }
                    ]
                ]
            }
        }
    }]
module.exports = {
    // 入口
    entry: './src/index.js',
    // 出口, __dirname表示当前文件的根路径，即文件夹名
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: 'js/bundle.js'
    },
    // loader配置
    module: {
        rules: [
            // 配置css
            {
                test: /\.css$/,
                use: [...commonCssLoader]
            },
            // 配置less
            {
                test: /\.less$/,
                use: [...commonCssLoader ,'less-loader']
            },
            // eslint语法检查
            {
                // 在package.json中添加属性 eslintConfig:{extends: 'airbnb-base'}
                test: /\.js$/,
                exclude: /node_modules/,
                // 让eslint优先执行，这样防止被babel后不符合规范
                enforce: 'pre',
                loader: 'eslint-loader',
                // 自动纠错
                options: {
                    fix: true
                }
            },
            // babel的js兼容性处理
            {
                test: /\.js$/,
                exclude: /node_modules/,
                loader: 'babel-loader',
                options: {
                    presets: [
                        '@babel/preset-env',
                        {
                            // 要兼容的浏览器版本
                            targets: {
                                chrome: '80',
                                ie: '9'
                            },
                            // 下的是多少版本就写几
                            corejs: 3,
                            // 使用方式，按需加载
                            useBuiltIns: 'usage'
                        }
                    ]
                }
            },
            // 配置img
            {
                test: /\.(jpg|png|gif)/,
                loader: 'url-loader',
                options: {
                    limit: 8 * 1024,
                    name: '[hash:10].[ext]',
                    //输出的文件夹
                    outputPath: 'imgs',
                    // 防止和html-loader的commonjs解析方式产生冲突
                    esModule: false
                },         

            }
            // 提取html中通过img标签引入的图片
            , {
                test: /\.html$/,
                loader: 'html-loader'
            },
            // 配置其他文件
            {
                exclude: /\.(css|less|jpg|png|gif|js)/,
                loader: 'file-loader',
                options: {
                    outputPath: 'media'
                }
            }
        ]
    },
    // 插件配置
    plugin: [
        // 配置自动生成html并引入相关js,css文件的插件
        new HtmlWebpackPlugin({
            title: '自定义title',
            //模板
            template: './src/index.html'    
        }),
        // 配置提取单独css文件的插件
        new MiniCssExtractPlugin({
            // 生成的文件名
            filename: 'css/built.css'
        }),
        // 配置压缩css文件的插件
        new OptimizeCssAssetsWebpackPlugin()
    ],
    // 开发模式
    mode: 'production'
}
```

## webpack优化配置

### 开发环境性能优化

#### HMR（热模块热替换）

HMR: hot module replacement 热模块替换 / 模块热替换

作用：一个模块发生变化，只会重新打包构建这一个模块（而不是打包所有模块） ，极大提升构建速度

代码：只需要在 devServer 中设置 hot 为 true，就会自动开启HMR功能（只能在开发模式下使用）

```javascript
devServer: {
	...
  // 开启HMR功能
  // 当修改了webpack配置，新配置要想生效，必须重启webpack服务
  hot: true
}
```

每种文件实现热模块替换的情况：

- 样式文件：可以使用HMR功能，因为开发环境下使用的 style-loader 内部默认实现了热模块替换功能

- js 文件：默认不能使用HMR功能（修改一个 js 模块所有 js 模块都会刷新）

  --> 实现 HMR 需要修改 js 代码（添加支持 HMR 功能的代码）

  ```javascript
  // 绑定
  if (module.hot) {
    // 一旦 module.hot 为true，说明开启了HMR功能。 --> 让HMR功能代码生效
    module.hot.accept('./print.js', function() {
      // 方法会监听 print.js 文件的变化，一旦发生变化，只有这个模块会重新打包构建，其他模块不会。
      // 会执行后面的回调函数
      print();
    });
  }
  ```

  注意：HMR 功能对 js 的处理，只能处理非入口 js 文件的其他文件。

- html 文件: 默认不能使用 HMR 功能（html 不用做 HMR 功能，因为只有一个 html 文件，不需要再优化）。使用 HMR 会导致问题：html 文件不能热更新了（不会自动打包构建）

  解决：修改 entry 入口，将 html 文件引入（这样 html 修改整体刷新）

  ```javascript
  entry: ['./src/js/index.js', './src/index.html']
  ```

#### source-map

source-map：一种提供**源代码到构建后代码的映射**的技术 （如果构建后代码出错了，通过映射可以追踪源代码错误）

参数：`[inline-|hidden-|eval-][nosources-][cheap-[module-]]source-map`

**配置**

```javascript
//webpack.config.js 
module.exports = {
	devtool: 'eval-source-map'
}
```

**开发环境**：需要考虑速度快，调试更友好

eval-source-map（完整度高，内联速度快） 

eval-cheap-module-souce-map（错误提示忽略列但是包含其他信息，内联速度快）

**生产环境**：需要考虑源代码要不要隐藏，调试要不要更友好

source-map（最完整） 

cheap-module-souce-map（错误提示一整行忽略列）

### 生产环境性能优化

#### 优化打包构建速度

##### oneOf

oneOf：匹配到 loader 后就不再向后进行匹配，优化生产环境的打包构建速度。

```javascript
module: {
	rules: [
		{
          // js 语法检查
          test: /\.js$/,
          exclude: /node_modules/,
          // 优先执行
          enforce: 'pre',
          loader: 'eslint-loader',
          options: {
            fix: true
          },
        {
    // oneOf 优化生产环境的打包构建速度
    // 以下loader只会匹配一个（匹配到了后就不会再往下匹配了）
    // 注意：不能有两个配置处理同一种类型文件（所以把eslint-loader提取出去放外面）
          oneOf: [
            {
              test: /\.css$/,
              use: [...commonCssLoader]
            },
             {
              // js 兼容性处理
              test: /\.js$/,
              exclude: /node_modules/,
              loader: 'babel-loader',

            }
        ]
       }
	] 
}
```

##### 缓存

**babel缓存**

类似 HMR，将 babel 处理后的资源缓存起来（哪里的 js 改变就更新哪里，其他 js 还是用之前缓存的资源），让第二次打包构建速度更快

```javascript
{
    test: /\.js$/,
        exclude: /node_modules/,
            loader: 'babel-loader',
                options: {
                    presets: [
                        '@babel/preset-env',
                        {
                            // 要兼容的浏览器版本
                            targets: {
                                chrome: '80',
                                ie: '9'
                            },
                            // 下的是多少版本就写几
                            corejs: 3,
                            // 使用方式，按需加载
                            useBuiltIns: 'usage'
                        }
                    ],
       // 开启babel缓存
        // 第二次构建时，会读取之前的缓存
        cacheDirectory: true
            }
        }
```

**文件资源缓存**

文件名不变，就不会重新请求，而是再次用之前缓存的资源。

1.hash: 每次 wepack 打包时会生成一个唯一的 hash 值。

 问题：重新打包，所有文件的 hsah 值都改变，会导致所有缓存失效。（可能只改动了一个文件）

2.chunkhash：根据 chunk 生成的 hash 值。来源于同一个 chunk的 hash 值一样

 问题：js 和 css 来自同一个chunk，hash 值是一样的（因为 css-loader 会将 css 文件加载到 js 中，所以同属于一个chunk）

3.contenthash: 根据文件的内容生成 hash 值。不同文件 hash 值一定不一样(文件内容修改，文件名里的 hash 才会改变)

修改 css 文件内容，打包后的 css 文件名 hash 值就改变，而 js 文件没有改变 hash 值就不变，这样 css 和 js 缓存就会分开判断要不要重新请求资源 --> 让代码上线运行缓存更好使用

```javascript
//使用方法就是将输出的文件名加上contenthash前缀，如下，打包后的js文件也要改
plugins: [
    new MiniCssExtractPlugin({
        filename: 'css/built.[contenthash:10].css'
    })
]
```

##### 多进程打包

某个任务消耗时间较长会卡顿，多进程可以同一时间干多件事，效率更高。

优点是提升打包速度；缺点是每个进程的开启和交流都会有开销（babel-loader消耗时间最久，所以使用thread-loader针对其进行优化）

```javascript
{
  test: /\.js$/,
  exclude: /node_modules/,
  use: [
    /* 
      thread-loader会对其后面的loader（这里是babel-loader）开启多进程打包。 
      进程启动大概为600ms，进程通信也有开销。(启动的开销比较昂贵，不要滥用)
      只有工作消耗时间比较长，才需要多进程打包
    */
    {
      loader: 'thread-loader',
      options: {
        workers: 2 // 进程2个
      }
    },
    {
      loader: 'babel-loader',
      options: ...,
        // 开启babel缓存
        // 第二次构建时，会读取之前的缓存
        cacheDirectory: true
      }
    }
  ]
}
```

##### externals

让某些库不打包，通过 cdn 引入

```javascript
//webpack.config.js
externals: {
  // 拒绝jQuery被打包进来(通过cdn引入，速度会快一些)
  // 忽略的库名 -- npm包名
  jquery: 'jQuery'
}
```

需要在 index.html 中通过 cdn 引入：

```html
<script src="https://cdn.bootcss.com/jquery/1.12.4/jquery.min.js"></script>
```

#### 优化代码运行的性能

##### 树摇tree shaking

tree shaking：去除无用代码，减少代码体积

前提：1. 必须使用 ES6 模块化 2. 开启 production 环境 （这样就自动会把无用代码去掉）

##### 代码分割code split

代码分割。将打包输出的一个大的 bundle.js 文件拆分成多个小文件，这样可以并行加载多个文件，比加载一个文件更快。

1.多入口拆分（一般不用，因为基本都是单页面应用）

```javascript
entry: {
    // 多入口：有几个入口就输出几个bundle
    index: './src/js/index.js',
    test: './src/js/test.js'
  },
  output: {
    // [name]：取文件名
    filename: 'js/[name].[contenthash:10].js',
    path: resolve(__dirname, 'build')
  },
```

2.optimization：

```javascript
module.exports = {
  optimization: {
    splitChunks: {
      chunks: 'all'
    }
  }
}

```

- 将 node_modules 中的代码单独打包（大小超过30kb）
- 自动分析多入口chunk中，有没有公共的文件。如果有会打包成单独一个chunk(比如两个模块中都引入了jquery会被打包成单独的文件)（大小超过30kb）

3.import 动态导入语法：

```javascript
/*
  通过js代码，让某个文件被单独打包成一个chunk
  import动态导入语法：能将某个文件单独打包(test文件不会和index打包在同一个文件而是单独打包)
  webpackChunkName:指定test单独打包后文件的名字
*/
import(/* webpackChunkName: 'test' */'./test')
  .then((res) => {
    // 文件加载成功~，res就是引入的test模块，可以打印自己查看
    console.log(res);
  })
  .catch(() => {
    // eslint-disable-next-line
    console.log('文件加载失败~');
  });
```

##### 懒加载/预加载

1.懒加载：当文件需要使用时才加载（需要代码分割）。但是如果资源较大，加载时间就会较长，有延迟。

2.正常加载：可以认为是并行加载（同一时间加载多个文件）没有先后顺序，先加载了不需要的资源就会浪费时间。

3.预加载 prefetch（兼容性很差）：会在使用之前，提前加载。等其他资源加载完毕，浏览器空闲了，再偷偷加载这个资源。这样在使用时已经加载好了，速度很快。所以在懒加载的基础上加上预加载会更好。

```javascript
document.getElementById('btn').onclick = function() {
  // 将import的内容放在异步回调函数中使用，点击按钮，test.js才会被加载(不会重复加载)
  // webpackPrefetch: true表示开启预加载
  import(/* webpackChunkName: 'test', webpackPrefetch: true */'./test').then(({ mul }) => {
    console.log(mul(4, 5));
  });
  import('./test').then(({ mul }) => {
    console.log(mul(2, 5))
  })
};
```

## webpack配置详情

### entry

**1.string --> './src/index.js'，单入口**

打包形成一个 chunk。 输出一个 bundle 文件。此时 chunk 的名称默认是 main

**2.array --> ['./src/index.js', './src/add.js']，多入口**

所有入口文件最终只会形成一个 chunk，输出出去只有一个 bundle 文件。

（一般只用在 HMR 功能中让 html 热更新生效）

**3.object，多入口**

有几个入口文件就形成几个 chunk，输出几个 bundle 文件，此时 chunk 的名称是 key 值

**特殊用法**

```javascript
entry: {
  // 最终只会形成一个chunk, 输出出去只有一个bundle文件。
  index: ['./src/index.js', './src/count.js'], 
  // 形成一个chunk，输出一个bundle文件。
  add: './src/add.js'
}
```

### output

```javascript
output: {
  // 文件名称（指定名称+目录）
  filename: 'js/[name].js',
  // 输出文件目录（将来所有资源输出的公共目录）
  path: resolve(__dirname, 'build'),
  // 所有资源引入公共路径前缀 --> 'imgs/a.jpg' --> '/imgs/a.jpg'
  publicPath: '/',
 // 指定非入口chunk的名称，例如通过import引入的文件
  chunkFilename: 'js/[name]_chunk.js',
  // 打包整个库后向外暴露的变量名，多与dll结合使用   
  library: '[name]', 
  libraryTarget: 'window' // 变量名添加到哪个上 browser：window
  // libraryTarget: 'global' // node：global
  // libraryTarget: 'commonjs' // conmmonjs模块 exports
},
```

### module

```javascript
module: {
  rules: [
    // loader的配置
    {
      test: /\.css$/,
      // 多个loader用use
      use: ['style-loader', 'css-loader']
    },
    {
      test: /\.js$/,
      // 排除node_modules下的js文件
      exclude: /node_modules/,
      // 只检查src下的js文件
      include: resolve(__dirname, 'src'),
      enforce: 'pre', // 优先执行
      // enforce: 'post', // 延后执行
      // 单个loader用loader
      loader: 'eslint-loader',
      options: {} // 指定配置选项
    },
    {
      // 以下配置只会生效一个
      oneOf: []
    }
  ]
},
```

### resolve

```javascript
// 解析模块的规则
resolve: {
  // 配置解析模块路径别名: 优点：当目录层级很复杂时，简写路径；缺点：路径不会提示
  alias: {
    $css: resolve(__dirname, 'src/css')
  },
  // 配置省略文件路径的后缀名（引入时就可以不写文件后缀名了）
  extensions: ['.js', '.json', '.jsx', '.css'],
  // 告诉 webpack 解析模块应该去找哪个目录
  modules: [resolve(__dirname, '../../node_modules'), 'node_modules']
}
```

这样配置后，引入文件就可以这样简写：`import '$css/index';`

### devServer

```javascript
devServer: {
  // 运行代码所在的目录
  contentBase: resolve(__dirname, 'build'),
  // 监视contentBase目录下的所有文件，一旦文件变化就会reload
  watchContentBase: true,
  watchOptions: {
    // 忽略文件
    ignored: /node_modules/
  },
  // 启动gzip压缩
  compress: true,
  // 端口号
  port: 5000,
  // 域名
  host: 'localhost',
  // 自动打开浏览器
  open: true,
  // 开启HMR功能
  hot: true,
  // 不要显示启动服务器日志信息
  clientLogLevel: 'none',
  // 除了一些基本信息外，其他内容都不要显示
  quiet: true,
  // 如果出错了，不要全屏提示
  overlay: false,
  // 服务器代理，--> 解决开发环境跨域问题
  proxy: {
    // 一旦devServer(5000)服务器接收到/api/xxx的请求，就会把请求转发到另外一个服务器3000
    '/api': {
      target: 'http://localhost:3000',
      // 发送请求时，请求路径重写：将/api/xxx --> /xxx （去掉/api）
      pathRewrite: {
        '^/api': ''
      }
    }
  }
}
```

其中，跨域问题：同源策略中不同的协议、端口号、域名就会产生跨域。

正常的浏览器和服务器之间有跨域，但是服务之间没有跨域。代码通过代理服务器运行，所以浏览器和代理服务器之间没有跨域，浏览器把请求发送到代理服务器上，代理服务器替你转发到另外一个服务器上，服务器之间没有跨域，所以请求成功。代理服务器再把接收到的响应响应给浏览器。这样就解决开发环境下的跨域问题。

### optimization

contenthash 缓存会导致一个问题：修改 a 文件导致 b 文件 contenthash 变化。
因为在 index.js 中引入 a.js，打包后 index.js 中记录了 a.js 的 hash 值，而 a.js 改变，其重新打包后的 hash 改变，导致 index.js 文件内容中记录的 a.js 的 hash 也改变，从而重新打包后 index.js 的 hash 值也会变，这样就会使缓存失效。（改变的是a.js文件但是 index.js 文件的 hash 值也改变了）
解决办法：runtimeChunk --> 将当前模块记录其他模块的 hash 单独打包为一个文件 runtime，这样 a.js 的 hash 改变只会影响 runtime 文件，不会影响到 index.js 文件

```javascript
output: {
  filename: 'js/[name].[contenthash:10].js',
  path: resolve(__dirname, 'build'),
  chunkFilename: 'js/[name].[contenthash:10]_chunk.js' // 指定非入口文件的其他chunk的名字加_chunk
},
optimization: {
  splitChunks: {
    chunks: 'all',
    /* 以下都是splitChunks默认配置，可以不写
    miniSize: 30 * 1024, // 分割的chunk最小为30kb（大于30kb的才分割）
    maxSize: 0, // 最大没有限制
    minChunks: 1, // 要提取的chunk最少被引用1次
    maxAsyncRequests: 5, // 按需加载时并行加载的文件的最大数量为5
    maxInitialRequests: 3, // 入口js文件最大并行请求数量
    automaticNameDelimiter: '~', // 名称连接符
    name: true, // 可以使用命名规则
    cacheGroups: { // 分割chunk的组
      vendors: {
        // node_modules中的文件会被打包到vendors组的chunk中，--> vendors~xxx.js
        // 满足上面的公共规则，大小超过30kb、至少被引用一次
        test: /[\\/]node_modules[\\/]/,
        // 优先级
        priority: -10
      },
      default: {
        // 要提取的chunk最少被引用2次
        minChunks: 2,
        prority: -20,
        // 如果当前要打包的模块和之前已经被提取的模块是同一个，就会复用，而不是重新打包
        reuseExistingChunk: true
      }
    } */
  },
  // 将index.js记录的a.js的hash值单独打包到runtime文件中
  runtimeChunk: {
    name: entrypoint => `runtime-${entrypoint.name}`
  },
  minimizer: [
    // 配置生产环境的压缩方案：js/css
    new TerserWebpackPlugin({
      // 开启缓存
      cache: true,
      // 开启多进程打包
      parallel: true,
      // 启用sourceMap(否则会被压缩掉)
      sourceMap: true
    })
  ]
}
```

## webpack5

### 默认值

- `entry: "./src/index.js`
- `output.path: path.resolve(__dirname, "dist")`
- `output.filename: "[name].js"`

# vuecli

## 安装vuecli

```
npm install @vue/cli -g		//默认安装的是最新版本
npm install @vue/cli-init -g 	//拉取2.x版本，不是必须
```

## 创建项目

```
vue create objectname	//vuecli3初始化项目，项目名必须小写
vue init webpack objectname 	//vuecli2初始化项目
```

## 配置路径别名

- 注意在html中使用路径别名要加上~前缀，例如<  img  src="~assets/img/xx.jpg" >

### vuecli3/4配置路径别名

```javascript
// vue.config.js，该文件要自己新建并于package.json在同一级目录
const path = require('path');//引入path模块
function resolve(dir){
    return path.join(__dirname,dir)//path.join(__dirname)设置绝对路径
}
module.exports={
    chainWebpack:(config)=>{
        config.resolve.alias
        .set('@',resolve('./src'))
        .set('components',resolve('./src/components'))
        .set('views',resolve('src/views'))
        .set('assets',resolve('src/assets'))
        .set('common',resolve('src/common'))
        .set('network',resolve('src/network'))
        //set第一个参数：设置的别名，第二个参数：设置的路径
　　　　
    }
}
```

### vuecli2配置路径别名

```javascript
// webpack.base.config.js
resolve: {
    extensions: ['.js','.vue','.json'],
    alias: {
        '@' : resolve('src'),
        'assets' : resolve('src/assets'),
        'components' : resolve('src/components'),
        'views' : resolve('src/views'),
    }
}
```

## 配置sass

```html
<style lang="scss" scoped>
</style>
$ npm install node-sass --save-dev

$ npm install sass-loader --save-dev
```

## 为开发模式与发布模式指定不同的打包入口

**1.基本介绍**

默认情况下，Vue项目的开发模式与发布模式，共用一个打包的入口文件，即（src/main.js)。为了将项目的开发过程与发不过程分离，我们可以分为两种模式，各自指定打包的入口文件。即：

①开发模式的入口文件为：src/main-dev.js

@发布模式的入口文件为：src/main-prod.js

**2.configureWebpack和chainWebpack**

vue.config.js导出的配置对象中，新增`configureWebpack`和`chainWebpack`节点，来自定义webpack的打包配置。

他俩的作用相同，唯一的区别就是他们修改webpack配置的方式不同：

①chainWebpack通过链式编程的形式，来修改默认的webpack配置。

②configureWebpack通过操作对象的形式，来修改默认的webpack配置。

**3.通过chainWebpack自定义打包入口**

```js
module.exports = {
	chainWebpack : config => {
        config.when(process.env.NODE_ENV === 'production', config => {
            config.entry('app').clear().add('./src/main-prod.js')
		})
        config.when(process.env.NODE_ENV === 'development', config => {
            config.entry('app').clear().add('./src/main-dev.js')
		})
	}
}
```

## 优化打包大小

默认情况下打包的文件比较大，我们可以通过externals加载外部CDN资源来优化它。当然一般都是在生产环境时才会用到。

```js
module.exports = {
	chainWebpack : config => {
        config.when(process.env.NODE_ENV === 'production', config => {
            config.entry('app').clear().add('./src/main-prod.js')
            config.set('externals', {
                vue: 'Vue',
                'vue-router': 'VueRouter',
                axios：'axios',
                lodash: '_',
                echarts: 'echarts',
                nprogress: 'NProgress',
                'vue-quill-editor': 'VueQuillEditor'
            })
		})
        config.when(process.env.NODE_ENV === 'development', config => {
            config.entry('app').clear().add('./src/main-dev.js')
		})
	}
}
```

之后在public的index.html中利用cdn链接来进行引入

## 打包路径设置

默认`npm run build`打包后的静态资源文件static是根路径，需要改成相对路径。