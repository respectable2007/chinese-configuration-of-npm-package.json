# npm package.json 中文文档
## 什么是package.json？

   package.json是一个JSON文件，文件内声明了一个JSON，而不是一个JavaScript对象字面量。该文件描述的行为与npm-config有关。

## package.json可配置属性
* name
* version
* description
* keywords
* homepage
* bugs
* license
* 作者
* files
* main
* browser
* bin
* man
* directories
* repository
* scripts
* config
* dependencies
* GitHub URLs
* Local Paths
* devDependencies
* peerDependencies
* bundledDependencies
* optionalDependencies
* engines
* enginesStrict
* os
* cpu

  用来指定一组代码运行的cpu架构。
  指定代码运行在某些cpu架构，如下：
  ```
  "cpu": ["x64","ia32"]
  ```
  指定cpu架构黑名
  ```
  "cpu:: ["!arm","!mips"]
  ```
  主cpu架构由process.arch决定
  
* 信息提示/preferGlobal

  *已弃用*
  
  之前用于触发NPM警告，现在用于提供信息，且建议尽可能将任何二进制文件安装为本地开发依赖包（devDependencies）。
  
* 私有/private

  该属性为true，NPM将拒绝发布它。可防止意外发布私有库。
  
  如果想要确保给定的包只发布到特定的注册表（例如内部注册表），可在publishConfig属性中重写注册表配置参数。
 
* 发布配置/publishConfig

  该属性可定义一组在发布时使用的配置值。如果您想设置标记、注册表或访问，那么它特别方便，这样可确保给定的包没有标记为“最新”、被发布到全局公共注册表和默认情况下范围模块是私有的。
  
  可重写任何配置值，但只有“tag”、“registry”和“access”可能对发布有影响。
  
  可重写的配置选项请参阅npm config。
  
* 默认值/default values

  npm会根据文件夹内容设置一些默认值，如下：
  * 如果你的文件夹根目录中包含server.js文件，那么npm会在scripts属性中定义node server.js命令
    ```
    "scripts":{"start":"node server.js"}
    ```
  * 如果你的文件夹根目录中包含binding.gyp文件，且未定义instll/preinstall命令，那么npm会在scripts属性中定义node-gyp rebuild
    命令
     ```
    "scripts":{"install":"node-gyp rebuild"}
    ```
  * 如果你的文件夹根目录中包含AUTHORS文件，那么npm会将文件中每行解析为Name<email>(url)格式，其中email/url为可选，以#或空格为开头的行会被忽略，
    解析结果会定义在contributors属性中
     ```
    "contributors":[{"name": "xxx","email": "xxx","url": "xxx"]
    ```
