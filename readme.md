
# html-widthsource-loader

webpack处理资源无往不利，但有个问题总是很苦恼，html中直接使用img标签src加载图片的话，因为没有被依赖，图片将不会被打包。其实不光img标签，video、audio标签中的src也不会被webpack处理。 

html-loader处理html文件需要在js中引入html，而这恰恰又是我不想实现的方式。

于是在借鉴了社区的解决方案后，通过这个loader解决上面的问题，外部资源会被打包，而且路径也处理妥当。额外提供html的include子页面功能。

______________

## 安装

    npm install html-widthsource-loader --save

## 使用

    var html = require('html-widthsource-loader!../xxx.html');

或者写到配置中：

    loaders: [
        {
            test: /\.(htm|html)$/i,
            loader: 'html-widthsource-loader'
        }
    ]

额外支持一项黑科技：

    <div>
        #include("./layout/top.html")
    </div>

子页面将被引入，并且子页面中的img标签同样会进行处理。该语法支持嵌套。

如：xxx.html:

    <!DOCTYPE html>
    <html>
    <head>
    #include("./handlebars/layout.html")
    <title>示例页面</title>
    </head>
    <body>
    <img id='test2' src='images/logo.gif' />
    <img id="test1" src="./images/test4.jpg" />
    #include("./handlebars/scripts.html")
    </body>
    </html>

编译结果为：

    <!DOCTYPE html><html><head><meta name="description" content=""><meta name="keywords" content=""><meta charset="utf-8"><meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1"><title>示例页面</title></head><body><img id='test2' src="/__build/images/logo_f7b644e2086e41139fa132fd229450f4.gif" /><img id="test1" src="/__build/images/test4_df538a9696eb4c032286f7f6bc0d1708.jpg" /><script src="https://static.yiji.com/resource/lib/jquery/1.11.2/jquery.min.js"></script></body></html>

结合html-webpack-plugin，在入口html中也可以使用img标签加载图片，不会打包失败

plugins:[
    new HtmlWebpackPlugin({
        template: 'html-widthsource-loader!'+path.resolve(srcDir, 'xxx.html'),
        filename: 'xxx.html'
    })
]


**注意，必须请求虚拟目录/__build/下的html才是处理后的html，新手可能不知道这个，如：127.0.0.1/__build/xxx.html**

github地址：https://github.com/wzsxyz/html-widthsource-loader

## 支持查询参数：

* query.exclude 匹配该参数的图片路径不进行处理。例如：

    {test: /\.html$/, loader: 'html-widthsource-loader?exclude=/upload/'},

则如：src="/upload/head.png"这个图片路径将不会被处理。暂不支持正则表达式字符串。

* query.min 默认会去除html中的换行符，配置min=false可不去除
* query.deep deep=false将关闭include语法嵌套子页面的功能

    require('html-widthsource-loader?min=false!xxx.html');

## 更新记录：

|版本号|描述|备注|
|----|----|----|
|v1.0.0|发版||
