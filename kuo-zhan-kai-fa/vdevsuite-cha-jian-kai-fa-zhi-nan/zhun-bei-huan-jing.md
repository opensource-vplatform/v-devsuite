# 准备环境

## **后台环境**

开发插件不需要指定IDE，遵从业界开发标准。唯一需要特别处理情况是：依赖了第3方的jar时，需要把这些jar文件复制到制品jar的lib目录（后面会有介绍）。

准备环境时需要注意两个地方：a)注意修改maven的setting文件，b)jdk版本，现在二次开发使用了jdk1.8.，以前V平台默认是1.6开发（运行时是使用jdk1.8）。

### **JDK版本**

开发插件要求jdk1.8版本

### 依赖模块说明

引用方式：目前支持两种，第一种是通过maven依赖进行引用；第二种是直接通过官网下载jar包进行本地引用。

* **plugin-business-api模块(必须)**

插件接口模块，提供插件开发需要实现的接口（interface）。

比如，规则插件需要实现IRule接口；函数插件需要实现IFunction接口；Command插件需要实现IHttpCommand接口。

V-DevSuite之后会陆续放出更多的插件供开发人员进行扩展实现。

* **plugin-register模块(必须)**

插件元信息注册模块，不同的插件会提供不一样的插件元信息（如：规则的输入、输出参数名称、编码、类型...）。每个插件会提供对应的插件元信息Builder API便于开发人员构造插件元信息，供返回给插件注册器（IRegisterPlugin的getPluginProfile方法）。

* **plugin-utils模块(可选)**

插件工具模块，如json处理、xml处理、加密、解密等常用工具类，供二次开发使用。

### 环境**配置**

可自由选择自己熟悉的IDE进行开发，可构建maven工程进行打包，也可按照普通java工程进行打包，最终生成标准jar包就可以。这里重点介绍本地jar包引用与maven项目的开发模式。

