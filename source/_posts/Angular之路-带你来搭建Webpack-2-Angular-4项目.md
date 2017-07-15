---
title: Angular之路--带你来搭建Webpack 2 + Angular 4项目
date: 2017-04-12
tags:
---
上个月`Angular`发布了`4.0.0`版本，少年们，赶快学起来吧，这篇文章带领大家搭建一个简单的`Angular`应用，会尽量详细的把每个点都解释到。
   
首先我选择了用`webpack2`来作为打包工具，选择`wenpack2`的理由不言而喻。这里假设你已经了解`webpack2`的一些原理，下面开始来学习吧～

详细代码可以看我的[git项目](https://github.com/yanyixin/angular2-todolist)

<!--more-->

## 一、配置 webpack

首先新建一个项目文件夹

``` JavaScript
mkdir angular-dream
cd angular-dream
```
在控制台中输入命令 `npm init` ，创建 `package.json` 文件。如图：

![创建package.json文件](https://dn-mhke0kuv.qbox.me/9b59abb62c90ea6f70e0.png)
在控制台中可以一路回车。当然，这里我命名了项目的名称为 `angular-dream` ，还有一些其他的信息。

创建好之后用编辑器（我使用的是webstorm）打开这个项目。

### package.json 文件的配置
关于 `package.json` 文件里面的一些参数的含义，可以参考[阮一峰老师写的这篇文章](http://javascript.ruanyifeng.com/nodejs/packagejson.html#toc1) 。
``` Javascript
{
  "name": "angular2-dream",
  "version": "1.0.0",
  "description": "Hello Angular2",
  "scripts": {
    "start": "webpack-dev-server --config config/webpack.dev.js --progress",
    "test": "karma start",
    "build": "webpack --config config/webpack.dev.js --progress --profile --bail",
    "webpack": "webpack",
    "rimraf": "rimraf"
  },
  "keywords": [
    "angular2",
    "webpack"
  ],
  "author": "yanmeng@hujiang.com",
  "license": "MIT",
  "dependencies": {
    "@angular/animations": "~4.0.1",
    "@angular/common": "~4.0.0",
    "@angular/compiler": "~4.0.0",
    "@angular/core": "^4.0.1",
    "@angular/forms": "~4.0.1",
    "@angular/http": "~4.0.1",
    "core-js": "^2.4.1",
    "rxjs": "5.2.0",
    "zone.js": "^0.8.5"
  },
  "devDependencies": {
    "reflect-metadata": "^0.1.10",
    "html-webpack-plugin": "^2.28.0",
    "@angular/compiler-cli": "~4.0.1",
    "@angular/platform-browser": "~4.0.1",
    "@angular/platform-browser-dynamic": "~4.0.1",
    "@angular/platform-server": "~4.0.1",
    "@angular/router": "~4.0.1",
    "@angularclass/hmr": "^1.2.2",
    "@angularclass/hmr-loader": "^3.0.2",
    "@types/jasmine": "^2.5.43",
    "@types/node": "^6.0.45",
    "angular2-template-loader": "^0.6.0",
    "awesome-typescript-loader": "^3.0.4",
    "bootstrap": "^4.0.0-alpha.6",
    "bootstrap-sass": "^3.3.7",
    "css-loader": "^0.26.1",
    "extract-text-webpack-plugin": "2.0.0-beta.5",
    "file-loader": "^0.9.0",
    "font-awesome": "^4.7.0",
    "html-loader": "^0.4.3",
    "postcss-loader": "^1.3.1",
    "raw-loader": "^0.5.1",
    "style-loader": "^0.13.1",
    "to-string-loader": "^1.1.5",
    "ts-helpers": "^1.1.2",
    "url-loader": "^0.5.7",
    "webpack": "2.2.0",
    "webpack-dev-server": "2.2.0-rc.0",
    "webpack-merge": "^2.4.0",
    "typescript": "^2.2.2"
  }
}

```

* `@angular/compiler` - Angular的模板编译器。 它会理解模板，并且把模板转化成代码，以供应用程序运行和渲染。 开发人员通常不会直接跟这个编译器打交道，而是通过platform-browser-dynamic或离线模板编译器间接使用它。
* `@angular/platform-browser` - 与DOM和浏览器相关的每样东西，特别是帮助往DOM中渲染的那部分。 这个包还包含bootstrapStatic方法，用来引导那些在产品构建时需要离线预编译模板的应用程序
* `@angular/platform-browser-dynamic` - 为应用程序提供一些提供商和bootstrap方法，以便在客户端编译模板。不要用于离线编译。 我们使用这个包在开发期间引导应用，以及引导plunker中的范例。
* `core-js` - 为全局上下文(window)打的补丁，提供了ES2015(ES6)的很多基础特性。 我们也可以把它换成提供了相同内核API的其它填充库。 一旦所有的“主流浏览器”都实现了这些API，这个依赖就可以去掉了。
* `reflect-metadata` - 一个由Angular和TypeScript编译器共享的依赖包。

### tsconfig.json 文件的配置
在项目的根目录下创建 `tsconfig.json` 文件。

浏览器不能直接执行 `TypeScript` ，需要用编译器转译成JavaScript，而且编译器需要进行一些配置。 `tsconfig.json` 的配置就是指导编译器如何生成JavaScript文件。

``` JavaScript
{
  "compilerOptions": {
    "declaration": false,
    "module": "commonjs", // 组织代码的方式
    "target": "es5", // 编译目标平台
    "moduleResolution": "node",
    "sourceMap": true, // 把ts文件变异成js文件时，是否生成对应的SourceMap文件
    "emitDecoratorMetadata": true, // 让TypeScript支持为带有装饰器的声明生成元数据
    "experimentalDecorators": true, // 是否启用实验性装饰器特性
    "noImplicitAny": true,
    "lib": ["dom", "es6"],
    "suppressImplicitAnyIndexErrors": true
  },
  "exclude": [
    "node_modules",
    "dist"
  ],
  "awesomeTypescriptLoaderOptions": {
    "forkChecker": true,
    "useWebpackText": true
  },
  "compileOnSave": false,
  "buildOnSave": false
}

```

当 `noImplicitAny` 标志是 `true` 并且TypeScript编译器无法推断出类型时，它仍然会生成JavaScript文件。 但是它也会报告一个错误。 很多饱经沧桑的程序员更喜欢这种严格的设置，因为类型检查能在编译期间捕获更多意外错误。

### 创建 webpack.config.js文件
在根目录下创建 `webpack.config.js`文件

``` JavaScript
module.exports = require('./config/webpack.dev.js');
```

现在在控制台中执行 `npm install` 命令，安装项目的依赖。

## 二、Polyfills
配置好上述的几个文件之后呢，我们在项目中的根目录下创建一个 `src` 文件夹。

在 `src` 文件夹的下面新建一个 `polyfills.ts` 文件。

`polyfills.ts` 文件里引入了运行Angular应用时所需的一些标准js。
``` JavaScript
import 'core-js/es6/symbol';
import 'core-js/es6/object';
import 'core-js/es6/function';
import 'core-js/es6/parse-int';
import 'core-js/es6/parse-float';
import 'core-js/es6/number';
import 'core-js/es6/math';
import 'core-js/es6/string';
import 'core-js/es6/date';
import 'core-js/es6/array';
import 'core-js/es6/regexp';
import 'core-js/es6/map';
import 'core-js/es6/set';
import 'core-js/es6/weak-map';
import 'core-js/es6/weak-set';
import 'core-js/es6/typed';

/** Evergreen browsers require these. **/
import 'core-js/es6/reflect';

import 'core-js/es7/reflect';

/***************************************************************************************************
 * Zone JS is required by Angular itself.
 */
import 'zone.js/dist/zone';

import 'ts-helpers';

if (process.env.ENV === 'production') {
  // Production
} else {
  // Development and test
  Error['stackTraceLimit'] = Infinity;
  require('zone.js/dist/long-stack-trace-zone');
}
```
## 三、Vendor

在 `src` 文件夹的下面新建一个 `vendor.ts` 文件。

`vendor.ts` 文件里面引入了一些第三方的依赖。
``` JavaScript
// Angular
//包含所有提供商依赖
import '@angular/platform-browser';
import '@angular/platform-browser-dynamic';
import '@angular/compiler';
import '@angular/core';  // 存放核心代码，如变化监测机制，依赖注入机制，渲染，装饰器等。
import '@angular/common';
import '@angular/http';
import '@angular/router';

// RxJS
import 'rxjs/Observable';
import 'rxjs/Subscription';
import 'rxjs/Subject';
import 'rxjs/BehaviorSubject';

// Bootsctrap
import 'bootstrap/dist/css/bootstrap.css';
import 'font-awesome/css/font-awesome.css';

```
## 四、Main
在 `src` 文件夹的下面新建一个 `main.ts` 文件。

在 `main.ts` 文件中，我们指定了项目的根模块为 `AppModule`

``` Javascript
import {AppModule} from './app/app.module';
import { platformBrowserDynamic } from "@angular/platform-browser-dynamic";

platformBrowserDynamic().bootstrapModule(AppModule);

// platformBrowserDynamic().bootstrapModule()方法来编译启用AppModule模块
// 根据当前的运行环境，如操作系统、浏览器，来初始化一个运行环境，然后从这个环境里面运行AppModule。

```

## 五、config
在根目录下创建一个 `config` 文件夹

### helpers.js
在 `config` 文件夹下面创建一个 `helpers.js` 文件。

在这里请注意入口 `polyfills`,`vendor` 和 `app` 的先后顺序。
``` JavaScript
var path = require('path');
var _root = path.resolve(__dirname, '..');
function root(args) {
  args = Array.prototype.slice.call(arguments, 0);
  return path.join.apply(path, [_root].concat(args));
}
exports.root = root;
```
### webpack.common.js
在 `config` 文件夹下面创建一个 `webpack.common.js` 文件。
``` Javascript
const helpers = require('./helpers');
var webpack = require('webpack');
var HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: {
    'polyfills': './src/polyfills.ts', // 运行Angular时所需的一些标准js
    'vendor': './src/vendor.ts', // Angular、Lodash、bootstrap.css......
    'app': './src/main.ts' // 应用代码
  },
  resolve: { // 解析模块路径时的配置
    extensions: ['.ts', '.js'] // 制定模块的后缀，在引入模块时就会自动补全
  },
  module: {
    rules: [ // 告诉webpack每一类文件需要使用什么加载器来处理
      {
        test   : /\.ts$/,
        loaders: ['awesome-typescript-loader', 'angular2-template-loader']
        //awesome-typescript-loader - 一个用于把TypeScript代码转译成ES5的加载器，它会由tsconfig.json文件提供指导
        //angular2-template-loader - 用于加载Angular组件的模板和样式
      }, {
        test: /\.json$/,
        use : 'json-loader'
      }, {
        test: /\.styl$/,
        loader: 'css-loader!stylus-loader'
      }, {
        test   : /\.css$/,
        loaders: ['to-string-loader', 'css-loader']
      }, {
        test: /\.html$/,
        use: 'raw-loader',
        exclude: [helpers.root('src/index.html')]
        //html - 为组件模板准备的加载器
      }, {
        test:/\.(jpg|png|gif)$/,
        use:"file-loader"
      }, {
        test: /\.woff(2)?(\?v=[0-9]\.[0-9]\.[0-9])?$/,
        use : "url-loader?limit=10000&minetype=application/font-woff"
      }, {
        test: /\.(ttf|eot|svg)(\?v=[0-9]\.[0-9]\.[0-9])?$/,
        use : "file-loader"
      }
    ]
  },
  plugins: [
    //热替换
    new webpack.HotModuleReplacementPlugin(),
    new webpack.optimize.CommonsChunkPlugin({
      name: ['vendor', 'polyfills']
      //多个html共用一个js文件，提取公共代码
    }),
    
    new HtmlWebpackPlugin({
      template: './src/index.html'
      // 自动向目标.html文件注入script和link标签
    })
  ]
};
```

### webpack.dev.js
在 `config` 文件夹下面创建一个 `webpack.dev.js` 文件。
``` JavaScript
var webpackMerge = require('webpack-merge');
var commonConfig = require('./webpack.common.js');
const helpers = require('./helpers');

module.exports = webpackMerge(commonConfig, {
  output   : {
    path      : helpers.root('dist'),
    publicPath: '/',
    filename  : '[name].js'
  },
  devServer: {
    port              : 8080,
    historyApiFallback: true
  }
});
```
至此，现在的目录结构就如下图所示：

![](https://dn-mhke0kuv.qbox.me/01070446b15d18534a3d)
因为我们还没有创建 `AppModule` ，所以 `main.ts` 文件会被标红。

## 六、根模块 AppModule
基本的配置已经完成啦，现在我们来创建根模块～

在 `src` 文件下面新建一个 `app` 文件夹，
### 创建 app.component.ts
在`app` 文件夹下面新建 `app.component.ts` 文件

``` JavaScript
import { Component } from "@angular/core";

@Component({
  selector   : 'root-app',
  templateUrl: './app.component.html'
})
export class AppComponent {
  constructor() {}
}
```
### 创建 app.component.html
在 `app` 文件夹下面新建 `app.component.html` 文件
``` HTML
<h1 class="title">Hello Angular2</h1>

<router-outlet></router-outlet>
```
### 创建 app.routes.ts
这里我们用一下路由来完成页面之间的跳转
``` Javascript
import { Routes } from '@angular/router';
import { AppComponent } from "./app.component";
export const routes: Routes = [ // Routes类型的数组
  {
    path      : 'index',
    component : AppComponent
  },{
    path      : '',
    redirectTo: 'index',
    pathMatch : 'full'
  }
];

```
### 创建 app.module.ts
在 `app` 文件夹下面新建 `app.module.ts` 文件
``` JavaScript
import { AppComponent } from './app.component';
import { routes } from './app.routes';
import { BrowserModule } from "@angular/platform-browser";
import { FormsModule } from "@angular/forms";
import { RouterModule } from "@angular/router";
import { NgModule } from "@angular/core";
//@NgModule装饰器用来为模块定义元数据
@NgModule({ // @NgModule 用来定义模块用的装饰器
  declarations: [AppComponent], // 导入模块所依赖的组件、指令等,用于指定这个模块的视图类
  imports: [
    BrowserModule, //包含了commonModule和applicationModule模块,封装在浏览器平台运行时的一些工具库
    FormsModule,  // 表单相关的组件指令等，包含了[(ngModel)]
    RouterModule.forRoot(routes,{useHash: false}), // RouterModule.forRoot()方法来创建根路由模块
  ], // 导入当前模块所需要的其他模块
  bootstrap: [AppComponent], // 标记出引导组件
  //把这个AppComponent标记为引导 (bootstrap) 组件。当Angular引导应用时，它会在DOM中渲
  //染AppComponent，并把结果放进index.html的元素内部。
})
export class AppModule { }
```

## 六、宿主页面

在 `src` 文件夹下面新建 `index.html` 文件

``` HTML
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>Angular2 Hello Word</title>
    <base href="/">
    <meta name="viewport" content="width=device-width, initial-scale=1">
</head>
<body>
    <root-app>Loading...</root-app>
</body>
</html>
```


好啦，此时的项目目录结构就是下图所示：
![](https://dn-mhke0kuv.qbox.me/3ec563d462c569bfe32c)

接下来运行 `npm start` 开始你的 `Angular` 之旅吧～

参考：
* [Angular官方网站](https://angular.io)