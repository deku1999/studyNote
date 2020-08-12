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

