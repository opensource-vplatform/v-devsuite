---
description: 前端插件的开发快速入门介绍
---

# 开发前端插件

进行开发前，请保证开发环境已经准备好，环境配置请查看**《准备环境》**一节

## **编写业务代码**

根据package.json 配置，业务代码在 src/main.js为主文件

```javascript
/*
 * 删除指定实体的当前行记录
 * 规则编号：DeleteListCurrentRow
 * 规则配置信息:
 * 1、entityCode 实体编号
 */
vds.import("vds.ds.*"); //引用了vds工具包的ds模块
let evaluate = function (ruleContext) {
    return new Promise((resolve,reject)=>{
		//获取实体编号配置
		let entityCode = ruleContext.getInput("entityCode");
		if(!entityCode ){
			reject(Error("未配置实体编号，请检查!"));
		}else{
			//获取实体实例
			let entity = vds.ds.lookup(entityCode);
			if(entity ){
				let record = entity.getCurrentRecord();
				if(record){
					entity.deleteRecordByIds([record.getSysId()]);
				}
				resolve();
			}else{
				reject(Error("未找到实体，将检查实体编号是否正确!编号：" + entityCode ));
			}
		}
	});
};

export {
    evaluate
}
```

## **编译生成define.js**

```javascript
npm install
rollup -c rollup.config.js
```

编译后的目标文件生成在dist/bundle.js路径，如果不使用rollup，可手工编写define.js文件（标准的js文件）。

## **编写元数据文件**

前端需要手工编写manifest.json文件

```javascript
{
  "groupId":"com.yindangu.vplatform.client.rule",
  "code":"DeleteListCurrentRow",
  "plugins":[{
    "type":"rule",
    "scope":"client",
    "code":"DeleteListCurrentRow",
    "name":"删除实体中当前行记录",
    "desc":"删除实体中当前行记录",
    "entry":"com.yindangu.rule.demo.evaluate",
    "defineUrl":"./dist/define.js",//目标文件（必须）
    "debugUrl":"", //测试文件（可选）
    "inputs":[{
        "code":"entityCode",
        "name":"实体编号",
        "desc":"选择实体",
        "editor":{
          "type":"expression",
          "required":true
        }
    }]
  }]
}
```

## **打包文件格式**

将以上两个文件（manifest.json和define.js）打包压缩成zip格式的压缩包，其中manifest.json必须放置在压缩包的根目录下，定义文件（define.js）目录根据manifest.json中defineUrl属性值决定。上述配置压缩包目录结构如下：

![前端格式](../../../.gitbook/assets/jar-user3.png)

根据manifest.json的defineUrl配置， 文件define.js在dist目录

## **发布构件**

在开发系统上传，过程与后端相同

## **使用构件**

在开发系统上传，过程与后端相同

## 函数实例

```javascript
/*
 * 数字转大写
 * 函数编号：NumToChineseFunc
 * 函数配置信息:
 * 1、num 数字
 */
vds.import("vds.ds.*");
//代码如下所示：
function convertCurrency(money) {
	//汉字的数字
	var cnNums = new Array('零', '壹', '贰', '叁', '肆', '伍', '陆', '柒', '捌', '玖');
	//基本单位
	var cnIntRadice = new Array('', '拾', '佰', '仟');
	//对应整数部分扩展单位
	var cnIntUnits = new Array('', '万', '亿', '兆');
	//对应小数部分单位
	var cnDecUnits = new Array('角', '分', '毫', '厘');
	//整数金额时后面跟的字符
	var cnInteger = '整';
	//整型完以后的单位
	var cnIntLast = '元';
	//最大处理的数字
	var maxNum = 999999999999999.9999;
	//金额整数部分
	var integerNum;
	//金额小数部分
	var decimalNum;
	//输出的中文金额字符串
	var chineseStr = '';
	//分离金额后用的数组，预定义
	var parts;
	if (money == '') {
		return '';
	}
	money = parseFloat(money);
	if (money >= maxNum) {
		//超出最大处理数字
		return '';
	}
	if (money == 0) {
		chineseStr = cnNums[0] + cnIntLast + cnInteger;
		return chineseStr;
	}
	//转换为字符串
	money = money.toString();
	if (money.indexOf('.') == -1) {
		integerNum = money;
		decimalNum = '';
	} else {
		parts = money.split('.');
		integerNum = parts[0];
		decimalNum = parts[1].substr(0, 4);
	}
	//获取整型部分转换
	if (parseInt(integerNum, 10) > 0) {
		var zeroCount = 0;
		var IntLen = integerNum.length;
		for (var i = 0; i < IntLen; i++) {
			var n = integerNum.substr(i, 1);
			var p = IntLen - i - 1;
			var q = p / 4;
			var m = p % 4;
			if (n == '0') {
				zeroCount++;
			} else {
				if (zeroCount > 0) {
					chineseStr += cnNums[0];
				}
				//归零
				zeroCount = 0;
				chineseStr += cnNums[parseInt(n)] + cnIntRadice[m];
			}
			if (m == 0 && zeroCount < 4) {
				chineseStr += cnIntUnits[q];
			}
		}
		chineseStr += cnIntLast;
	}
	//小数部分
	if (decimalNum != '') {
		var decLen = decimalNum.length;
		for (var i = 0; i < decLen; i++) {
			var n = decimalNum.substr(i, 1);
			if (n != '0') {
				chineseStr += cnNums[Number(n)] + cnDecUnits[i];
			}
		}
	}
	if (chineseStr == '') {
		chineseStr += cnNums[0] + cnIntLast + cnInteger;
	} else if (decimalNum == '') {
		chineseStr += cnInteger;
	}
	return chineseStr;
}
let evaluate = function (num) {
	var data = convertCurrency(num);
	return data;
}

export {
	evaluate
}
```

