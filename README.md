# npm package.json 中文文档
## 什么是package.json？

   package.json是一个JSON文件，文件内声明了一个JSON，而不是一个JavaScript对象字面量。该文件描述的行为与npm-config有关。

## package.json可配置字段
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
* 依赖包/dependencies
  
  该字段，是一个简单的对象，以包名:包版本范围形式来保存工程依赖包。依赖包也可以是tarball文件或Git URL。
  
  注意：请不要在dependencies中放置依赖包测试框架或转换器。请参阅*devdependencies*。
  
  包版本范围是一个字符串，其中有一个或多个空格分隔的描述符。有关指定版本范围的详细信息，请参见[semver](https://docs.npmjs.com/misc/semver)。
   *version 必须与版本完全匹配
   *>version 必须大于版本
   *>=version 必须大于等于版本
   *<version 必须小于版本
   *<=version 必须小于等于版本
   *~version “近似等同于版本”见[semver](https://docs.npmjs.com/misc/semver)
   *^version “与版本兼容”见[semver](https://docs.npmjs.com/misc/semver)
   *1.2.x 1.2.0,1.2.1，etc.,but not 1.3.0
   *http://… 请参阅“URLs as Dependencies”
   ** 匹配任何版本
   *"" 空字符串与*相同
   *version1 - version2 与>=version1 <=version2相同，介于版本1和版本2间
   *range1 || range2 只要满足范围1或范围2即可
   *git… 请参阅“URLs as Dependencies”
   *user/repo 请参见“Github URL”
   *tag 已标记并发布为标记的特定版本，请参见[npm-dist-tag](https://docs.npmjs.com/cli/dist-tag)
   *path/path/path 请参阅“localPaths”
  以下这些形式均有效：
  ```
  { "dependencies" :
    { "foo" : "1.0.0 - 2.9999.9999"
     , "bar" : ">=1.0.2 <2.1.2"
     , "baz" : ">1.0.2 <=2.3.4"
     , "boo" : "2.0.1"
     , "qux" : "<1.0.0 || >=2.3.1 <2.4.5 || >=2.5.2 <3.0.0"
     , "asd" : "http://asdf.com/asdf.tar.gz"
     , "til" : "~1.2"
     , "elf" : "~1.2.3"
     , "two" : "2.x"
     , "thr" : "3.3.x"
     , "lat" : "latest"
     , "dyl" : "file:../dyl"
    }
  }
  ```
  **URLs as Dependencies**
  
  您可以指定tarball URL来代替版本范围。
  此tarball将在安装时下载并安装到本地包中。
  
  **Git URLs as Dependencies**
  
  Git URLs的形式如下：
  ```
  <protocol>://[<user>[:<password>]@]<hostname>[:<port>][:][/]<path>[#<commit-ish> | #semver:<semver>]
  ```
  <protocol> is one of git, git+ssh, git+http, git+https, or git+file.
   
   如果提供了<commit ish>，则将使用它精确克隆该提交。如果提交ISH的格式为semver:<semver>，<semver>可以是任何有效的semver范围或精确的版本，并且NPM将在远程存储库中查找与该范围匹配的任何标记或引用，就像查找注册表依赖项一样。如果未指定<commit ish>或semver:<semver>，则使用master。
   
   举例
   ```
   git+ssh://git@github.com:npm/cli.git#v1.0.27
   git+ssh://git@github.com:npm/cli#semver:^5.0
   git+https://isaacs@github.com/npm/cli.git
   git://github.com/npm/cli.git#v1.0.27
   ```
* GitHub URLs

  根据1.1.65版本，可将GitHub URL以"Foo"："user/Foo-project"形式添加到dependencies中（类似于Git URL），也可将添加后缀*commit-ish*，示例如
  下：
  ```
  {
    "name": "foo",
    "version": "0.0.0",
    "dependencies": {
     "express": "expressjs/express",
     "mocha": "mochajs/mocha#4727d357ea",
     "module": "user/repo#feature\/branch"
    }
  }
  ```
* 本地路径/Local Paths

  根据npm@2.0.0版本，可通过运行*npm install -S/npm install --save*来保存包的本地目录的路径。本地路径的形式如下： 
  ```
  ../foo/bar
  ~/foo/bar
  ./foo/bar
  /foo/bar
  ```
  使用以上本地路径形式，被认为是相对路径并添加到package.json中
  ```
  {
   "name": "baz",
   "dependencies": {
    "bar": "file:../foo/bar"
   }
  }
  ```
  此功能有助于本地脱机开发和创建一些不访问内部服务器但需要NPM安装的测试，安装的这些包不会被发布到公共注册表。 

* 开发依赖包/devDependencies

  devDependencies对象用于定义用户可能不希望或不需要下载依赖包所包含的测试或文档框架的依赖包。
  
  当在工程根目录中运行*npm link/npm install*后，这些已安装的依赖包可以像其他NPM配置字段一样进行管理。更多主题信息，请参阅[npm-config](https://docs.npmjs.com/misc/config)
  
  对于平台未指定的构建步骤（例如将CopeScript或其他语言编译为JavaScript），可使用scripts的prepare来执行此操作，并将所需的包设置为一个开发依赖
  包。
  
  举例
  ```
  { 
    "name": "ethopia-waza",
    "description": "a delightfully fruity coffee varietal",
    "version": "1.2.3",
    "devDependencies": {
      "coffee-script": "~1.6.3"
    },
    "scripts": {
      "prepare": "coffee -o lib/ -c src/waza.coffee"
    },
    "main": "lib/waza.js"
  }
  ```
  发布之前，先执行script的prepare命令，这样用户可在未编译的情况下使用这些功能。在dev模式下（即本地运行*npm install*），用户也会运行并轻松测试这
  个脚本。
  
* 对等依赖包/peerDependencies

  在某些情况下，需要表示包与主机工具或库的兼容性，而不必执行此主机的要求。这通常被称为插件。
  模块可能公开了主机文档所预期和指定的特定接口。
  举例
  ```
  {
    "name": "tea-latte",
    "version": "1.3.5",
    "peerDependencies": {
      "tea": "2.x"
    }
  }
  ```
  这样可以确保tea-latte只能与tea的第二个主要版本一起安装。运行*npm install tea-latte*可能产生以下依赖关系图：
  ```
  ├── tea-latte@1.3.5
  └── tea@2.2.0
  ```
  **注意：如果peerDependencies指定的包在依赖关系树中没有显式依赖于更高版本，NPM@1和@2将自动安装这些包。然而在NPM@3中，将出现一条**
  **peerDependencies未安装的警告。** 
  
  NPM@1和@2中的注意行为易混淆，且可容易产生依赖性地狱，因此，NPM@3重新设计，尽可能避免发生这种情况。
  
  安装另一个与需求冲突的插件会发生错误。因此，请确保插件需求尽可能广泛，不要将其锁定到特定的补丁版本。
  假设主机编译符合<font color=red>semver</font>，只有主包主版本的更改才会破坏插件。因此，如果处理过主包的每个1.x版本，请使用“^1.0”或“1.x”。如
  果依赖于1.5.2中介绍的功能，请使用“>=1.5.2<2”。

* 捆绑依赖包/bundledDependencies

  这个字段是一个数组，用来定义工程发布时被捆绑的依赖包。如果需要本地保存npm依赖包或通过单个文件下载使其可用，那么可在bundledDependencies数组指定
  依赖包并执行*npm pack*命令，进而将这些依赖包捆绑到一个tarball文件中
  举例：
  先定义一个package.json文件：
  ```
  {
    "name": "awesome-web-framework",
    "version": "1.0.0",
    "bundledDependencies": [
      "renderized", 
      "super-streams"
    ]
  }
  ```
  运行*npm pack*命令，获得awesome-web-framework.tgz文件（包含renderized和super-streams依赖包）；
  运行*npm install awesome-web-framework.tgz*，则renderized和super-streams依赖包被安装在新工程内。
  也可在package.json文件中，拼写为bundleDependencies。
  
* 可选依赖包/optionalDependencies

  用来存放一些可能找不到或安装失败的依赖包，进而不影响npm运行。这个字段为一个对象，语法是包名称：版本/URL。optionalDependencies和dependencies的
  区别在于构建失败不会导致安装失败。
  
  你的工程可利用这个字段来处理依赖包缺失的问题，比如：
  ```
  try {
    var foo = require('foo')
    var fooVersion = require('foo/package.json').version
  } catch (er) {
    foo = null
  }
  if ( notGoodFooVersion(fooVersion) ) {
    foo = null
  }
  // .. then later in your program ..
  if (foo) {
    foo.doFooThings()
  }
  ```
  此外，可选依赖包会覆盖依赖包中的同名条目，建议同一依赖包放在一个位置上。
  
* 引擎配置/engines
  
  可用来指定工程运行的node版本，若未指定或值为\*，那么可在任何node版本运行。也可指定运行你的工程需要的npm版本
  ```
  {"engines": {"node": ">=0.10.3 <0.12"}}
  {"engines": {"npm": "~1.0.20"}}
  ```  
  如果指定了引擎，那么npm要求必需添加node；如果没有指定，那么npm假设在node上运行。
  
  这个字段仅在按照依赖包时提供警告信息。
  
* 引擎限制/enginesStrict

   >=npm3.0.0 删除此属性,<npm3.0.0，引擎限制
  
* os
  
  用于指定一组运行的操作系统
  可指定某个操作系统
  ```
  "os":["darwin","linux"]
  ```
  可设置黑白名单（一般用不到）
  ```
  "os":["!win32"]
  ```
  主操作系统由process.platform决定
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
