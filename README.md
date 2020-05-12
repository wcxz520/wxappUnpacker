# wxappUnpacker123

Wechat App(微信小程序, .wxapkg)解包及相关文件(.wxss, .json, .wxs, .wxml)还原工具

## 教程22222

[微信小程序“反编译”实战（一）：解包](https://mp.weixin.qq.com/s/GnHzJWYFJUXYuvYq6Cb_FA)

[微信小程序“反编译”实战（二）：源码还原](https://mp.weixin.qq.com/s/O3Ix91GkkS0ollYK_s6e_Q)

## 执行

> 下载此工具包执行：

````
npm install
````

> 再执行以下命令

````
node wuWxapkg.js [-d] <path/to/.wxapkg>
````


## 当前功能如下

- `node wuConfig.js <files...>` 将 app-config.json 中的内容拆分到各个文件对应的 .json 和 app.json , 并通过搜索 app-config.json 所在文件夹下的所有文件尝试将 iconData 还原为 iconPath 。
- `node wuJs.js <files...>` 将 app-service.js (或小游戏中的 game.js ) 拆分成一系列原先独立的 javascript 文件，并使用 Uglify-ES 美化，从而尽可能还原编译前的情况。
- `node wuWxml.js [-m] <files...>` 将编译/混合到 page-frame.html ( 或 app-wxss.js ) 中的 wxml 和 wxs 文件还原为独立的、未编译的文件。如果加上`-m`指令，就会阻止`block`块自动省略，可能帮助解决一些相关过程的 bug 。
- `node wuWxss.js <dirs...>` 通过获取文件夹下的 page-frame.html ( 或 app-wxss.js ) 和其他 html 文件的内容，还原出编译前 wxss 文件的内容。
- `node wuWxapkg.js [-d] <files...>` 将 wxapkg 文件解包，并将包中上述命令中所提的被编译/混合的文件自动地恢复原状。如果加上`-d`指令，就会保留编译/混合后所生成的新文件，否则会自动删去这些文件。同时，前面命令中的指令也可直接加在这一命令上。

### wxapkg 包的获取

Android 手机最近使用过的微信小程序所对应的 wxapkg 包文件都存储在特定文件夹下，可通过以下命令查看：

    adb pull /data/data/com.tencent.mm/MicroMsg/{User}/appbrand/pkg

其中`{User}` 为当前用户的用户名，类似于 `2bc**************b65`。

### 另注

所有命令上都可以使用`-f`指令来提高一定的并行度，但输出信息会混乱。

### 局限

- 实现中很多功能基于特定的版本(`wcc-v0.6vv_20180111_fbi`, 且不考虑面向低版本适配)和字符串搜索，所以不能很好的适应各种特殊情况。
- wxml 文件拥有不同于 xml 和 html 文件的字符转义规则，且尚未公开(并非"没有")，因此未能很好的还原相关内容。
- js 文件被压缩后会丢失原始变量名等信息内容无法还原；wxss 文件压缩后的注释也会丢失。
- wxs 文件会将所有的变量如 Math 改为 nv_Math ，这里仅通过字符串替换去除。
- 一些被引用 wxss 文件本身的源文件丢失，因此无法恢复原始目录。
- 有些项目开启了难以复原的`es6转es5`选项，检验本项目结果是否正确时需要关闭项目中的`es6转es5`选项。
- wxml 中一些无法找到正向语句的内容无法还原。

## 依赖

这些 node.js 程序除了自带的 API 外还依赖于以下包:
[cssbeautify](https://github.com/senchalabs/cssbeautify)、[CSSTree](https://github.com/csstree/csstree)、[VM2](https://github.com/patriksimek/vm2)、[Esprima](https://github.com/jquery/esprima)、[UglifyES](https://github.com/mishoo/UglifyJS2/tree/harmony)

您需要安装这些包才能正确执行这些程序，为了做到这一点，您可以执行以下命令:

    npm install esprima -g
    npm install css-tree -g
    npm install cssbeautify -g
    npm install vm2 -g
    npm install uglify-es -g

此外，这些 node.js 程序之间也有一定的依赖关系，比如他们都依赖于 wuLib.js 。

## 参考

这些实现除了参考微信小程序开发文档、 wxapkg 文件解包后的内容以及通过开发者工具编译的一些 wxml 外，还参考了一些 github 上的相关内容的分析( [unwxapkg.py](https://gist.github.com/feix/32ab8f0dfe99aa8efa84f81ed68a0f3e)、[wechat-app-unpack](https://github.com/leo9960/wechat-app-unpack/) )，在此感谢他们。

另外，如果您对本程序的一些具体实现细节感兴趣，可以参考 [DETAILS.md](https://github.com/qwerty472123/wxappUnpacker/blob/master/DETAILS.md) 。
