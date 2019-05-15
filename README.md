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
* preferGlobal
* private
* publishConfig
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
