## webpack4

+ webpack定义

> 是一个模块打包的工具

+ webpcak一些安装技巧

> cnpm info webpack //可以查看webpack有关的一些版本号
>
> cnpm install webpack@4.16.5 根据版本号安装webpack

+ webpack4安装步骤

> cnpm init //初始化package.json
>
> cnpm install webpack webpack-cli --save-dev 安装webpack4
>
> npx webpack 需要打包的文件名
>
> npx是npm运行webpack一个命令，如果你没有装全局webpack可以用npx

+ webpack 使用技巧

> npx webpack --cofig 文件名 //可以以这个文件名内的规则对项目进行打包
>
> - global
>
>   > webpack
>
> - local
>
>   > npx webpack 文件名
>
> - 配置package.json的scripts标签
>
>   > cnpm run 标签名

------



### webpack默认文件 webpack.config.js

+ 入口与出口 **entry** and **output**

> ```javascript
> const path =require('path')
> 
> module.exports={
>     entry:'./index.js', //设置入口文件
>     //entry:{
>     	//main:'./index.js'，//打包后的名字为main.js
>         //sub:'./index.js'//打包后的名字为sub.js
> 	//}, 
>     output: {
>         filename: "bundle.js",//打包后的文件名 
>         path: path.resolve(__dirname,'dist')//设置打包后的路径地址，这里的路径一定是绝对路径
>     }
> }
> 
> //entry与output打包多个文件的配置
> module.exports={
>    // entry:'./index.js', //设置入口文件
>     entry:{
>     	main:'./index.js'，//打包后的名字为main.js
>         sub:'./index.js'//打包后的名字为sub.js
> 	}, 
>     output: {
>         filename: "[name].js",//打包后的文件名 通过占位符来动态的修改多个文件的打包名字
>         path: path.resolve(__dirname,'dist')//设置打包后的路径地址，这里的路径一定是绝对路径
>     }
> }
> ```

+ 模式 ***mode***默认**production**

> ~~~javascript
> const path =require('path')
> 
> module.exports={
>     mode:'development',//模式默认production
>     entry:'./index.js', //设置入口文件
>     // entry:{
>     //	main:'./index.js'
> 	//}, 
>     output: {
>         filename: "bundle.js",//打包后的文件名
>         path: path.resolve(__dirname,'bundle')//设置打包后的路径地址，这里的路径一定是绝对路径
>     }
> }
> ~~~
>
>

+ 模块打包 ***module***  

~~~javascript
module.exports={
    entry:'./src/index.js', //设置入口文件
    output: {
        filename: "bundle.js",//打包后的文件名
        path: path.resolve(__dirname,'dist')//设置打包后的路径地址，这里的路径一定是绝对路径
    },
    module: {
        rules: [
            {
                test: /\.jpg/, //匹配规则
                use:{
                    loader: "file-loader"//帮助webpack打包一些webpack无法打包的文件
                }
            }
        ]
    }
}
~~~

+ 打包图片及一些静态文件的**file-loader** 

  处理当遇到打包文件里有匹配到规则的文件时，先将这个文件根据loader打包到**制定目录下比如dist**，生成对应的文件，再将**文件地址**返回给原来文件的所处位置

  > ~~~javascript
  > module.exports={
  >     mode:'development',//模式默认production
  >     entry:'./src/index.js', //设置入口文件
  >     output: {
  >         filename: "bundle.js",//打包后的文件名
  >         path: path.resolve(__dirname,'dist')//设置打包后的路径地址，这里的路径一定是绝对路径
  >     },
  >     module: {
  >         rules: [
  >             {
  >                 test: /\.jpg/, //匹配规则
  >                 use:{
  >                     loader: "file-loader",
  >                     options: {//配置打包后文件的配置参数
  >                         name:'[name].[ext]',//保持原来的文件名
  >                         outputPath:'images/'//输出的打包文件夹地址
  >                     }
  >                 }
  >             }
  >         ]
  >     }
  > }
  > ~~~
  >
  > ![1560415523155](C:\Users\ZX50V\AppData\Roaming\Typora\typora-user-images\1560415523155.png)
  >
  >
  >

+ **url-loader** 能实现file-loader的所有功能

  > 但是url-loader打包图片是吧他打压成base64码放入bundle.js文件中而不是生成单独的图片
  >
  > + 优点：省了http请求不需要在请求图片
  > + 缺点：如果图片很大，那么你的js文件也会很大，加载速度也就变慢，页面会陷入短暂的白屏
  > + ![1560416800680](C:\Users\ZX50V\AppData\Roaming\Typora\typora-user-images\1560416800680.png)

  + **如果设置limit属性**

    > limit:204800 //相当于小于200kb则不会打包生成另一个文件，若大于200kb则会生成制定要求的文件

