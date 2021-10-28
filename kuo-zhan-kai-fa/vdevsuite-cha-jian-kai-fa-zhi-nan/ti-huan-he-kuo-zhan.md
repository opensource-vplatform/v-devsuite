---
description: V-DevSuite针对大多数通用场景已内置了一系列默认的规则、函数、控件插件，但实际应用中为了应对特定的需求，要对这些插件做替换或者扩展的处理。
---

# 替换和扩展

## 1. 名词解释

**替换**

对V-DevSuite已提供的默认规则、函数、控件插件的原实现逻辑进行调整，并进行覆盖性的替换。

多适用于对默认插件的业务逻辑进行按需定制的场景，替换对新、旧逻辑均生效，但是需要谨慎操作与广泛测试验证，保证改动后的向下兼容性，以免影响系统中使用了此插件的已有业务逻辑的运行。

**扩展**

参考V-DevSuite已提供的规则、函数、控件插件的源码，使用新的构件编码，插件编码开发新的插件。

多适用于实现业务定制类的逻辑，部分复用已有默认插件的代码逻辑，由于企业编码（groupId）、构件编码、插件编码均重新定义，新创建的插件与原有插件相互独立，互不影响，插件的定制逻辑仅在使用了此新插件的功能中生效。

## 2. 替换的详解

### 开发步骤

1、复制V-DevSuite的规则、函数、控件源码（参看【源码公开】章节）；

2、按需求调整代码；

3、提交插件构件（通过V-AppDesigner提交，请参看前面插件开发的章节）

### 开发规范

目前V-DevSuite提供的插件和所属构件都是一对一的关系（一个构件只包含一个插件）。如果实现插件替换，也需要遵循这个一对一的规范。

### **注意事项**

关键元信息不能修改，并且入参、出参都不能增加或减少，也不能改变类型。如果其中一些改变，就相当于**扩展模式**了。

机构id(groupId)、构件编码(component.code)、插件编码(plugin.code)不能修改，若修改了则被视为第三方插件，不会替代V-DevSuite的默认插件。

_后端插件示例：_

```java
	@Override
	public IComponentProfileVo getComponentProfile() {
		return RegVds.getPlugin().getComponentProfile()
				.setGroupId("com.yindangu.plugin")//机构id(groupId)不能修改
				.setCode("mydemo")//构件编码不能修改
				.build();
	}
	private IPluginProfileVo getFileStorageService() {
		IFileStorageServiceBuilder fss = RegVds.getPlugin().getFileStorageServicePlugin();
		IFileStorageServiceProfileVo vo = fss.setName("我的文件存储")
				.setCode(MyFileStorageService.FileStorage_Code) //插件编码不能修改
				.setDesc("我的文件存储服务") //只能修改描述的信息
				.setConfigPath("./config.xml")
				.setAuthor("徐刚")
				.setEntry(MyFileStorageService.class)
				.build();
		return vo;
	}
```

_前端插件示例：_

```javascript
{
  "groupId": "com.yindangu.plugin",//机构id(groupId)不能修改
  "code": "mydemo",//构件编码不能修改
  "plugins": [
    {
      "type": "function",
      "scope": "client",
      "code": "NumToChineseFunc",//插件编码不能修改
      "name": "数字转大写汉字(前端)",
      "desc": "数字转大写汉字(前端)",//只能修改描述的信息
      "entry": "com.yindangu.func.demo.evaluate",
      "defineUrl": "./client-demo/function/dist/NumToChineseFunc.js",
      "debugUrl": "./client-demo/function/debug/function.js",
      "inputs": [
        {
          "index": "",
          "type": "number",
          "desc": "转换的数字",
          "required": false,
          "default": null
        }
      ],
      "output": {
        "type": "char",
        "desc": "数字转大写汉字",
        "unknowType": false
      }
    }
 }
```

**已有属性不建议修改、删除**