**本地开发模式： **需要下载系统提供的jar包，[plugin-business-api 下载](http://download.yindangu.com/yindangu-plugin/plugin-lib/20210930/plugin-business-api-3.3.0.jar)(必须)、[plugin-register 下载](http://download.yindangu.com/yindangu-plugin/plugin-lib/20210930/plugin-register-3.3.0.jar)(必须) 、[plugin-utils下载 (可选)](http://download.yindangu.com/yindangu-plugin/plugin-lib/20210930/plugin-utils-3.3.0.jar)，然后引入项目。_(注：有些浏览器如果点击无法下载，请右键选择“链接另存为”进行下载)_

**maven模式：**V-DevSuite已经把依赖的jar发布到银弹谷的maven仓库，所以需要追加配置银弹谷的maven仓库的settings.xml配置。

如使用命令行，就要_**配置maven**_的conf目录的settings，

或使用_**Eclipse 的maven的配置**_，就配置指定的settings文件。

打开settings.xml，找到profiles节点，复制下面的配置到profiles内。（没有 profiles 节点就创建节点）

```markup
<!-- 嵌入到maven的settings.xml 这部分是必须的 -->
<profile>
    <id>codingProxy</id>
    <activation>
        <activeByDefault>true</activeByDefault>
    </activation>
    <repositories>
        <repository>
            <id>yindangu-v-devsuite-sdk-maven</id>
            <name>maven</name>
            <url>https://yindangu-maven.pkg.coding.net/repository/v-devsuite-sdk/maven/</url>
            <releases>
                <enabled>true</enabled>
            </releases>
            <snapshots>
                <enabled>true</enabled>
            </snapshots>
        </repository>
    </repositories>
</profile>
<!-- 嵌入到maven的settings.xml 这部分是必须的 -->
```

**项目中的maven依赖配置**

```markup
		<!-- ////////////////插件依赖开始/////////////////// -->
		<dependency>
  			<groupId>com.yindangu.v3.platform</groupId>
  	  	<artifactId>plugin-business-api</artifactId>
			  <version>3.3.0</version>
		</dependency>
		<dependency>
			<groupId>com.yindangu.v3.platform</groupId>
			<artifactId>plugin-register</artifactId>
			<version>3.3.0</version>
		</dependency>
		<dependency>
			<groupId>com.yindangu.v3.platform</groupId>
			<artifactId>plugin-utils</artifactId>
			<version>3.3.0</version>
		</dependency>
		<!-- ////////////////插件依赖结束//////////////////// -->
```

**本地依赖**：如果不想配置maven的setings.xml，可以先下载后再本地依赖

```markup
	<!-- ////////////////插件依赖开始/////////////////// -->
		<dependency>
  			<groupId>com.yindangu.v3.platform</groupId>
  	  	<artifactId>plugin-business-api</artifactId>
			  <version>3.3.0</version>
			  <scope>system</scope>
        <systemPath>${basedir}/lib/com.yindangu.v3.platform-plugin-business-api-3.3.0.jar</systemPath>
		</dependency>
```

## **前端环境备**

### **前提要求**

1. 要模块化开发
2. 需要有入口方法
3. 导入平台提供的功能引入方式 vds.import("vds.ds.\*")，其中“vds.import”不能重定义
4. 需要手写接口描述json文件（后端是使用builder模式在代码中声明）

### **环境配置**

V平台的插件体系支持业界开放性的技术开发规范，做到最大的包容性。这里以JavaScript 官方模块化标准的 ES6规范来进行样例工程开发说明（如果希望使用其它的业界常用模块化开发标准，如CommonJS 和 AMD等，V平台仍然可支持适配）。

另外，这里建议使用rollup 作为JavaScript 模块打包器（当然，也可自选其它方式，如[webpack](https://webpack.js.org)）,下面安装rollup开发过程为例子进行说明

_rollup不是必须的，但是没有它工作效率比较低。_[_《rollup 中文文档》_](https://www.rollupjs.com)__

* 安装node [node.js 安装详细步骤教程](https://blog.csdn.net/antma/article/details/86104068)

只有第一步骤，第二步骤是必须的。其他步骤是高级配置，可以忽略。验证安装是否成功

```bash
node -v
npm -v
```

安装成功后可以设置一下淘宝镜像(提高安装速度)

```bash
npm config set registry https://registry.npm.taobao.org
```

* 安装rollup

```bash
npm install rollup -g
```

验证是否安装成功

```bash
npm rollup -v
```

* **rollup配置**

**rollup只是编译工具，工作目录需要手工创建。**

例如示例中在d:/myproject 创建 mydemo文件夹，然后把 rollup.config.js，package.json 复制到工作目录mydemo里。然后创建src文件夹，并且创建main.js，在main.js编写业务处理代码；如果不想使用main.js作为入口，需要修改配置文件的对应参数。

配置文件rollup.config.js指定了源码入口：

```javascript
input: 'src/main.js',//源码主入口路径
```

配置文件package.json指定了源码入口：

```javascript
 "main": "./src/main.js",
```

完整的配置文件

rollup.config.js

```javascript
import babel from "rollup-plugin-babel";
import { terser } from 'rollup-plugin-terser';
// rollup.config.js
export default {
  input: 'src/main.js',//源码主入口路径
  output: {
    file: 'dist/define.js',//打包输出路径
    format:'umd',//编译出umd格式
    name:'com.yindangu.rule.demo',//定义全局命名空间
    sourcemap:true
  },
  plugins: [
    babel({ runtimeHelpers: true }),//babel转换
    terser()//脚本压缩
  ]
};
```

package.json

```javascript
{
  "name": "vplatform-client-rule-demo",
  "version": "1.0.0",
  "description": "客户端规则插件样例",
  "main": "./src/main.js",
  "directories": {
    "test": "test"
  },
  "dependencies": {},
  "devDependencies": {
    "@babel/core": "^7.12.9",
    "@babel/plugin-transform-runtime": "^7.12.1",
    "@babel/preset-env": "^7.12.7",
    "babel-preset-es2015-rollup": "^3.0.0",
    "rollup": "^2.34.1",
    "rollup-plugin-babel": "^4.4.0",
    "rollup-plugin-terser": "^7.0.2"
  },
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [
    "v3"
  ],
  "author": "yindangu",
  "license": "ISC"
}

```

把这两个配置文件rollup.config.js、package.json放在项目根目录。
