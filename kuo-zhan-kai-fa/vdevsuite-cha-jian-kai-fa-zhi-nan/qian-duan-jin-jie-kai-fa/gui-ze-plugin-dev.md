---
description: >-
  本文档将详细介绍前端规则如何进行二次开发，并辅以样例进行说明；适合无任何V平台前端规则二次开发经验的开发人员进行阅读。本文以将金额转换成中文为例（453.564转换为：肆佰伍拾叁元伍角陆分肆毫），详细阐述整个开发过程。
---

# 规则插件开发

## 开发流程

&#x20;       在进行前端规则二次开发之前，首先需要了解前端规则二次开发的开发流程，以便于在整体轮廓上理解二次开发流程。如下图

![规则开发流程](<../../../.gitbook/assets/image (8).png>)

## 元数据定义

&#x20;       在二次开发规则前，首先需要明确当前规则需求，确定当前规则名称、配置及返回值等信息。当以上信息固化下来后，规则元信息已经完成，以下为元数据详细描述，其内容格式如下：

```
{
    groupId:"com.yindangu.client.rule",//组织id
    code:"moneyToChinese",//插件编号
    plugins:[{
			"type":"rule",//插件类型，规则为rule
			"code":"moneyToChineseRule",//规则编号
			"catalog":"other",//规则分类
			"name":"数字转大写汉字(前端规则)",//规则名称
			"desc":"数字转大写汉字(前端规则)",//规则描述
			"transactionType":"none",//是否有事务
			"entry":"com.yindangu.client.rule.moneyToChineseRule",//规则主入口
			"defineUrl":"./define.js",//规则定义脚本url
			"debugUrl":"./debug.js",//调试脚本url
			"inputs":[{//规则输入信息
				"code":"money",//输入编号
				"name":"单价",//输入名称
				"type":"number",//输入类型
				"desc":"转换前的单价数值",//输入描述
				"default":null,//默认值
				"editor":{//输入编辑器
					"type":"expression"//编辑器类型
				}
			}],
			"outputs":[{
				"code":"out",
				"type":"char",
				"name":"转换结果",
				"desc":"转换后结果"
			}]
	}]
}
```

元数据文件名称固定为manifest.json，其中各部分定义如下：

### plugins属性

定义控件基本信息，详情如下：

| 编码              | 值类型    | 必填 | 默认值         | 描述                                                                                                                                                                                         |
| --------------- | ------ | -- | ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| type            | Enum   | 是  |             | 插件类型，规则为rule                                                                                                                                                                               |
| code            | String | 是  |             | 规则编码，应全局保持唯一                                                                                                                                                                               |
| name            | String | 是  |             | 规则名称，应简要明确说明用途                                                                                                                                                                             |
| entry           | String | 是  |             | 规则主入口                                                                                                                                                                                      |
| desc            | String | 否  |             | 规则描述，详细描述规则用途                                                                                                                                                                              |
| author          | String | 否  |             | 规则开发者                                                                                                                                                                                      |
| scope           | String | 否  | client      | 规则应用范围，枚举值：client（客户端）、server（服务端）                                                                                                                                                         |
| catalog         | Enum   | 否  | other       | <p>规则所属目录，枚举值：</p><p>count（计算、赋值）、</p><p>check（检查、提示）、</p><p>control（控件操作）、</p><p>entity（实体操作）、</p><p>query（筛选、查询、搜索）、</p><p>import（数据导入、导出）、</p><p>database（数据库操作）、</p><p>other（其他操作）</p> |
| transactionType | String | 否  | none        | <p>事务类型,枚举值:</p><p>transaction：有事务，如保存数据到数据库或修改数据库记录等等</p><p>none：无事务，如提示信息、设置控件属性等等</p><p>unknown：未知，不能明确当前规则是否有事务，如执行方法</p>                                                              |
| defineUrl       | String | 否  | ./define.js | 规则定义脚本文件url                                                                                                                                                                                |
| debugUrl        | String | 否  |             | 规则测试脚本文件url                                                                                                                                                                                |

### inputs属性

&#x20;       inputs属性值为数据组，方便输入配置在编辑器中显示时进行排序，该属性为可选属性，如未设置，则代表规则无任何配置信息；inputs属性值中每一个数组元素定义一个规则输入配置，其中规则输入配置包括以下信息：

