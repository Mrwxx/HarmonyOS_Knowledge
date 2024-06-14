[TOC]



## 一、初识 TypeScript

### （一）、TypeScript 与 JavaScript 的关系（下文用TS与JS代替）

&emsp;&emsp;`JS`是动态类型的语言，变量与函数入参都没有静态类型检查，只有在运行时才会报出类型错误，存在较大的安全隐患，`TS`是`JS`的超类，它基于`JS`补充了静态类型检查，可以在任何支持`JS`的平台中执行。同时`TS`还增加了一些变量的类型，如元组、枚举、接口等。



<br>



### （二）、TS编译生成JS

&emsp;&emsp;需要注意的是，`TS`代码是不能被`JS`解析器直接执行，可以通过编译器将`TS`代码编译为`JS`代码后通过`JS`解析器执行。TypeScript提供了一个线上的 Playground 供练习使用，地址为https://www.typescriptlang.org/zh/play，我们可以在该网站中直观地看到编译后的JS文件。

&emsp;&emsp;我们可以自行下载` node.js `，并通过`npm`来安装` typescript`；

```
npm i -g typescript
```



&emsp;&emsp;简单编写一个`ts`文件，如添加一行`ts`语句

```
console.log('hello!');
```



&emsp;&emsp;`tsc` 是 `TypeScript` 官方的**命令行编译器**，用来检查代码，并将其编译成` JavaScript` 代码，我们可以通过`tsc`进行编译，生成`hello.js`文件

```
tsc hello.ts
```



<br>



### （三）、npm

#### 1、npm定义

&emsp;&emsp;`npm`（**Node 包管理器**）是` JavaScript `运行时 `Node.js `的默认程序包管理器。

&emsp;&emsp;`npm `由两个主要部分组成:

- 用于发布和下载程序包的 `CLI`（命令行界面）工具
- 托管` JavaScript `程序包的  [在线存储库](https://www.npmjs.com/)

&emsp;&emsp;作为使用`npm`的开发者，我们通常通过`CLI`工具下载`npm`包，这个是我们最常使用的`npm`操作。



<br>



#### 2、npm关键点

##### （1）、package.json

&emsp;&emsp;每一个`JavaScript` 项目（无论是` Node.js` 还是浏览器应用程序）都可以被当作` npm` 软件包，并且通过  `package.json` 来描述项目和软件包信息。因此，可以将`package.json`理解为项目信息一览表。其中的关键节点如下：

- `name`：`JavaScript `项目或库的名称。
- `version`：项目的版本。通常，在应用程序开发中，由于没有必要对开源库进行版本控制，因此经常忽略这一块。但是，仍可以用它来定义版本。
- `description`：项目的描述。
- `license`：项目的许可证。
- `scripts` ：项目本地运行的命令行工具，如`build`构建命令，`format`格式命令等等
- `dependencies`：生产环境的依赖，可以通过带有  `--save` 标志的  `npm install` 命令安装；
- `devDependencies`：**开发/测试环境的依赖**，可以通过带有  `--save-dev` 标志的  `npm install` 命令安装；

&emsp;&emsp;对于依赖版本，可能会出现如下的符号，参考如下：

- `^`：表示最新的次版本，例如， `^1.0.4` 可能会安装主版本系列  `1` 的最新次版本 `1.3.0`。
- `〜`：表示最新的补丁程序版本，与  `^` 类似， `〜1.0.4` 可能会安装次版本系列 `1.0` 的最新次版本`1.0.7`。

&emsp;&emsp;具体的安装版本体现在 `package-lock.json` 文件中



##### （2）、package-lock.json

&emsp;&emsp;该文件描述了 `npm JavaScript` 项目中使用的**依赖项的确切版本**，该文件通常是由  `npm install` 命令生成的，也可以由我们的 `NPM CLI `工具读取，以确保使用  `npm ci` 复制项目的构建环境



<br>



#### 3、npm使用

##### （1）、npm install

&emsp;&emsp;使用`npm`安装模块，分为**本地安装**、**全局安装**，如下所示：

```
npm install $module_name      # 本地安装
npm install $module_name -g   # 全局安装
```



&emsp;&emsp;本地安装时，会将安装包放在 **./node_modules** 下（运行 `npm` 命令时所在的目录），如果没有 `node_modules `目录，会在当前执行` npm` 命令的目录下生成 `node_modules` 目录；

&emsp;&emsp;全局安装时，会将安装包放在 **/usr/local** 下或者`node` 的安装目录；



<br>



##### （2）、npm uninstall

&emsp;&emsp;使用`npm`卸载模块：

```
npm uninstall $module_name
```



<br>



##### （3）、npm update

&emsp;&emsp;使用`npm`更新模块：



```
npm update $module_name
```



<br>



##### （4）、npm search

&emsp;&emsp;使用`npm`搜索模块：

```
npm search $module_name
```



<br>



### （四）、TS本地环境

#### 1、VSCode



Code Runner插件安装



#### 2、ts-node安装

**ts-node**是一个 TypeScript 的运行环境，它允许我们直接运行 TypeScript 代码。**ts-node**的安装和运行依赖于**Node.js**环境，因此在安装**ts-node**之前，我们需要准备好**Node.js**环境。

如果我们已经安装好了DevEco Studio客户端，就会自动下载node.js，可以在settings中进行查看。



安装好node.js之后，我们需要配置环境变量，便于后续执行npm命令：

```
npm install -g ts-node
```



全局安装时，会将安装包放在 **/usr/local** 下或者`node` 的安装目录下，由于已经在环境变量中配置了node的地址，因此ts-node模块会被直接安装在该node地址的node_modules目录下。



#### 3、typescript安装



在安装完ts-node之后，我们发现typescript也会被自动安装。



注：完成后需要重新启动**VSCode**，另其重新加载环境变量和相关依赖。



### （五）、TS代码运行

- 本地创建空文件夹作为项目目录，如TS-Test
- 使用VSCode打开TS-Test目录，操作路径：File -> Open Folder
- 创建新的ts文件，如Hello.ts
- 通过右上角运行按钮运行代码，在日志中看到输出的结果



![image-20240615003958018](D:\HarmonyOS\workspace\Doc\HarmonyOS_Doc\HarmonyOS_Knowledge\Typescript\初识TS.assets\image-20240615003958018.png)













## Reference

[什么是 npm  (freecodecamp.org)](https://www.freecodecamp.org/chinese/news/what-is-npm-a-node-package-manager-tutorial-for-beginners/)

[NPM 使用介绍 | 菜鸟教程 (runoob.com)](https://www.runoob.com/nodejs/nodejs-npm.html)

[基础类型 · TypeScript中文网 · TypeScript——JavaScript的超集 (tslang.cn)](https://www.tslang.cn/docs/handbook/basic-types.html)

