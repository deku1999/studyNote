# 总览

- webpack依赖node环境，vuecli依赖webpack。

# webpack

```
npm install webpack -g 		//全局安装webpack
```

## 使用流程

- 局部安装webpack

```
npm install webpack --save-dev
```

- 新建src和dist文件夹，dist为发布文件夹放打包后的js，src为开发时的文件，另需个index.html作为浏览器打开展示的首页。
- npm init ，初始化生成package.json文件；npm install，安装依赖。
- 配置映射关系

```
//package.json
{
"scripts" : {
	"build" : "webpack"
}
}
```

- 配置打包的入口和出口，现在npm run build 就可以进行打包了。

```javascript
// webpack.config.js，该文件是新建出来的与src，dist文件夹同级
const path = require('path')
module.exports = {
    entry: './src/main.js',		//该文件为路口
    output: {
        path: path.resolve(__dirname,'dist'),
        filename: 'bundle.js'	//打包到dist文件下的bundle.js文件中
    }
}
```

- 直接打包可能会报一些错，因为webpack编译一些文件需要安装相应loader，就是npm安装然后在入口文件引入，以及webpack.config.js文件配置一下就行。请自行从网上查阅。

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