不建议删除以及修改插件的已有的输入参数、返回值参数、控件属性，以免影响系统中已经使用此插件的业务逻辑。必要时，在保证业务逻辑向下兼容的前提下，可以新增参数、属性，建议参数名起特殊一点的前缀以免与V-DevSuite后续版本的参数冲突。

```java
/** 函数元信息(数字转汉字) */
	private IPluginProfileVo getNumberUpperFunc() {
		IFunctionBuilder bf = RegVds.getPlugin().getFunctiontPlugin();
		IPluginProfileVo p1 = bf.setCode(NumberUpperFunc.D_Code)
				.setName("数字转汉字-name")
				.setDesc("数字转汉字")
				.setAuthor("徐刚")
				.addInputParam(bf.newInput().setType(VariableType.Integer).setDesc("数字").build())  //参数不能修改
				.setEntry(NumberUpperFunc.class)
				.setOutput(bf.newOutput().setType(VariableType.Char).setDesc("汉字大写").build()) //参数不能修改
				.setExample("使用示例说明，\r\n换号,这是示例说明")//只能修改描述的信息
				.build();
		return p1;
	}
```

## 3. 扩展的详解

### 函数扩展

直接复制V-DevSuite已有插件的代码和元信息描述后，修改企业id(groupId)、构件编码(component.code)、插件编码(plugin.code)为新插件自定义的内容。

扩展的函数与平台的函数已经没有必然的关系，参数信息、返回值等都可以按需改造。

### 函数示例

函数比较简单，取得V-DevSuite的插件源码后，需要修改的地方为：

a) 修改pom.xml文件内容: groupId，artifactId，version等都需要修改。

b) 修改插件注册器: groupId，component.code，plugin.code。

c) 按需增加扩展参数：扩展方式参考前面章节的开发介绍及demo源码。

### 规则扩展

扩展V-DevSuite默认规则插件可以复用现有的配置界面，还可以增加额外参数配置项。原默认规则插件没有入参、返回值的，可通过扩展参数元信息声明新入参、返回值来实现。

取得V-DevSuite的插件源码后，需要修改的地方：

a) 修改pom.xml文件内容: groupId，artifactId，version等都要修改。

b) 修改插件注册器groupId，component.code，plugin.code。

c) 配置扩展平台规则属性：manifest.json（references属性）**【重要】**

references：说明扩展平台的具体规则

前端配置json(前后其实都一样，只是表现方式不同而已)

```java
reference:{
  groupId:"组织id，平台规则基本就是：com.toone.v3.platform"
  scope:"client",//可选，默认值为client，枚举值：client、server
  componentCode:"Webrule_AbortRule",//必填，构件编号
  pluginCode:"AbortRule"//必填，插件编号
},
```

完整的结构：

```java
[{
    type:"rule",//插件类型
    code:"",//规则编号
    name:"",//规则名称
    deprecated:false,//是否已弃用
    desc:"",//规则描述
    author:"",//开发者
    entry:"",//规则主入口
    scope:"",//规则应用范围
    catalog:"",//规则所属目录
    transactionType:"",//事务类型
    defineUrl:"",//规则定义脚本文件url
    debugUrl:""//规则测试脚本文件url
    reference:{//可选，基于已有规则扩展，目前只支持平台规则
        groupId:"com.toone.v3.platform",//必填，组织id
        scope:"client",//可选，默认值为client，枚举值：client、server
        componentCode:"Webrule_AbortRule",//必填，构件编号
        pluginCode:"AbortRule"//必填，插件编号
    },
    inputs:[{//使用数组，方便属性排序,可选
		code:"",//属性编号，
		name:""//属性名称，
        type:"",//属性类型
		desc:""//属性描述，
		default:null//默认值,
		editor:{//编辑器信息，可选
			type:"",//编辑器类型
		}
	}],
    outputs:[{
        code:""//规则输出编号,
        type:""//规则输出类型,
        name:"",//规则输出名称,
        desc:"",//规则输出描述,
        fields:[{//输出类型为entity时,此属性生效
            code:""//列编号,
            type:""//列类型,
            name:"",//列名称,
            desc:""//列描述
        }...]
    }]
}]
```



