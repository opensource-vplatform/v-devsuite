---
description: 属性编辑器定义为用户在开系统中使用前端二次开发插件（如：规则、控件等），编辑插件属性时所使用属性编辑器的元数据，本文将详细说明平台目前所支持的属性编辑器。
---

# 属性编辑器定义

### 编号编辑器

编号编辑器会根据当前上下文自动生成窗体内唯一的编号;如按钮控件，窗体中已存在编号为JGButton1的按钮控件后，用户添加一个按钮到窗体中时，该按钮的编号自动为JGButton2。

### 属性列表

| 编码   | 类型     | 必填 | 默认值  | 描述    |
| ---- | ------ | -- | ---- | ----- |
| type | String | 是  | code | 编辑器类型 |

### 使用样例

```
{
    type："code"
}
```

### 界面效果

![](<../../../.gitbook/assets/image (23).png>)

## 文本编辑器

### 属性列表

| 编码       | 类型      | 必填 | 默认值   | 描述    |
| -------- | ------- | -- | ----- | ----- |
| type     | String  | 是  | text  | 编辑器类型 |
| required | Boolean | 否  | false | 是否必填  |

### 使用样例

```
{
    type："text",
    required:true
}
```

### 界面效果

![](<../../../.gitbook/assets/image (31).png>)

## 日期编辑器

### 属性列表

| 编码       | 类型      | 必填 | 默认值   | 描述    |
| -------- | ------- | -- | ----- | ----- |
| type     | String  | 是  | date  | 编辑器类型 |
| required | Boolean | 否  | false | 是否必填  |

### 使用样例

```
{
    type："date",
    required:true
}
```

### 界面效果

![](<../../../.gitbook/assets/image (21).png>)

## 日期时间编辑器

### 属性列表

| 编码       | 类型      | 必填 | 默认值      | 描述    |
| -------- | ------- | -- | -------- | ----- |
| type     | String  | 是  | dateTime | 编辑器类型 |
| required | Boolean | 否  | false    | 是否必填  |

### 使用样例

```
{
    type："dateTime",
    required:true
}
```

### 界面效果

![](<../../../.gitbook/assets/image (21).png>)

## 布尔编辑器

### 属性列表

| 编码   | 类型     | 必填 | 默认值     | 描述    |
| ---- | ------ | -- | ------- | ----- |
| type | String | 是  | boolean | 编辑器类型 |

### 使用样例

```
{
    type："boolean"
}
```

### 界面效果

![](<../../../.gitbook/assets/image (29).png>)

## 整数编辑器

### 属性列表

| 编码   | 类型     | 必填 | 默认值     | 描述    |
| ---- | ------ | -- | ------- | ----- |
| type | String | 是  | integer | 编辑器类型 |

### 使用样例

```
{
    type："integer"
}
```

### 界面效果

![](<../../../.gitbook/assets/image (19).png>)

## 小数编辑器

### 属性列表

| 编码   | 类型     | 必填 | 默认值   | 描述    |
| ---- | ------ | -- | ----- | ----- |
| type | String | 是  | float | 编辑器类型 |

### 使用样例

```
{
    type："float"
}
```

### 界面效果

![](<../../../.gitbook/assets/image (32).png>)

## 下拉选择编辑器

### 属性列表

| 编码       | 类型      | 必填 | 默认值    | 描述    |
| -------- | ------- | -- | ------ | ----- |
| type     | String  | 是  | select | 编辑器类型 |
| options  | Array   | 是  | null   | 候选项   |
| required | Boolean | 否  | false  | 是否必填  |

其中options为数组，其每个数组元素定义如下：

| 编码    | 类型     | 必填 | 默认值 | 描述  |
| ----- | ------ | -- | --- | --- |
| value | Any    | 是  |     | 标识值 |
| label | String | 是  |     | 显示值 |

### 使用样例

```
{
    type:"select",
    required:true,
    options:[{
        value: 'New York',
        label: 'New York'
    },{
        value: 'London',
        label: 'London'
    }]
}
```

### 界面效果

![](<../../../.gitbook/assets/image (33).png>)

## 单选组编辑器

### 属性列表

| 编码       | 类型      | 必填 | 默认值        | 描述    |
| -------- | ------- | -- | ---------- | ----- |
| type     | String  | 是  | radioGroup | 编辑器类型 |
| options  | Array   | 是  | null       | 候选项   |
| required | Boolean | 否  | false      | 是否必填  |

其中options为数组，其每个数组元素定义如下：

| 编码    | 类型     | 必填 | 默认值 | 描述  |
| ----- | ------ | -- | --- | --- |
| value | Any    | 是  |     | 标识值 |
| label | String | 是  |     | 显示值 |

### 使用样例

```
{
    type:"radioGroup",
    required:true,
    options:[{
        value:"male",
        label:"男"
    },{
        value:"female",
        label:"女"
    }]
}
```

### 界面效果

![](<../../../.gitbook/assets/image (18).png>)

## 宽度编辑器

### 属性列表

| 编码       | 类型      | 必填 | 默认值   | 描述    |
| -------- | ------- | -- | ----- | ----- |
| type     | String  | 是  | width | 编辑器类型 |
| required | Boolean | 否  | true  | 是否必填  |
| disabled | Array   | 否  | null  | 禁用选项  |

宽度编辑器为定制编辑器，由单选按钮组及输入框组成，参考界面效果图；单选按钮组由三个候选项组成：

1、空间自适应（space）2、内容自适应（content）3、固定值（\*px）

ps：括号中的值为标识值；其中\*为整数值

当用户选择固定值时，输入框变为可编辑状态，否则处于禁用状态。当disabled属性存在值时，代表禁用指定候选项；

