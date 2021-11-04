---
description: >-
  本文档将详细介绍前端函数如何进行二次开发，并辅以样例进行说明；适合无任何V平台前端函数二次开发经验的开发人员进行阅读。本文以将金额转换成中文为例（453.564转换为：肆佰伍拾叁元伍角陆分肆毫），详细阐述整个开发过程。
---

# 函数插件开发

## 开发流程

&#x20;        在进行前端函数二次开发之前，首先需要了解前端函数二次开发的开发流程，以便于在整体轮廓上理解二次开发流程。如下图

![函数开发流程图](<../../../.gitbook/assets/image (8).png>)

## 元数据编辑

&#x20;       在二次开发函数前，首先需要明确当前函数需求，确定当前函数名称、入参及返回值等信息。当以上信息固化下来后，函数元信息已经完成，以下为元数据详细描述，其内容格式：

```bash
{
    groupId:"com.yindangu.vplatform",//组织id
    code:"moneyToChinese",//插件编号
    plugins:[{
        type:"function",//插件类型
        code:"moneyToChinese",//函数编号
        name:"金额转大写汉字",//函数名称
        entry:"com.yindangu.client.function.moneyToChinese",//函数主入口
        desc:"将金额转换成大写汉字，如：453.564 -> 肆佰伍拾叁元伍角陆分肆毫",//函数描述
        author:"",//开发者
        example:"moneyToChinese(514.24)",//使用样例
        defineUrl:"./moneyToChinese.js",//函数定义文件url
        debugUrl:"",//函数测试验证文件url
        inputs:[{//函数入参信息定义,可选
            type:"number",//入参类型，
            desc:"金额",//入参描述，
            required:true//是否必填
        }],
        output:{//可选
            type:"char",//函数输出类型,
            desc:""//函数输出描述
        }
    }]
}

```

元数据文件名称固定为manifest.json，其中各部分定义如下：

### plugins定义

定义函数基本信息，属性值为数组，详细描述如下：

| 编码        | 类型     | 必填    | 默认值         | 描述                                                      |
| --------- | ------ | ----- | ----------- | ------------------------------------------------------- |
| type      | 枚举     | 是     |             | 函数为function                                             |
| code      | String | 是     |             | <p>函数编码，应全局保持唯一，本文中为：</p><p>moneyToChinese</p>          |
| name      | String | 是     |             | 函数名称，应简要明确说明用途                                          |
| entry     | String | 是     |             | 函数入口方法，本文中为：com.yindangu.client.function.moneyToChinese |
| defineUrl | String | 是     | ./define.js | 函数定义脚本文件url，相对于元数据文件路径                                  |
| desc      | String | 否     |             | 函数描述，详细描述函数用途                                           |
| author    | String | 否     |             | 函数开发者                                                   |
| example   | String | 否     |             | 函数使用样例                                                  |
| debugUrl  | String | 否     | ./debug.js  | 函数测试脚本文件url                                             |

### inputs定义

定义函数入参，属性值为数组，详细描述如下：

| 编码        | 类型      | 必填 | 默认值   | 描述                                                                                                                                                                                               |
| --------- | ------- | -- | ----- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| type      | 枚举      | 是  |       | <p>入参类型，当枚举值为range时，则使用typeRange属性值中指定的范围</p><p>枚举项：</p><p>char-文本，</p><p>text-长文本，</p><p>number-小数，</p><p>boolean-布尔，</p><p>date-日期，</p><p>longDate-长日期，</p><p>integer-整型,</p><p>range-指定范围</p> |
| typeRange | Array   | 否  |       | 指定类型范围、入参类型可以为指定范围中的任意一种，枚举项为上述type枚举值，除去range                                                                                                                                                   |
| index     | String  | 否  |       | 参数配置适用下标范围，采用数学区间表示,如\[0,3)；如未设置，则使用当前配置所在inputs属性值中的下标值                                                                                                                                         |
| desc      | String  | 否  |       | 入参描述,详细描述入参信息                                                                                                                                                                                    |
| required  | Boolean | 否  | false | 是否必填                                                                                                                                                                                             |

### output定义

定义函数输出信息，属性值为Object，详细描述如下：



| 编码   | 类型     | 必填 | 默认值 | 描述                                                                                                                                 |
| ---- | ------ | -- | --- | ---------------------------------------------------------------------------------------------------------------------------------- |
| type | String | 是  |     | <p>输出类型:</p><p>char-文本，</p><p>text-长文本，</p><p>number-小数，</p><p>boolean-布尔，</p><p>date-日期，</p><p>longDate-长日期，</p><p>integer-整型</p> |
| desc | String | 否  |     | 函数输出描述，详细描述函数输出                                                                                                                    |

## 本地开发

&#x20;       抛开V平台函数二次开发规范，在本地需要实现此需求，我们该怎么做？ 首先新建一个js文件（如：moneyToChinese.js），在js文件中定义一个方法，在方法体中实现需求，如下所示：

```bash
var moneyToChinese = function(money){
    //这里编写实现逻辑，并返回结果
}
```

&#x20;        如此，该需求实现完毕。现在深入思考下，上面存在什么问题？ 因moneyToChinese定义为全局方法，有可能会被其他引入的第三方js库或其他开发人员编写的代码给覆盖掉，因此平台建议给函数添加命名空间，命名空间规范建议使用java的包名规范。现在我们改造下上述代码：