d) 增加扩展参数：扩展方式可参看前面章节的开发介绍及demo源码。

### 规则示例

如清除实体记录(ClearEntityData)的规则需要增加写操作日志标志参数，如果参数为true，就要记录数据被谁清除的记录。

1\) 修改pom.xml(只截取了需要修改的部分)

```markup
<groupId>com.mydemo</groupId>
<artifactId>demo_ClearEntityData</artifactId>
<version>3.4.0</version>
<description>后台规则-清除实体数据，并且记录清除人</description>
```

2\) 修改插件注册器ClearEntityRegister.java(增加入参和返回值，已经修改groupid等参数)，注意要设置扩展的规则信息

设置扩展的规则信息(前后端其实都一样，只是表现方式不同)：

```java
IRuleReferenceBuilder refBuilder = ruleBuilder.newReference()
	.setGroupId("com.toone.v3.platform")
	.setComponentCode("Serverrule_ClearEntityData")
	.setPluginCode("ClearEntityData");
```

```java
import java.util.Collections;
import java.util.List;

import com.yindangu.v3.plugin.vds.reg.api.IRegisterPlugin;
import com.yindangu.v3.plugin.vds.reg.api.builder.IRuleBuilder;
import com.yindangu.v3.plugin.vds.reg.api.builder.IRuleBuilder.IRuleReferenceBuilder;
import com.yindangu.v3.plugin.vds.reg.api.model.IComponentProfileVo;
import com.yindangu.v3.plugin.vds.reg.api.model.IPluginProfileVo;
import com.yindangu.v3.plugin.vds.reg.api.model.IRuleProfileVo;
import com.yindangu.v3.plugin.vds.reg.api.model.VariableType;
import com.yindangu.v3.plugin.vds.reg.common.RegVds;

/**
 * @Author xugang
 * @Date 2021/5/27 11:23
 */
public class ClearEntityRegister implements IRegisterPlugin {
	
    @Override
    public IComponentProfileVo getComponentProfile() {
        return RegVds.getPlugin()
                .getComponentProfile()
                .setGroupId("com.mydemo") //这3个元素按需要修改
                .setCode("demo")
                .setVersion("1.0.0")
                .build();
    }

    @Override
    public List<IPluginProfileVo> getPluginProfile() {
    	IPluginProfileVo pro = getRuleProfile();
        return Collections.singletonList(pro);
    }

 
    private IRuleProfileVo getRuleProfile() {
    	IRuleBuilder ruleBuilder = RegVds.getPlugin().getRulePlugin();
    	IRuleBuilder.IRuleInputBuilder rulePlog = ruleBuilder.newInput()
    			.setCode(ClearEntityData.D_PARAM_WRITELOG)
				.setName("需要写日志标志").setType(VariableType.Boolean);
    	IRuleBuilder.IRuleOutputBuilder ruleOut = ruleBuilder.newOutput()
    			.setCode(ClearEntityData.D_PARAM_ClearCount)
    			.setName("返回清除记录数")
    			.setType(VariableType.Integer);
    	
    	IRuleReferenceBuilder refBuilder = ruleBuilder.newReference()
    			.setGroupId("com.toone.v3.platform")
    			.setComponentCode("Serverrule_ClearEntityData")
    			.setPluginCode("ClearEntityData");
    	
    	ruleBuilder.setAuthor("jiqj")
                .setCode(ClearEntityData.D_RULE_CODE)//这3个元素按需要修改
                .setDesc(ClearEntityData.D_RULE_DESC)
                .setName(ClearEntityData.D_RULE_NAME)
                .setEntry(ClearEntityData.class)
                .addInput(rulePlog.build())//增加入参
                .addOutput(ruleOut.build()) //增加返回值
                .setReference(refBuilder.build()) //设置扩展的规则信息
                ;
    	
        return ruleBuilder.build();
    }
}

```

