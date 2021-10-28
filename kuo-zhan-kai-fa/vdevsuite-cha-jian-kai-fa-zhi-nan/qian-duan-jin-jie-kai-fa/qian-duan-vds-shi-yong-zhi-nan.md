---
description: 平台提供一系列接口供二次开发人员调用，方便开发人员使用平台现有接口，提升开发效率。
---

# 前端vds使用指南

## 使用规范

在使用vds提供的接口前，需要先将vds接口引入，引入规范如下：

```
//引入vds命名空间下所有接口
vds.import("vds.ds.*");
//引入多个vds命名空间
vds.import("vds.ds.*","vds.mock.*");
```

## 验证准备

二次开发使用vds提供的接口后，验证时需要先初始化vds，如下：

```
<script src="http://localhost:8080/module-operation!executeOperation?operation=vds-sdk-js"></script>
<script>
	vds.config({
		debug: true,
		import: ["vds.mock.*", "vds.ds.*","vds.test.*"]//额外引入vds命名空间
	}).ready(function () {
		//这里编辑验证逻辑
	});
</script>
```

其中http://localhost:8080为平台执行系统服务地址。

vds详细接口说明请参考：[前端vds说明文档](http://vstore-doc.yindangu.com/jsdoc/index.html)
