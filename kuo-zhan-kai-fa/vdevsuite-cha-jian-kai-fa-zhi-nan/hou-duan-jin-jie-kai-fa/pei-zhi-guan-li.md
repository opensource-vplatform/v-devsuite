---
description: 实际开发中可能需要根据不同的配置实现不同的逻辑，服务运行过程中，配置可以动态修改，我们在这个模块中提供这样的功能
---

# 配置管理

首先，在插件注册器（IRegisterPlugin）中描述一个配置文件，参考如下样例代码的81行：

```java
package com.toone.v3.platform.function;

import com.yindangu.v3.plugin.vds.reg.api.IRegisterPlugin;
import com.yindangu.v3.plugin.vds.reg.api.builder.IFunctionBuilder;
import com.yindangu.v3.plugin.vds.reg.api.model.IComponentProfileVo;
import com.yindangu.v3.plugin.vds.reg.api.model.IFunctionProfileVo;
import com.yindangu.v3.plugin.vds.reg.api.model.IPluginProfileVo;
import com.yindangu.v3.plugin.vds.reg.api.model.VariableType;
import com.yindangu.v3.plugin.vds.reg.common.RegVds;

import java.util.ArrayList;
import java.util.List;

/**
 * 函数注册器
 * 
 * @Author xugang
 * @Date 2021/6/1 16:04
 */
public class LogRegister implements IRegisterPlugin {

    private static final String Component_Code = "Serverfunc_LogFunc";
    private final static String Group_Id = "com.toone.v3.platform";
    private final static String Plugin_Author = "同望科技";
    public static final String Plugin_Code = "LogFunc";
    private static final String Plugin_Name = "打日志信息到控制台上";
    private static final String Plugin_Desc = "打日志信息到控制台上。";
    private static final String Component_Version = "3.10.0";

    @Override
    public IComponentProfileVo getComponentProfile() {
        return RegVds.getPlugin()
                .getComponentProfile()
                .setGroupId(Group_Id)
                .setCode(Component_Code)
                .setVersion(Component_Version)
                .build();
    }

    @Override
    public List<IPluginProfileVo> getPluginProfile() {
        List<IPluginProfileVo> plugins = new ArrayList<>();
        plugins.add(getFunc());

        return plugins;
    }

    /**
     * LogFunc函数
     *
     * @return LogFunc函数描述器
     */
    private IFunctionProfileVo getFunc() {
        IFunctionBuilder pluginBuilder = RegVds.getPlugin().getFunctiontPlugin();
        IFunctionProfileVo.IFunctionInputVo inputVo1 = pluginBuilder.newInput()
                .setDesc("日志信息")
                .setType(VariableType.Char)
                .setRequired(true)
                .build();
        IFunctionProfileVo.IFunctionInputVo inputVo2 = pluginBuilder.newInput()
                .setDesc("日志类型")
                .setType(VariableType.Char)
                .setRequired(true)
                .build();
        IFunctionProfileVo.IFunctionOutputVo outputVo = pluginBuilder.newOutput()
                .setDesc("结果")
                .setType(VariableType.Boolean)
                .build();
        pluginBuilder.setAuthor(Plugin_Author)
                .setCode(Plugin_Code)
                .setDesc(Plugin_Desc)
                .setName(Plugin_Name)
                .setEntry(LogFunc.class)
                .setExample("代码示例:LogFunc(\"aaaa\",\"error\") 返回值为 true。 \n" +
                        "参数1--日志信息（字符串类型）；\n" +
                        "参数2--日志类型（\"debug\",\"info\",\"warn\",\"error\"）；\n" +
                        "返回值为布尔类型。")
                .setOutput(outputVo)
                .addInputParam(inputVo1)
                .addInputParam(inputVo2)
                // 在这里描述配置文件的路径，改路径相对于jar包的跟路径
                .setConfigPath("./config.xml");

        return pluginBuilder.build();
    }
}

```

![](<../../../.gitbook/assets/1 (2).png>)

然后，就是在VDS接口中获取配置管理接口

```java
IPreferencesManager preferencesManager = VDS.getIntance().getPreferencesManager();
String outputToConsole = preferencesManager.getProperty("com.toone.v3.platform", "Serverfunc_LogFunc", funcCode, "outputToConsole");
```

最后，关于配置文件的格式，参考如下样例：

```markup
<?xml version="1.0" encoding="UTF-8"?>
<config title="后端日志函数配置">
	<property key="outputToConsole" des="信息是否输出到控制台，默认值false">false</property>
</config>
```