### 使用样例

```
{
    type:"width",
    required:true,
    disabled:["apace"]
}
```

### 界面效果

![](<../../../.gitbook/assets/image (22).png>)

## 高度编辑器

### 属性列表

| 编码       | 类型      | 必填 | 默认值    | 描述    |
| -------- | ------- | -- | ------ | ----- |
| type     | String  | 是  | height | 编辑器类型 |
| required | Boolean | 否  | true   | 是否必填  |
| disabled | Array   | 否  | null   | 禁用选项  |

高度编辑器为定制编辑器，由单选按钮组及输入框组成，参考界面效果图；单选按钮组由三个候选项组成：

1、空间自适应（space）2、内容自适应（content）3、固定值（\*px）

ps：括号中的值为标识值；其中\*为整数值

当用户选择固定值时，输入框变为可编辑状态，否则处于禁用状态。当disabled属性存在值时，代表禁用指定候选项；

### 使用样例

```
{
    type:"height",
    required:true,
    disabled:["space"]
}
```

### 界面效果

![](<../../../.gitbook/assets/image (20).png>)

## 实体选择器

### 属性列表

| 编码          | 类型      | 必填 | 默认值    | 描述    |
| ----------- | ------- | -- | ------ | ----- |
| type        | String  | 是  | entity | 编辑器类型 |
| required    | Boolean | 否  | true   | 是否必填  |
| placeholder | String  | 否  | 请选择实体  | 占位提示  |

### 使用样例

```
{
    "type":"entity",
    "required":true
}
```

### 界面效果

![](<../../../.gitbook/assets/image (27).png>)

## 字段选择器

### 属性列表

| 编码          | 类型      | 必填 | 默认值   | 描述     |
| ----------- | ------- | -- | ----- | ------ |
| type        | String  | 是  | field | 编辑器类型  |
| entityProp  | String  | 是  |       | 实体属性编号 |
| required    | Boolean | 否  | true  | 是否必填   |
| placeholder | String  | 否  | 请选择字段 | 占位提示   |

字段选择器在使用时，需先选择实体；entityProp属性值为控件绑定实体属性编码，其规范为\[parent.]\[控件属性编码],parent代表父控件，例：parent.parent.TableName,代表关联父控件的父控件的TableName属性。如果实体关联属性未设置，应提示：

![](<../../../.gitbook/assets/image (16).png>)

点击前往选择后，跳转到对应的控件属性设置。

### 使用样例

```
{
    type:"field",
    entityProp:"TableName"
}
```

### 界面效果

![](<../../../.gitbook/assets/image (28).png>)

## 实体&字段选择器

### 属性列表

| 编码          | 类型      | 必填 | 默认值         | 描述    |
| ----------- | ------- | -- | ----------- | ----- |
| type        | String  | 是  | entityField | 编辑器类型 |
| required    | Boolean | 否  | true        | 是否必填  |
| placeholder | String  | 否  | 请选择字段       | 占位提示  |

### 使用样例

```
{
    type:"entityField"
}
```

### 界面效果

![](<../../../.gitbook/assets/image (24).png>)

## 方法选择器

### 属性列表

| 编码          | 类型      | 必填 | 默认值     | 描述    |
| ----------- | ------- | -- | ------- | ----- |
| type        | String  | 是  | ruleset | 编辑器类型 |
| required    | Boolean | 否  | false   | 是否必填  |
| placeholder | String  | 否  | 请选择字段   | 占位提示  |

### 使用样例

```
{
    type:"ruleset"
}
```

### 界面效果

![](<../../../.gitbook/assets/image (17).png>)

## 资源选择器

### 属性列表

| 编码          | 类型      | 必填 | 默认值      | 描述    |
| ----------- | ------- | -- | -------- | ----- |
| type        | String  | 是  | resource | 编辑器类型 |
| required    | Boolean | 否  | true     | 是否必填  |
| placeholder | String  | 否  | 请选择字段    | 占位提示  |

### 使用样例

```
{
    type:"resource"
}
```

### 界面效果

![](<../../../.gitbook/assets/image (25).png>)

## 左边距编辑器

### 属性列表

| 编码   | 类型     | 必填 | 默认值  | 描述    |
| ---- | ------ | -- | ---- | ----- |
| type | String | 是  | left | 编辑器类型 |

继承自整数编辑器，单独抽提是左边距编辑器是为了方便平台识别出控件中哪个属性属于左边距，在web窗体设计器中，用户移动了控件，平台能根据该信息去调整控件中对应属性值。

### 使用样例

```
{
    type:"left"
}
```

### 界面效果

![](<../../../.gitbook/assets/image (26).png>)

## 上边距编辑器

### 属性列表

| 编码   | 类型     | 必填 | 默认值  | 描述    |
| ---- | ------ | -- | ---- | ----- |
| type | String | 是  | left | 编辑器类型 |

继承自整数编辑器，单独抽提是上边距编辑器是为了方便平台识别出控件中哪个属性属于上边距，在web窗体设计器中，用户移动了控件，平台能根据该信息去调整控件中对应属性值。

### 使用样例

```
{
    type:"top"
}
```

### 界面效果

![](<../../../.gitbook/assets/image (30).png>)

## 表达式编辑器

### 属性列表

| 编码          | 类型      | 必填 | 默认值   | 描述    |
| ----------- | ------- | -- | ----- | ----- |
| type        | String  | 是  | left  | 编辑器类型 |
| required    | boolean | 否  | false | 是否必填  |
| placeholder | String  | 否  | 请输入   | 占位提示  |

### 使用样例

```
{
    type："expression",
    placeholder:"我是标题",
    required:true
}
```

### 界面效果

暂未提供







