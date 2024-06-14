[TOC]

<br>



## 背景

  &emsp;&emsp;小白新员工从一个**Hello World**应用了解一个**Harmony OS**应用的基本结构。



<br>



## 一、Hello World 应用拆解

### （一）、Hello World 应用构建

  &emsp;&emsp;参考如下步骤构建一个`Hello World` 应用：

[构建第一个ArkTS应用（Stage模型）-HarmonyOS应用开发](https://developer.harmonyos.com/cn/docs/documentation/doc-guides-V3/start-with-ets-stage-0000001477980905-V3)



<br>



### （二）、FA模型与Stage模型



  &emsp;&emsp;在创建项目的过程中，我们会选择使用**FA模型或者Stage模型**进行创建项目，**FA、Stage**模型是**HarmonyOS**先后提供的两种应用模型，对于应用模型官方的定义如下所示：

> 应用模型是HarmonyOS为开发者提供的应用程序所需能力的抽象提炼，它提供了应用程序必备的组件和运行机制。有了应用模型，开发者可以基于一套统一的模型进行应用开发，使应用开发更简单、高效。



  &emsp;&emsp;可以看到的是，应用模型是一套统一的、固定不变的开发架构，**HarmonyOS**应用的开发者遵循这套架构规则进行应用程序的开发。应用模型包括：**应用组件、应用进程模型、应用线程模型、应用任务管理模型、应用配置文件**，具体的内容可参考官网：[应用模型的构成要素-HarmonyOS应用开发](https://developer.harmonyos.com/cn/docs/documentation/doc-guides-V3/application-model-composition-0000001544384013-V3)



  &emsp;&emsp;那么**FA、Stage**这两个具体的应用模型有哪些区别呢？

> **Stage模型与FA模型最大的区别在于**：Stage模型中，多个应用组件**共享同一个ArkTS引擎实例**；而FA模型中，每个应用组件独享一个ArkTS引擎实例。因此在**Stage模型中，应用组件之间可以方便的共享对象和状态，同时减少复杂应用运行对内存的占用**。Stage模型作为主推的应用模型，开发者通过它能够更加便利地开发出分布式场景下的复杂应用。



  &emsp;&emsp;官网详细地介绍了从应用模型的五大组成元素的角度进行对比的结果：

[应用模型解读-应用模型概述-应用模型-开发-HarmonyOS应用开发](https://developer.harmonyos.com/cn/docs/documentation/doc-guides-V3/application-model-description-0000001493584092-V3)



<br>



### （三）、APP包结构

  &emsp;&emsp;对于一个简单的**HarmonyOS**应用来说，可能只有一个**module**，但是对于一个庞大的项目来说，可能会有多个**module**，因此对于应用开发者来说，还需要了解应用的包结构。



#### 1、APP与HAP的关系

  &emsp;&emsp;下面是官网对于应用发布**APP、HAP**包之间关系的描述，具体参考[工程介绍-工程管理-DevEco Studio使用指南-工具-HarmonyOS应用开发](https://developer.harmonyos.com/cn/docs/documentation/doc-guides-V3/project_overview-0000001053822398-V3)：

> 应用/服务发布形态为**APP Pack**（Application Package，简称APP），它是由一个或多个**HAP**（Harmony Ability Package）包以及描述APP Pack属性的pack.info文件组成。
>
> **一个HAP在工程目录中对应一个Module**，它是由**代码、资源、第三方库及应用/服务配置文件**组成，HAP可以分为**Entry**和**Feature**两种类型。
>
> - **Entry：**应用/服务的**主模块**，**可独立安装运行**。一个APP中，**对于同一类型的设备，可以包含一个或多个Entry类型的HAP**，如果同一类型的设备包含多个Entry模块，需要[配置distroFilter分发规则](https://developer.harmonyos.com/cn/docs/documentation/doc-guides-V3/add_new_module-0000001053223741-V3#section18939175113911)，使得应用市场在做应用的云端分发时，对该设备类型下不同规格的设备进行分发。
> - **Feature：**应用/服务的**动态特性模块**。一个APP可以包含**零到多个Feature**类型的HAP。只有包含Ability的HAP才能够独立运行。



<img data-original="http://image.huawei.com/tiny-lts/v1/images/53e4d4361116496632cc782ebe8868f4_602x1047.png" src="https://3ms.huawei.com/hi/public/images/grey.gif" alt="image-20231031161807514" style="zoom:50%;" />



  &emsp;&emsp;我们可以看到的是，**一个APP由多个HAP包组成**，**这些HAP包中包括一个或多个Entry包，以及若干个（可以为0）个Feature包**。

  &emsp;&emsp;**Entry**模块是应用的**主模块**，通常用于实现应用的入口界面、入口图标、主特性功能等。

  &emsp;&emsp;**Feature**模块是应用的**动态特性模块**，通常用于实现应用的特性功能，可以**配置成按需下载安装**，因此**即使没有Feature模块也行**。



  &emsp;&emsp;**多个HAP机制**的设计思路，可参考官网：[多HAP机制设计目标-HarmonyOS应用开发](https://developer.harmonyos.com/cn/docs/documentation/doc-guides-V3/multi-hap-objective-0000001427584580-V3)



<br>



#### 2、HAP、HAR、HSP

  &emsp;&emsp;对于一个复杂项目，我们可以通过查看项目结构目录了解当前项目的模块组成，可以发现除了**Entry**、**Feature**之外，还有**Shared、Har**的模块类型，如下所示：

![image-20231031163506722](http://image.huawei.com/tiny-lts/v1/images/b8197750c72096629c87b5375d5ea37b_998x288.png)



  &emsp;&emsp;原来， **Module**分为**Ability**和**Library**两种类型，**“Ability”**类型的**Module**对应于编译后的**HAP（Harmony Ability Package）**；**“Library”**类型的**Module**对应于**HAR（Harmony Archive）**，或者**HSP（Harmony Shared Package）**

  &emsp;&emsp;**HAR静态共享包**，和**HSP动态共享包**，都是为了**实现代码和资源的共享**，都可以包含代码、C++库、资源和配置文件。其中**HAR不支持在配置文件中声明pages页面**，**HSP支持配置pages页面**。 使用**HAR/HSP**，能够实现多个模块或多个工程**共享ArkUI组件、资源、页面(HSP)**等相关代码，进一步对应用进行**解耦**，减少包体积。



<br>



#### 3、创建四种module

  &emsp;&emsp;**DevEco  Studio** 中创建一个 **module**，可以选择多种模板，四种**module**对应的模板已经标注在图中：



<img data-original="http://image.huawei.com/tiny-lts/v1/images/536e97527505172aa27ad5b5d7caf9b0_994x683.png" src="https://3ms.huawei.com/hi/public/images/grey.gif" alt="image-20231031165252341" style="zoom:67%;" />



<br>



### （四）、ArkTS工程目录结构（Stage模型）

  &emsp;&emsp;虽然我们只是创建了一个简单的**HarmonyOS** 应用，但也可以通过它管中窥豹，了解一个应用的基本结构，不至于在获得一个项目时无法快速了解基本信息。



#### 1、AppScore

##### （1）、resources

  &emsp;&emsp;多**module**共享的资源目录。



<br>



##### （2）、app.json5

  &emsp;&emsp;**AppScore**目录中主要包括**应用范围内的全局配置文件app.json5**，其中包括应用的包名、开发厂商、版本号等基本信息，还有特定设备类型的配置信息。

<img data-original="http://image.huawei.com/tiny-lts/v1/images/54f25ee14fe5d6ccd43e9c6c40638c6e_407x204.png" src="https://3ms.huawei.com/hi/public/images/grey.gif" alt="image-20231031172755573" style="zoom: 80%;" />



&emsp;&emsp;这里，我们只是简单介绍一下不可缺省的配置参数：

- **bundleName**：标识应用的**Bundle**名称，用于标识应用的唯一性，在该应用下创建的源码文件都会以该**Bundle**名称为前缀。

- **icon**：标识[应用的图标](https://developer.harmonyos.com/cn/docs/documentation/doc-guides-V3/application-component-configuration-stage-0000001478340869-V3)，标签值为图标资源文件的索引。如`$media:app_icon`就表示**AppScore/resources/base/media**目录下的图片名称；

- **label**：标识[应用的名称](https://developer.harmonyos.com/cn/docs/documentation/doc-guides-V3/application-component-configuration-stage-0000001478340869-V3)，标签值为字符串资源的索引。如`$string:app_name`就表示**AppScore/resources/base/element**目录下的**string.json**文件中的**app_name**字符串；

- **versionCode**：标识应用的版本号，该标签值为32位非负整数。此数字仅用于确定某个版本是否比另一个版本更新，数值越大表示版本越高。

- **versionName**：标识应用版本号的文字描述，用于向用户展示。该标签仅由数字和点构成，推荐采用**A.B.C.D**四段式的形式。



 &emsp;&emsp; **AppScore -> app.json5** 详细配置信息解读参考：[app.json5配置文件-HarmonyOS应用开发](https://developer.harmonyos.com/cn/docs/documentation/doc-guides-V3/app-configuration-file-0000001427584584-V3)



<br>



#### 2、module

 &emsp;&emsp; 自定义的**module**工程模块结构如下所示，我们一一进行拆解：

<img data-original="C:%5CUsers%5Cw00810062%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20231031172933271.png" src="https://3ms.huawei.com/hi/public/images/grey.gif" alt="image-20231031172933271" style="zoom:80%;" />



##### （1）、src源码

- **src > main > ets**：用于存放**ArkTS源码**。
- **src > main > ets > entryability**：应用/服务的入口，可以打开看到其中已经自动创建好了一个**EntryAbility.ets**，这是**应用的入口类**，其中定义了应用的生命周期回调函数。
- **src > main > ets > pages**：应用/服务包含的页面，其中已经**自动创建好了一个index.ets**，这是**应用的主页面**，其中已经用系统提供的组件写了简单的**Hello World**。



<br>



##### （2）、resources资源目录

 &emsp;&emsp; **src > main > resources**：用于存放应用/服务所用到的资源文件，如图形、多媒体、字符串、布局文件等。关于资源文件，详见后面的[资源分类与访问](#resources)章节。



<br>



##### （3）、module.json5

 &emsp;&emsp; **src > main > module.json5**：**Stage模型**模块配置文件，应用在当前的模块下。主要包含**HAP包的配置信息、应用/服务在具体设备上的配置信息以及应用/服务的全局配置信息**。

  &emsp;&emsp;**要了解一个模块的基本信息，可以从该配置文件开始**：

- **name**：模块的唯一标识符
- **type**：模块属于四种类型中的哪一种
- **srcEntry**：模块对应的代码路径
- **mainElement**：模块的入口UIAbility名称或者ExtensionAbility名称（代码走读可用）
- **deviceTypes**：该模块可以运行在哪类设备



  &emsp;&emsp;这些信息中有的是字符串形式，有的是数组形式，如模块需要申请的权限就是数组形式，拓展能力也是数组形式，对于每一个拓展能力都是采用字符串对的形式来描述。



  &emsp;&emsp;具体的配置文件说明，详见[module.json5配置文件](https://developer.harmonyos.com/cn/docs/documentation/doc-guides-V3/module-configuration-file-0000001427744540-V3)。



<br>



##### （4）、build-profile.json5

  &emsp;&emsp;**build-profile.json5**：当前**模块级别的**信息、编译信息配置项，包括**buildOption、targets**配置等。

  &emsp;&emsp;其中**buildOption** 中配置编译的选项，如**so打包顺序、so覆盖优先级**等；

  &emsp;&emsp;其中**targets**是开发者自定义的版本，应用厂商会根据不同的部署环境，不同的目标人群，不同的运行环境等，将**同一个应用定制为不同的版本**，如国内版、国际版、普通版、VIP版、免费版、付费版等。针对以上场景，**DevEco Studio**支持通过少量的代码差异化配置处理，在编译构建过程中实现一个应用**构建出不同的目标产物版本**，从而实现源代码、资源文件等的高效复用。**一个模块可以定义多个target，每个target对应一个定制的HAP，通过配置可以实现一个模块构建出不同的HAP**。

  &emsp;&emsp;**注意！！！**在定义**HarmonyOS**应用/服务的**target**时，需要通过**runtimeOS**字段标识该**Target**是一个可运行在**HarmonyOS**设备上的**HAP**。**如果未定义该字段，或该字段取值为OpenHarmony，则表示该Target是一个运行在OpenHarmony设备上的HAP**，**不能运行在HarmonyOS设备上**。



  &emsp;&emsp;具体的编译构建信息，可参考[定制多目标构建产物-HarmonyOS应用开发](https://developer.harmonyos.com/cn/docs/documentation/doc-guides-V3/customized-multi-targets-and-products-0000001430013853-V3)



<br>



##### （5）、hvigorfile.ts

- **hvigorfile.ts**：**模块级**编译构建任务脚本，开发者可以自定义相关任务和代码实现。



<br>





##### （6）、oh-package.json5

- **oh-package.json5**：应用的三方包依赖，支持共享包的依赖。



  &emsp;&emsp;应用/服务支持通过**ohpm**来安装、共享、分发代码，管理项目的依赖关系。

> OHPM CLI（OpenHarmony Package Manager Command-line Interface） 作为鸿蒙生态三方库的包管理工具，支持共享包的发布、安装和依赖管理。
>
> 在DevEco Studio 3.1 Beta2上新建API 9及以上版本的工程将使用ohpm作为默认包管理器。



  &emsp;&emsp;**ohpm**包的依赖分为三种：**ohpm原生三方包**、**ohpm三方共享包**和**ohpm本地共享模块**，都是统一在工程\模块下的**oh-package.json5**文件中配置的。

- **ohpm**原生三方包依赖示例如下：

  ```
  "dependencies": {  "eslint": "^7.32.0",  ... }
  ```

- 三方包依赖示例如下：

  ```
  "dependencies": { "@ohos/lottie": "^2.0.0", ...}
  ```

- **ohpm**本地共享模块依赖示例如下

  ```
  "dependencies": { "library": "file:../library",  ...}
  ```

  &emsp;&emsp;依赖配置完成后，请在**Terminal**窗口执行**ohpm install**命令下载依赖包，或者单击编辑器窗口上方的**Sync Now**进行同步，依赖包会存储在工程或各模块的**oh_modules**目录下。

```
ohpm install
```



  &emsp;&emsp;具体的依赖定义以及依赖引用，请参考[配置应用的依赖-HarmonyOS应用开发](https://developer.harmonyos.com/cn/docs/documentation/doc-guides-V3/configuring-dependencies-0000001545614945-V3)



<br>



#### 3、hvigor

  &emsp;&emsp;**Hvigor构建工具**：一款全新基于**TS**实现的**前端构建任务编排工具**，结合**npm**包管理机制，主要提供任务管理机制，任务注册编排、工程模型管理、配置管理等关键能力，更符合**ArkTS/JS**开发者的开发习惯。

  &emsp;&emsp;**HarmonyOS API Version 9**基于**Hvigor构建体系**，**DevEco Studio**定义了其工程范式，下面是**Hvigor构建体系的工程目录结构**示意图：

<img data-original="http://image.huawei.com/tiny-lts/v1/images/57b0f8fbd6590408e2b2015c756f028a_890x630.png" src="https://3ms.huawei.com/hi/public/images/grey.gif" alt="image-20231101102429025" style="zoom: 67%;" />

  &emsp;&emsp;当前工程下有**hvigor目录**，其中包括**hvigor-config.json5、hvigor-wrapper.js**两个文件，查看**hvigor-config.json5**文件中，定义了**hvigor的版本，hvigor的依赖**，依赖中初始有个**hvigor-ohos-plugin**构建插件，该插件是基于**Hvigor**构建工具开发的一个插件，利用**Hvigor**的任务编排机制实现应用/服务构建任务流的执行，完成**HAP/APP**的构建打包，应用于应用/服务的构建。



  &emsp;&emsp;理论上，这些都不需要我们操心，因为是**DevEco Studio 自动配置配套版本的编译工具和构建插件依赖**，关于构建工具和构建插件的版本配套关系可参考[DevEco Studio版本说明](https://developer.harmonyos.com/cn/docs/documentation/doc-releases/release_notes-0000001057597449)。



<br>



#### 4、oh_modules

  &emsp;&emsp;**oh_modules**目录用于存放三方库依赖信息，之前我们通过工程/模块级的 **oh-package.json5**文件**定义了依赖三方库信息**，**oh_modules**目录就会显示三方库的具体内容，以一个简单依赖库为例：

![image-20231101104758348](http://image.huawei.com/tiny-lts/v1/images/bde87a6995606a747c680f8754dbb10e_1107x305.png)



  &emsp;&emsp;我们可以看到的是， **oh-package.json5**文件中定义的依赖都出现在了**oh_modules**目录下，**.ohpm**是通过**ohpm管理的包目录**，下面的`@hw-appgallery、@ohos`目录都是分类过的依赖包。



<br>



#### 5、build-profile.json5

  &emsp;&emsp;当前工程级别的应用/服务构建配置文件，其中包括

- **signingConfigs**：工程的签名信息

- **compileSdkVersion**：指定当前工程应用编译时使用的**SDK**版本，

- **compatibleSdkVersion**：可兼容的最低**SDK**版本

- **products**： 定义构建的产品品类，如通用默认版、付费版、免费版等

- **modules**：每个模块构建信息，其实已经在各个模块的**build-profile.json5**文件中定义过了，这边起到汇总作用



  &emsp;&emsp;具体的详细信息可参考[配置编译构建信息-编译构建-DevEco Studio使用指南-工具-HarmonyOS应用开发](https://developer.harmonyos.com/cn/docs/documentation/doc-guides-V3/build_config-0000001052902431-V3)



<br>



#### 6、hvigorfile.ts

- **hvigorfile.ts**：**工程级**编译构建任务脚本，开发者可以自定义相关任务和代码实现。



<br>



#### 7、hvigorw

- **hvigor** 命令行工具。



<br>



### （五）、资源分类与访问

#### 1、资源介绍

  &emsp;&emsp;**HarmonyOS**应用开发过程中，经常需要用到颜色、字体、间距、图片等资源，在不同的设备或配置中，这些资源的值可能不同。因此，我们需要自定义应用资源来支撑应用的开发，也可以使用系统自带的资源。

- 应用资源：借助资源文件能力，开发者在应用中自定义资源，自行管理这些资源在不同的设备或配置中的表现。
- 系统资源：开发者直接使用系统预置的资源定义（即分层参数，同一资源ID在设备类型、深浅色等不同配置下有不同的取值）。



<br>



#### 2、resources目录分析

##### （1）、资源目录结构

  &emsp;&emsp;**stage**模型多工程情况下共有的资源文件放到**AppScope**下的**resources**目录中；

  &emsp;&emsp;在**src**源码目录下会存在**多个resources**目录，用来存储开发中使用到的各类资源文件；

  &emsp;&emsp;**resources**目录包括三大类目录，一类为**base**目录，一类为**限定词**目录（**en_US**和**zh_CN**是默认存在的两个限定词目录），一类为**rawfile**目录，如下所示：

```powershell
resources 
|---base 
|   |---element 
|   |   |---string.json 
|   |---media 
|   |   |---icon.png 
|   |---profile 
|   |   |---test_profile.json 
|---en_US  // 默认存在的目录，设备语言环境是美式英文时，优先匹配此目录下资源 
|   |---element 
|   |   |---string.json 
|   |---media 
|   |   |---icon.png 
|   |---profile 
|   |   |---test_profile.json 
|---zh_CN  // 默认存在的目录，设备语言环境是简体中文时，优先匹配此目录下资源 
|   |---element 
|   |   |---string.json 
|   |---media 
|   |   |---icon.png 
|   |---profile 
|   |   |---test_profile.json 
|---en_GB-vertical-car-mdpi // 自定义多限定词目录示例，由开发者创建 
|   |---element 
|   |   |---string.json 
|   |---media 
|   |   |---icon.png 
|   |---profile 
|   |   |---test_profile.json 
|---rawfile // 其他类型文件，原始文件形式保存，不会被集成到resources.index文件中。文件名可自定义。
```



<br>



##### （2）、base目录

  &emsp;&emsp;**base**目录默认存在，为一个基础的资源目录，**优先级较低，当应用的resources目录中没有与设备状态匹配的限定词目录时，会自动引用该目录中的资源文件**。

- 组织形式：从上述的资源目录结构中可以看出，**base**目录的二级子目录为**资源组目录**，用于存放字符串、颜色、布尔值等基础元素，以及媒体、动画、布局等资源文件，包括**element、media、profile几种类型**，注意，**profile**是自定义的配置文件，如**Hello World**应用的**profile**资源目录中就包含一个**main_pages.json**，其中定义了当前的页面。

- 我们可以通过在**resources**目录右键点击 **New -> Resource File**，指定资源类型创建资源文件，具体要求参见[resources资源组目录](https://developer.harmonyos.com/cn/docs/documentation/doc-guides-V3/resource-categories-and-access-0000001544463977-V3#ZH-CN_TOPIC_0000001523808422__资源组目录)。

- 编译方式：在应用/模块编译过程中，**base**目录下得资源文件会被**编译成二进制文件**，并被赋予**资源文件ID**。

- 引用方式：通过指定资源类型（**type**）和资源名称（**name**）来引用。



<br>



##### （3）、限定词目录

  &emsp;&emsp;限定词目录需要开发者**自行创建**，简单来理解这个资源目录是**根据具体的版本来制定的特殊资源目录**，在应用使用某资源时，系统会**根据当前设备状态优先从相匹配的限定词目录中寻找该资源**。只有当**resources**目录中没有与设备状态匹配的限定词目录，或者在限定词目录中找不到该资源时，才会去**base**目录中查找。下面是官网对于限定词目录的定义：

> 限定词目录由一个或多个表征应用场景或设备特征的限定词组合而成，包括移动国家码和移动网络码、语言、文字、国家或地区、横竖屏、设备类型、颜色模式和屏幕密度等维度，限定词之间通过下划线（_）或者中划线（-）连接。开发者在创建限定词目录时，需要掌握限定词目录的命名要求，以及限定词目录与设备状态的匹配规则。



- 编译方式：目录中的资源文件会被**编译成二进制文件**，并赋予**资源文件ID**。

- 引用方式：通过指定资源类型（**type**）和资源名称（**name**）来引用。



  &emsp;&emsp;限定词目录的详细命名与使用，参考[资源分类与访问-HarmonyOS应用开发](https://developer.harmonyos.com/cn/docs/documentation/doc-guides-V3/resource-categories-and-access-0000001544463977-V3#ZH-CN_TOPIC_0000001523808422__资源组目录)



<br>



##### （4）、rawfile目录

  &emsp;&emsp;**rawfile**是**原始文件目录**，**不会根据设备状态去匹配不同的资源**。支持创建多层子目录，目录名称可以自定义，文件夹内可以自由放置各类资源文件。

- 编译方式：目录中的资源文件会被**直接打包进应用，不经过编译**，也不会被赋予资源文件ID。

- 引用方式：通过**指定文件路径和文件名来引用**。



<br>



 ## Reference

[构建第一个ArkTS应用（Stage模型）-HarmonyOS应用开发](https://developer.harmonyos.com/cn/docs/documentation/doc-guides-V3/start-with-ets-stage-0000001477980905-V3)

[工程介绍-工程管理-DevEco Studio使用指南-工具-HarmonyOS应用开发](https://developer.harmonyos.com/cn/docs/documentation/doc-guides-V3/project_overview-0000001053822398-V3)

[使用多Module的应用场景 - 鸿蒙开发者社区 - 3ms知识管理社区 (huawei.com)](https://3ms.huawei.com/hi/group/3943104/wiki_7516635.html)

[多HAP机制设计目标-HarmonyOS应用开发](https://developer.harmonyos.com/cn/docs/documentation/doc-guides-V3/multi-hap-objective-0000001427584580-V3)

[app.json5配置文件-HarmonyOS应用开发](https://developer.harmonyos.com/cn/docs/documentation/doc-guides-V3/app-configuration-file-0000001427584584-V3)

[module.json5配置文件](https://developer.harmonyos.com/cn/docs/documentation/doc-guides-V3/module-configuration-file-0000001427744540-V3)

[定制多目标构建产物-HarmonyOS应用开发](https://developer.harmonyos.com/cn/docs/documentation/doc-guides-V3/customized-multi-targets-and-products-0000001430013853-V3)

[配置编译构建信息-编译构建-DevEco Studio使用指南-工具-HarmonyOS应用开发](https://developer.harmonyos.com/cn/docs/documentation/doc-guides-V3/build_config-0000001052902431-V3)

[配置应用的依赖-HarmonyOS应用开发](https://developer.harmonyos.com/cn/docs/documentation/doc-guides-V3/configuring-dependencies-0000001545614945-V3)

[resources资源组目录](https://developer.harmonyos.com/cn/docs/documentation/doc-guides-V3/resource-categories-and-access-0000001544463977-V3#ZH-CN_TOPIC_0000001523808422__资源组目录)

[资源分类与访问-HarmonyOS应用开发](https://developer.harmonyos.com/cn/docs/documentation/doc-guides-V3/resource-categories-and-access-0000001544463977-V3#ZH-CN_TOPIC_0000001523808422__资源组目录)