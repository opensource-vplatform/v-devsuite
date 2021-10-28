---
description: 文件处理模块包含两个接口IAppFileInfo（文件描述类）、IFileOperate（文件操作接口）
---

# 附件模块

```java
//获取文件操作接口：
IFileOperate operate = VDS.getIntance().getFileOperate();

//判断文件是否存在：
operate.containsFile("0001");

//删除文件：
operate.deleteFileInfo("0001");

//获取文件：
IAppFileInfo fileInfo = operate.getFileInfo("0001");

//保存文件：
IAppFileInfo fileInfo2 = VDS.getIntance().newAppFileInfo();
// 设置文件流
fileInfo2.setDataStream(new FileInputStream(new File("")));
// 设置文件类型
fileInfo2.setFileType("xml");
// 设置文件大小
fileInfo2.setFileSize(2021);
// 设置文件id，需保证唯一
fileInfo2.setId("0002");
// 设置原文件名，下载时使用
fileInfo2.setOldFileName("test.xml");
// 设置文件上传时间
fileInfo2.setUploadTime("2021-04-29 15:00:00");
operate.saveFileInfo(fileInfo2);

```
