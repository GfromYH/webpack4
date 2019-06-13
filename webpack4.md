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
>     // entry:{
>     //	main:'./index.js'
> 	//}, 
>     output: {
>         filename: "bundle.js",//打包后的文件名
>         path: path.resolve(__dirname,'bundle')//设置打包后的路径地址，这里的路径一定是绝对路径
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






