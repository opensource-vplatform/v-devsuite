# FAQ

## 1、发布的构件不生效，优先检查构件时间戳是否相同

a)每个错误必须的第一项检查：构件是否发布到执行系统。构件时间戳是否相同就表示构件系统

打开本地的jar下的META-INF/MANIFEST.MF 的Bnd-LastModified属性值，对照系统构件的时间戳

![本地构件](<../../.gitbook/assets/image (39).png>)

打开执行系统的本地构件管理，点击对应的构件，显示详细信息

![系统生效的构件](<../../.gitbook/assets/image (37).png>)

## 2、引用第三方jar，运行时报ClassNotFoundException.

a)每个错误必须的第一项检查：构件是否发布到执行系统。构件时间戳是否相同就表示构件系统：

b)检查云编译时的jar是否有包含lib目录，并且第三方的jar也在里面。

c)检查本地构件的信息是否有包含第三方的依赖。

![依赖了第三方的jar](<../../.gitbook/assets/image (38).png>)

d)检查发布到执行的信息是否有包含第三方的依赖。

![依赖第3方属性信息](<../../.gitbook/assets/image (40).png>)