编译并安装到开发系统，会显示【原始配置】参数界面、【扩展配置】参数界面

![扩展配置](<../../.gitbook/assets/image (34).png>)

代码逻辑获取参数与demo一样，通过上下文对象获取：

后端：

```java
//获取扩展参数
Boolean writeLog = (Boolean)context.getInput(D_PARAM_WRITELOG);
if(writeLog!=null && writeLog) {
	clearCount = + 10000; 
}
//输出返回值
return context.newOutputVo()
		.put("clearCount", Integer.valueOf(clearCount))
		.put(Boolean.TRUE);
```

前端:

```javascript
var output = ruleContext.newOutputVo();
var clearCount =100;//获取扩展参数
var writelog = ruleContext.getInput("writelog");
if(writelog){
	clearCount = 999;
}//输出返回值
output.set("clearCount",clearCount);
```

## **4. 建议**

**若无强烈需要，建议优先选择扩展方式，次要选择替换方式。**

替换V-DevSuite的默认规则、函数、控件的需要重点关注以下事项：

1、替换默认插件需要全面了解已有插件的业务逻辑，改动的内容需要满足向下兼容，否则替换后容易由于兼容性问题导致系统处于不稳定或者错误状态；

2、已进行过替换的插件，无法享受原默认插件的后续升级优化的成果，替换的执行者如果考虑业务功能的长期演进，需要定期关注原插件源码的版本更新状况，作适当的手工代码合并升级；

3、需要关注复杂的版本控制，定制版与原版插件的版本（或时间戳）高低情况，存放定制版的项目与V-DevSuite平台项目的构件优先级设置情况，同项目或多项目间的主线、分支优先级设置情况（若分支主线存在差异的定制版插件），多种情况叠加后的排列组合场景可能会很多，出问题比较难跟进；

总之，替换模式能够做到新的需求改造可以无感替代现有逻辑，但后续维护成本颇高，除非是那种很少做升级的一次性项目，考虑长期维护演进的项目需要定制默认插件时需慎重考虑使用替换方式。

反观，扩展模式则无需关注原默认插件逻辑的向下兼容性及演进升级，因为扩展模式实际上是新开发的插件，与原插件完全脱钩，定制开发者只需要把注意力集中在新插件本身的功能及后续维护方案的制定即可以。

## 5. 版本控制策略

版本控制策略适合所有的构件，先说明一下日常工作会涉及的版本：

a) maven中的pom.xml的版本：这个版本号在二次开发插件的规范中并没有使用，因为考虑二次开发是开放型的，有些开发团队可能不是用maven管理jar，因此maven版本号是没有作用的。

b) 构件元信息版本：二次开发插件是以这个版本为准，如果没有填写默认是"1.0.0"。前端插件是在manifest.json文件中定义，后端插件是在IRegisterPlugin.getComponentProfile()代码方法中定义。

c) 构件编译时间戳：有本地编译原始jar时间，V-AppDesigner云编译时间。二次开发规范是以云编译时间为准。如果两个构件的版本号一样，就以最新云编译时间的构件为最高版本。

d) 清单引用项分支优先级: 当清单存在多个项目或分支的引用项，需要在vteam中配置优先级排序策略。

这4个版本项的优先级由高到低：

优先使用引用项分支优先级：如果a清单和b清单存在相同插件构件，a清单优先高于b清单，不管b清单构件版本号、时间戳是否高于a清单，都不会使用b清单的构件。

另外，构件元信息版本号第二优先级，构件编译时间戳第三优先级。

### 项目清单优先级

设置项目清单优先级的功能在vteam的清单管理:

![项目清单优先级](<../../.gitbook/assets/image (36).png>)

**注意：**只有引用了重复构件的情况才需要配置项目优先级，例如替换了平台的构件、或者引用两个项目包含了重复的构件，配置项目优先级才有意义。没有重复构件的项目不需要配置项目优先级。