元数据说明：

```javascript
{
  "type": "function",
  "scope": "client",
  "code": "NumToChineseFunc",
  "name": "数字转大写汉字(前端)",
  "desc": "数字转大写汉字(前端)",
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
```

## 规则实例

```javascript
  /*
 * 数字转汉字
 * 规则编号：NumToChineseRule
 * 规则配置信息:
 * 1、entityCode 实体编号
 */
vds.import("vds.ds.*");
function convertCurrency(money) {
	//汉字的数字
	var cnNums = new Array('零', '壹', '贰', '叁', '肆', '伍', '陆', '柒', '捌', '玖');
	//基本单位
	var cnIntRadice = new Array('', '拾', '佰', '仟');
	//对应整数部分扩展单位
	var cnIntUnits = new Array('', '万', '亿', '兆');
	//对应小数部分单位
	var cnDecUnits = new Array('角', '分', '毫', '厘');
	//整数金额时后面跟的字符
	var cnInteger = '整';
	//整型完以后的单位
	var cnIntLast = '元';
	//最大处理的数字
	var maxNum = 999999999999999.9999;
	//金额整数部分
	var integerNum;
	//金额小数部分
	var decimalNum;
	//输出的中文金额字符串
	var chineseStr = '';
	//分离金额后用的数组，预定义
	var parts;
	if (money == '') {
		return '';
	}
	money = parseFloat(money);
	if (money >= maxNum) {
		//超出最大处理数字
		return '';
	}
	if (money == 0) {
		chineseStr = cnNums[0] + cnIntLast + cnInteger;
		return chineseStr;
	}
	//转换为字符串
	money = money.toString();
	if (money.indexOf('.') == -1) {
		integerNum = money;
		decimalNum = '';
	} else {
		parts = money.split('.');
		integerNum = parts[0];
		decimalNum = parts[1].substr(0, 4);
	}
	//获取整型部分转换
	if (parseInt(integerNum, 10) > 0) {
		var zeroCount = 0;
		var IntLen = integerNum.length;
		for (var i = 0; i < IntLen; i++) {
			var n = integerNum.substr(i, 1);
			var p = IntLen - i - 1;
			var q = p / 4;
			var m = p % 4;
			if (n == '0') {
				zeroCount++;
			} else {
				if (zeroCount > 0) {
					chineseStr += cnNums[0];
				}
				//归零
				zeroCount = 0;
				chineseStr += cnNums[parseInt(n)] + cnIntRadice[m];
			}
			if (m == 0 && zeroCount < 4) {
				chineseStr += cnIntUnits[q];
			}
		}
		chineseStr += cnIntLast;
	}
	//小数部分
	if (decimalNum != '') {
		var decLen = decimalNum.length;
		for (var i = 0; i < decLen; i++) {
			var n = decimalNum.substr(i, 1);
			if (n != '0') {
				chineseStr += cnNums[Number(n)] + cnDecUnits[i];
			}
		}
	}
	if (chineseStr == '') {
		chineseStr += cnNums[0] + cnIntLast + cnInteger;
	} else if (decimalNum == '') {
		chineseStr += cnInteger;
	}
	return chineseStr;
}
let NumToChineseRule = function (ruleContext) {
    return new Promise((resolve,reject)=>{
		//获取规则输入参数
		let num1 = ruleContext.getInput("price");
		let num2 = ruleContext.getInput("sum");
		try{
			var output = ruleContext.newOutput();
			let datas = ruleContext.getInput("entity");
			let field = ruleContext.getInput("field");
			let outField = ruleContext.getInput("outField");
			if(datas && field){
				var ds = ruleContext.getVObject().getInput("entity");
				var updateRecords = [];
				var outRecords = [];
				for(var i = 0,len = datas.length;i<len;i++){
					var map = {
						id:datas[i].id
					}
					map[outField] = convertCurrency(datas[i][field]);
					updateRecords.push(map);
					var od = {}
					for(var key in datas[i]){
						od[key] = datas[i][key];
					}
					od[outField] = convertCurrency(od[field]);
					outRecords.push(od)
				}
				//更新到输入实体
				ds.updateRecords(updateRecords);
				//设置到输出实体
				output.set("entity", outRecords);
			}
			//设置规则输出参数
			output.set("price_cn", convertCurrency(num1)).set("sum_cn", convertCurrency(num2));

			resolve();
		}catch(e){
			reject(e);
		}
	});
};

export {
	NumToChineseRule
}
```