+ 打包css的loader  **style-loader,css-loader**

  ~~~javascript
              {
                  test: /\.css/, //匹配规则
                  use:['style-loader','css-loader'] //执行顺序是从右到左
              }
  
  ~~~

  + 如果要打包scss

  ~~~javascript
              {
                  test: /\.scss/, //匹配规则
                  use:['style-loader','css-loader','scss-loader','postcss-loader'] //执行顺序是从右到左
              }
  
  如果在一个scss文件中导入另一个scss文件，那么会导致webpack打包的时候第一个经过4个loader，第二则会只经过css-loader,style-loader
  
  解决办法
              {
                  test: /\.scss/, //匹配规则
                  use:[
                      'style-loader',
                      {
                          loader:'css-loader',
                          options:{
                              importLoader:2，//这个参数则会使另一个scss也经过下面2个loader
                              modules:true//使导入css文件变得模块化，在一个js文件中css样式不会被耦合
                          }
                      },
                      'scss-loader',
                      'postcss-loader'] //执行顺序是从右到左
              }
  ~~~

  + 打包字体文件eot|ttf|svg

  ~~~javascript
   			{
                  test: /\.(eot|ttf|svg)$/, //匹配规则
                  use:{
                      loader: "file-loader"
                  }
              }
  ~~~



  + postcss-loader自动帮你加上厂商前缀，例如-webkit-

  ~~~javascript
  cnpm i postcss-loader -D
  然后在项目下创建postcss.config.js配置文件
  
  module.exports = {
      parser: 'sugarss',
      plugins: {
          'postcss-import': {},
          'postcss-cssnext': {},
          'cssnano': {}
      }
  }
  
  或者安装autoprefixer插件
  cnpm i autoprefixer -D
  
  module.exports = {
      plugins: [
          require('autoprefixer')
      ]
  }
  
  webpack.config.js的module的rule中
              {
                  test: /\.css/, //匹配规则
                  use:['style-loader','css-loader','postcss-loader']
              }
  ~~~

+ plugins打包 

  plugin就是可以在webpack运行到某个时刻，帮助你做一些事情

  + htmlWebpackPlugin

    ~~~javascript
    cnpm i --save-dev html-webpack-plugin
    然后再webpack.config.js初始化htmlWebpackPlugin
    const HtmlWebpackPlugin = require('html-webpack-plugin');
    
    
        plugins: [
            new HtmlWebpackPlugin({
                template: "src/index.html" //以什么为模板 ，打包后就会生成该模板的html
            })
        ]
    ~~~

    > 作用：**会在打包结束后自动生成html文件，并将打包好的js文件自动引入到这个html文件中**

  + cleanWebpackPlugin

    ~~~javascript
    cnpm i clean-webpack-plugin -D
        plugins: [
            new HtmlWebpackPlugin({
                template: "src/index.html" //以什么为模板 ，打包后就会生成该模板的html
            }),
            new CleanWebpackPlugin(['dist'])//打包是会先删除dist目录然后再打包
        ]
    ~~~


+ devtool

  + source-map 打包后的文件跟源代码文件产生映射关系

  ~~~javascript
  module.exports={
      mode:'development',//模式默认production
      devtool:'source-map', //如果文件出错在控制台打印出来的错误就会指向原代码，而不是打包后的代码，并且在打包后的文件夹中会生成bundle.js.map，如果加上inline-source-map,打包后则不会在生成bundle.js.map文件，但是它会将映射关系变成base64编码形式放到bundle.js的底部。
      
      //如果是cheap-inline-source-map的话，他会提高性能，他只会告诉你那一行出错，但是inline-source-map会告诉你精确到哪一列出错
      
     	//如果是eval的参数的话，他的打包速度是最快的，性能最好，打包后的文件底部是通过eval形式及地址，指向具体的映射文件，但是比较复杂的业务的话，eval并不准确
      
      //如果使用devtool建议使用cheap-module-eval-source-map，提示错误比较全，速度也快
      
      //最后如果用的是development cheap-module-eval-source-map，
      //如果是production cheap-module-source-map
      entry:'./src/index.js', //设置入口文件
      output: {
          filename: "bundle.js",//打包后的文件名
          path: path.resolve(__dirname,'dist')//设置打包后的路径地址，这里的路径一定是绝对路径
      },
      module: {
          rules: [
              {
                  test: /\.jpg/, //匹配规则
                  use:{
                      loader: "file-loader",
                      options: {//配置打包后文件的配置参数
                          name:'[name].[ext]',//保持原来的文件名
                          outputPath:'images/'//输出的打包文件夹地址
                      }
                  }
              }
          ]
      }
  }
  ~~~


+ webpackDevServer

  + 第一种

    ~~~javascript
    //第一种自动监听变化的做法
    //修改package.json scripts标签
    //“watch”:"webpack --watch" //每次修改后会重新打包
    ~~~

  + 第二种

  + 如果用webpack-dev-server，打包后会没有dist目录，是因为将dist目录放到内存中，这样会使重新打包的速度更快

    ~~~javascript
    cnpm install webpack-dev-server -D
    //package.json
    "start":"webpack-dev-server"
    
    //webpack.config.js
        devServer: {
          contentBase:'./dist',  //服务器的根路径
          open:true, //自动打开浏览器
          proxy:{
              '/api':'http://localhost:3000'//配置跨域
          }
        },
    ~~~

  + webpack-dev-server中的**热跟新** ，指页面不刷新，但是数据更新了 **第一种写法**

  ~~~javascript
    //
  const webpack=require('webpack') //第一步
  
  
  devServer: {
        contentBase:'./dist',  //服务器的根路径
        open:true, //自动打开浏览器
        proxy:{
            '/api':'http://localhost:3000'//配置跨域
        },
          hot:true,//启动热更新，数据改变了，页面不刷新，但是数据更新  第二部
          hotOnly:true//即使热跟新未开启，页面也不自动刷新
      },
          
     // plugins里要
      new webpack.HotModuleReplacementPlugin() //第三部
  ~~~

  + 第二种写法

  ~~~javascript
  
  ~~~