| 编码      | 值类型    | 必填 | 默认值  | 描述                                                                                                                                                                            |
| ------- | ------ | -- | ---- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| code    | String | 是  |      | 规则输入配置编码，在规则中应保持唯一                                                                                                                                                            |
| name    | String | 是  |      | 规则输入配置名称，应简要明确说明输入配置用途                                                                                                                                                        |
| type    | Enum   | 否  | text | <p>输入配置类型：</p><p>char-文本，</p><p>text-长文本，</p><p>number-小数，</p><p>boolean-布尔，</p><p>date-日期，</p><p>longDate-长日期，</p><p>integer-整型, </p><p>entity-实体</p>                        |
| desc    | String | 否  |      | 规则输入配置描述，详细描述规则输入配置用途                                                                                                                                                         |
| default | Any    | 否  | null | 规则输入配置默认值                                                                                                                                                                     |
| editor  | Object | 否  | null | [属性编辑器信息](https://yindangu.gitbook.io/v-devsuite/kuo-zhan-kai-fa/vdevsuite-cha-jian-kai-fa-zhi-nan/qian-duan-jin-jie-kai-fa/shu-xing-bian-ji-qi-ding-yi)，目前只支持expression属性编辑器 |

#### 编辑器定义信息(editor)

&#x20;       编辑器信息，描述当前属性使用何种编辑器及编辑器元信息；该属性为可选属性，如未设置，则使用文本编辑器编辑；详细参考[属性编辑器定义](https://yindangu.gitbook.io/v-devsuite/kuo-zhan-kai-fa/vdevsuite-cha-jian-kai-fa-zhi-nan/qian-duan-jin-jie-kai-fa/shu-xing-bian-ji-qi-ding-yi)；

### outputs属性

属性值为Array,数组中每个数组项详情定义如下:

| 编码     | 值类型    | 默认值  | 描述                                                                                                                                                   |
| ------ | ------ | ---- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| code   | String |      | 输出编码                                                                                                                                                 |
| name   | String |      | 输出名称，应简要明确说明输出用途                                                                                                                                     |
| type   | String | null | <p>输出类型：</p><p>char-文本，</p><p>text-长文本，</p><p>number-小数，</p><p>boolean-布尔，</p><p>date-日期，</p><p>longDate-长日期，</p><p>integer-整型, </p><p>entity-实体</p> |
| desc   | String |      | 规则属性描述，详细描述控件属性用途                                                                                                                                    |
| fields | Array  |      | 当输出类型为entity时,使用此属性定义实体列信息                                                                                                                           |

## 本地开发

在本地开发前，首先需要了解平台对规则主入口方法定义规范：

1、规则主入口方法入参为RuleContext实例，其定义参考[RuleContext接口说明文档](http://vstore-doc.yindangu.com/jsdoc/index.html)

2、规则主入口方法返回值Promise实例，平台会解决其浏览器兼容性问题

3、规则主入口方法需要定义命名空间，防止被其他javascript给覆盖，命名空间规范建议使用java包命名规范

了解平台规范后，规则实现如下：

```
//命名空间为：com.yindangu.client.rule
com = this.com||{};
com.yindangu = com.yindangu||{};
com.yindangu.client = com.yindangu.client||{};
com.yindangu.client.rule = com.yindangu.client.rule||{};
com.yindangu.client.rule.moneyToChinese = function(ruleContext){
    return new Promise(function(resolve,reject){
        try{
            //获取配置信息
            var money = ruleContext.getInput("money");
            //这里编写实现逻辑，并将转换结果赋值给res
            var res = ;
            var output = ruleContext.newOuput();
            //设置返回值
            output.set("out",res);
            resolve();    
        }catch(e){
            reject(e);
        }  
    });
}
```

## 本地验证

平台vds提供了mock命名空间，方便开发人员对规则进行验证，mock提供init接口，以规则元数据作为入参，根据规则元信息随机生成规则配置数据，具体使用如下：

```
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>函数验证</title>
        <script src="http://localhost:8080/module-operation!executeOperation?operation=vds-sdk-js"></script>
        <script src="./define.js"></script>
        <script>
            vds.config({
                debug: true,
                import: [ "vds.mock.*"]
            }).ready(function () {
                vds.mock.init("../manifest.json").then(function(mock){
                    mock.get("moneyToChinese").then(function(ruleMock){
                        var result = ruleMock.exec();
                        result.then(function(){
                            console.log("规则执行成功！");
                        }).catch(function(e){
                            console.log("规则执行失败！错误信息："+e.message);
                        });
                    });
                });
            });
        </script>
    </head>
    <body>
    </body>
</html>
```

其中http://localhost:8080为执行系统服务地址，define.js为规则定义脚本文件。

## 规则部署

&#x20;       规则部署前，需要将元数据manifest.json文件及规则定义js文件打包成zip，其中manifest.json文件需放置在根目录下，其他文件可以自定义目录，在元数据中填入相对路径即可。

![规则压缩包](https://gblobscdn.gitbook.com/assets%2F-MKdnMpEXew2dgfykkLs%2F-MZRHDcUwKMzrU3CFjj2%2F-MZRHqu4bpxc-sVeI8f\_%2Fimage.png?alt=media\&token=5f9ecf02-b440-4f89-9fba-1d05af47e371)

&#x20;       此时规则二次开发工作已完成，进入开发系统（开发系统使用参考这里），在开始-》扩展管理-》上传中，选择规则压缩包即可，如下图：

![规则部署](https://gblobscdn.gitbook.com/assets%2F-MKdnMpEXew2dgfykkLs%2F-MZRI-pVTRR1BShvh5cM%2F-MZRJgelG6oBavhO9LdY%2Fimage.png?alt=media\&token=129a4f19-a633-42a2-bc64-daa0c10fc3b6)

## 规则安装

&#x20;       规则部署完毕后，函数已提交到平台仓库受控，开发系统中还未安装该规则，此时需要使用开发系统中开始-》安装构件-》搜索（搜索值为插件编号）-》安装，如下图：

![规则安装](<../../../.gitbook/assets/image (9).png>)

待提示构件安装成功后，构件安装完成。

## 规则使用

&#x20;       待开发系统安装完成后，二次开发规则的使用方法与平台内部提供的规则一样，在方法编辑器中直接选取使用，如下图：

![](<../../../.gitbook/assets/image (10).png>)