元数据描述

```javascript
{
      "type": "rule",
      "scope": "client",
      "code": "NumToChineseRule",
      "catalog": "other",
      "name": "数字转大写汉字(前端规则)",
      "desc": "数字转大写汉字(前端规则)",
      "transactionType": "none",
      "entry": "com.yindangu.rule.demo.common.NumToChineseRule",
      "defineUrl": "./client-demo/rule/dist/NumToChineseRule.js",
      "debugUrl": "./client-demo/rule/debug/rule.js",
      "inputs": [
        {
          "code": "price",
          "name": "单价",
          "type": "number",
          "desc": "转换前的单价数值",
          "default": null,
          "editor": {
            "type": "expression"
          }
        },
        {
          "code": "sum",
          "name": "总价",
          "type": "number",
          "desc": "转换的总价数值",
          "default": null,
          "editor": {
            "type": "expression"
          }
        },
        {
          "code": "entity",
          "name": "实体",
          "type": "entity",
          "desc": "转换的实体数值",
          "default": null,
          "editor": {
            "type": "entity"
          },
          "fields": [
            {
              "code": "id",
              "name": "编码",
              "type": "char",
              "desc": "编码数据"
            },
            {
              "code": "sum",
              "name": "总价",
              "type": "integer",
              "desc": "总价数据"
            },
            {
              "code": "sum_cn",
              "name": "总价（汉字）",
              "type": "char",
              "desc": "总价数据"
            }
          ]
        },
        {
          "code": "field",
          "name": "字段编码",
          "type": "char",
          "desc": "需要转换成大写金额的字段编码",
          "default": null,
          "editor": {
            "type": "expression"
          }
        },
        {
          "code": "outField",
          "name": "字段编码",
          "type": "char",
          "desc": "转换成大写金额保存的字段编码",
          "default": null,
          "editor": {
            "type": "expression"
          }
        }
      ],
      "outputs": [
        {
          "code": "price_cn",
          "type": "char",
          "name": "单价",
          "desc": "转换后的单价数值"
        },
        {
          "code": "sum_cn",
          "type": "char",
          "name": "总价",
          "desc": "转换后的总价数值"
        },
        {
          "code": "entity",
          "name": "实体",
          "type": "entity",
          "desc": "转换的实体数值",
          "default": null,
          "editor": {
            "type": "entity"
          },
          "fields": [
            {
              "code": "id",
              "name": "编码",
              "type": "char",
              "desc": "编码数据"
            },
            {
              "code": "sum",
              "name": "总价",
              "type": "integer",
              "desc": "总价数据"
            },
            {
              "code": "sum_cn",
              "name": "总价（汉字）",
              "type": "char",
              "desc": "总价数据"
            }
          ]
        }
      ]
    }
```

## 控件实例

