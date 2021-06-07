
# html-widthsource-loader

webpack结合html-webpack-plugin使用时，html中直接使用img标签src加载图片的话，图片的路径并不会被webpack处理。直接其实不光img标签，video、audio标签中的src也不会被webpack处理。 

解决上面的问题可以通过html-loader处理html文件，这往往需要在js中引入html，而这又是我不想解决问题的方式。

于是在借鉴了社区的解决方案[html-withimg-loader](https://www.npmjs.com/package/html-withimg-loader)后，在此之上优化解决了上面的问题，使得外部资源会被打包，而且路径也处理妥当。
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

## options

|Name|Type|Default|Description|
|----|-----|-----|-----|
|min|Boolean|true|删除html中的换行符，压缩html代码|
|exclude|String|undefined|排除要处理的资源，{exclude: '.mp3'}|
|deep|Boolean|true|是否使用#include加载模板|


## 更新记录：

|版本号|描述|备注|
|----|----|----|
|v1.0.0|发版||
|v1.0.1|修复#include模板引用问题|