```bash
//命名空间为：com.yindangu.client.function
com = this.com||{};
com.yindangu = com.yindangu||{};
com.yindangu.client = com.yindangu.client||{};
com.yindangu.client.function = com.yindangu.client.function||{};
com.yindangu.client.function.moneyToChinese = function(money){
    //这里编写实现逻辑，并返回结果
}
```

&#x20;      至此，整个函数逻辑开发完毕。

## 本地验证

&#x20;     沿用普通js验证方法，首先创建一个html文件（如：index.html），引入上述新建的js文件，编写验证逻辑进行验证。

```bash
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>函数验证</title>
        <script src="./moneyToChinese.js"></script>
        <script>
            var res = com.yindangu.client.function.moneyToChinese(456.14);
            console.log(res);
        </script>
    </head>
    <body>
    </body>
</html>
```

## 函数部署

&#x20;       函数部署前，需要将元数据manifest.json文件及函数定义js文件打包成zip，**其中manifest.json文件需放置在根目录下**，其他文件可以自定义目录，在元数据中填入相对路径即可。

![函数压缩包](<../../../.gitbook/assets/image (4).png>)

&#x20;       此时函数二次开发工作已完成，然后需要发布插件，操作请参考《发布插件》一节

## 函数安装

&#x20;       函数部署完毕后，函数已提交到平台仓库受控，开发系统中还未安装该函数，此时需要使用开发系统中开始-》安装构件-》搜索（搜索值为插件编号）-》安装，如下图：

![函数安装](<../../../.gitbook/assets/image (3).png>)

待提示构件安装成功后，构件安装完成。

## 函数使用

&#x20;       待开发系统安装完成后，二次开发函数的使用方法与平台内部提供的函数一样，在表达式编辑器中直接使用，如下图：

![](<../../../.gitbook/assets/image (5).png>)

## 高阶晋级

### nodejs环境

&#x20;       随着nodejs在前端开发中盛行，以下介绍如何在nodejs环境中二次开发函数。相较于普通方式二次开发函数，在nodejs环境中开发函数多了一个步骤，即将源码打包成适合在浏览器中运行的脚本。

首先按照nodejs规范创建nodejs插件，编写源码。如下图：

![](<../../../.gitbook/assets/image (2).png>)

使用打包插件打包源码，本文已rollup为例，配置如下：

```bash
import babel from "rollup-plugin-babel";
import { terser } from 'rollup-plugin-terser';
// rollup.config.js
export default {
  input: 'src/moneyToChineseFunc.js',//源码主入口路径
  output: {
    file: 'dist/moneyToChineseFunc.js',//打包输出路径
    format:'umd',//编译出umd格式
    name:'com.yindangu.client.function',//定义全局命名空间
    sourcemap:true
  },
  plugins: [
    babel({ runtimeHelpers: true }),//babel转换
    terser()//脚本压缩
  ]
};
```

&#x20;       配置详细含义请前往rollup官网查看，待打包完成后，调整manifest.json文件，将函数定义中defineUrl执行打包结果文件即可。

### 集成使用vds

&#x20;       平台提供一系列接口供二次开发调用，接口列表参考[前端vds使用指南](https://yindangu.gitbook.io/v-devsuite/kuo-zhan-kai-fa/vdevsuite-cha-jian-kai-fa-zhi-nan/qian-duan-jin-jie-kai-fa/qian-duan-vds-shi-yong-zhi-nan)。二次开发在使用前，需先将接口命名空间引入，如需要使用平台数据源相关接口，需如下引入：

```bash
vds.import("vds.ds.*");
```

引入完成后，在函数中即可使用vds.ds命名空间，如根据数据源编号获取数据源实例

```bash
var ds = vds.ds.lookup("user");//返回数据源实例
```

函数在使用平台vds提供的接口后，本地验证需要有所调整：

1、引入vds

```bash
<script src="http://localhost:8080/module-operation!executeOperation?operation=vds-sdk-js"></script>
```

其中http://localhost:8080为V平台引用服务地址。

2、验证逻辑执行时机调整

```bash
vds.config({
	debug: true,
	import: [ "vds.ds.*"]
}).ready(function () {
	//这里编写验证逻辑
});
```

完整的验证页面如下：

```bash
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>函数验证</title>
        <script src="http://localhost:8080/module-operation!executeOperation?operation=vds-sdk-js"></script>
        <script src="./dist/moneyToChineseFunc.js"></script>
        <script>
            vds.config({
                debug: true,
                import: [ "vds.ds.*"]
            }).ready(function () {
                var res = com.yindangu.client.function.moneyToChinese(456.14);
                console.log(res);
            });
        </script>
    </head>
    <body>
    </body>
</html>
```

### 函数验证增强

&#x20;       vds提供了mock命名空间，方便开发人员对函数进行验证，mock提供init接口，以函数元数据作为入参，根据元信息随机生成函数入参数据，具体使用如下：

```bash
vds.config({
    debug: true,
    import: [ "vds.mock.*"]
}).ready(function () {
    vds.mock.init("../manifest.json").then(function(mock){
        mock.get("moneyToChinese").then(function(functionMock){
            var result = functionMock.exec();
            console.log(result);
        });
    });
});
```

上述使用到的vds接口详细信息请参考[vds使用指南](https://yindangu.gitbook.io/v-devsuite/kuo-zhan-kai-fa/vdevsuite-cha-jian-kai-fa-zhi-nan/qian-duan-jin-jie-kai-fa/qian-duan-vds-shi-yong-zhi-nan)。