```javascript
isc.ClassFactory.defineClass("JGDevTextBox", "DynamicForm");
isc.JGDevTextBox.addProperties({
	Alias: "文本",
	MultiHeight: 30,
	MultiWidth: 235,
	Left : 50,
	datasource:null,
	fieldCode:null,
	TitleWidth:76,
	Top : 50,
	OnLabelClick: null,
	OnKeyDown: null,
	OnLeave: null
});
isc.JGDevTextBox.addMethods({
	init: function () {
		this.titleWidth = this.TitleWidth;
		this.left = this.Left;
		this.top = this.Top;
		this.width = this.MultiWidth;
		this.height = this.MultiHeight;
		this.enabled = !this.Disabled;
		this.valuesManager = isc.ValuesManager.getByDatasource(this.datasource);
		this.items = [{
			width:"*",
			title : this.Alias,
			type : "text",
			name : this.fieldCode,
			titleClick : this.OnLabelClick,
			//keyDown : this.OnKeyDown,
			blur : this.handleBlur
		}];
		return this.Super("init", arguments);
	},
	handleBlur : function(){
		this.form.valuesManager.saveData();
		if(this.OnLeave){
			this.OnLeave();
		}
	}
});
```

元数据描述

```javascript
{
  "type": "widget",
  "icon": "./client-demo/widget/JGDevTextBox/define/JGDevTextBox.png",
  "code": "JGDevTextBox",
  "name": "二次开发文本",
  "desc": "简单的二次开发文本控件",
  "defineUrl": "./client-demo/widget/JGDevTextBox/define/JGDevTextBox.js",
  "visible": true,
  "properties": [
    {
      "code": "Alias",
      "name": "标题",
      "desc": "文本中显示的标题",
      "default": "按钮",
      "type": "char",
      "editor": {
        "type": "text",
        "placeholder": "请输入标题"
      },
      "access": {
        "rule": {
          "readable": true,
          "writable": true
        },
        "webDesigner": {
          "readable": true,
          "writable": true
        }
      }
    },
    {
      "code": "Top",
      "name": "上边距",
      "desc": "上边距",
      "type": "integer",
      "default": 0,
      "editor": {
        "type": "top"
      }
    },
    {
      "code": "Left",
      "name": "左边距",
      "desc": "左边距",
      "type": "integer",
      "default": 0,
      "editor": {
        "type": "left"
      }
    },
    {
      "code": "MultiWidth",
      "name": "宽度",
      "desc": "宽度",
      "type": "integer",
      "default": 50,
      "editor": {
        "type": "width"
      }
    },
    {
      "code": "MultiHeight",
      "name": "高度",
      "desc": "高度",
      "default": 50,
      "type": "integer",
      "editor": {
        "type": "height"
      }
    },
    {
      "code": "datasource",
      "name": "数据源",
      "desc": "绑定数据源",
      "default": null,
      "type": "char",
      "editor": {
        "type": "entity"
      }
    },
    {
      "code": "fieldCode",
      "name": "字段",
      "desc": "绑定的数据源字段",
      "default": null,
      "type": "char",
      "editor": {
        "type": "field",
        "entityProp": "datasource"
      }
    },
    {
      "code": "TitleWidth",
      "name": "标题宽度",
      "desc": "标题宽度",
      "default": 76,
      "type": "integer",
      "editor": {
        "type": "integer",
        "min": 0
      }
    },
    {
      "code": "OnLabelClick",
      "name": "标题点击事件",
      "type": "funtion",
      "desc": "点击标题时触发",
      "editor": {
        "type": "ruleset"
      }
    },
    {
      "code": "OnKeyDown",
      "name": "键盘按下事件",
      "type": "funtion",
      "desc": "键盘按下时触发",
      "editor": {
        "type": "ruleset"
      }
    },
    {
      "code": "OnLeave",
      "name": "焦点离开事件",
      "type": "funtion",
      "desc": "焦点离开时触发",
      "editor": {
        "type": "ruleset"
      }
    },
    {
      "code": "Disabled",
      "name": "禁用",
      "type": "boolean",
      "desc": "禁用控件",
      "editor": {
        "type": "boolean"
      },
      "access": {
        "rule": {
          "readable": true,
          "writable": true
        },
        "webDesigner": {
          "readable": true,
          "writable": true
        }
      }
    }
  ]
}
```

\
demo实例代码 <a href="demo-shi-li-dai-ma" id="demo-shi-li-dai-ma"></a>
------------------------------------------------------------------

‌下载[demo代码](http://download.yindangu.com/yindangu-plugin/plugin-demo/20210430/94plugin-demo-client.zip)
