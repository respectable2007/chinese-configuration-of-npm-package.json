# npm package.json 中文文档
## 什么是package.json？

   package.json是一个JSON文件，文件内声明了一个JSON，而不是一个JavaScript对象字面量。该文件描述的行为与npm-config有关。

## package.json可配置字段
* 名称/name

  如果打算发布包，那么package.json必须包含name和versiion。name和version生成唯一的标识符。包发生更改，version也应发生更改。
  
  如果不打算发布包，那么name和version是可选的。

  name，是你的工程的名称。

  命名规则：
  * 名称必须少于或等于214个字符。这包括作用域包的作用域。
  * 名称不能以点或下划线开头。
  * 新包的名称中不能包含大写字母。
  * 名称最终是URL、命令行上的参数和文件夹名称的一部分。因此，名称不能包含任何非URL安全字符。

  命名技巧：
  * 不要与核心node模块相同的名称。
  * 不要将“js”或“node”放在名称中。如果是js，当在写package.json文件时，可以使用“engines”字段指定引擎。（见下文）
  * 名称可能会作为参数传递给require()，因此名称应尽量简短，但也应该是合理的描述性的。
  * 可能在[https://www.npmjs.com/](https://www.npmjs.com/)上查阅NPM注册表，来看看名称是否已经存在。
  
  名称也可以以scope作为前缀，例如@myorg/mypackage。更多细节见[npm-scope](https://docs.npmjs.com/misc/scope)。

* 版本/version
  
  如果打算发布包，那么package.json必须包含name和versiion。name和version生成唯一的标识符。包发生更改，version也应发生更改。
  
  如果不打算发布包，那么name和version是可选的。

  version必须可由[node-semver](https://github.com/npm/node-semver)（npm作为依赖包与之捆绑）解析（npm install semver命令）。

  有关版本号和范围的更多信息，请查阅[semver](https://docs.npmjs.com/misc/semver)。

* 描述/description

  这个字段，是一个字符串，有利于用户在npm search中更快的找到你的包。

* 关键字/keywords

  这个字段，是一个字符串数组，有利于用户在npm search中更快的找到你的包。

* 主页/homepage

  指定项目主页url，如下：
  ```
  "homepage": "https://github.com/owner/project#readme"
  ```

* 问题/bugs

  可以是用来跟踪项目问题的URL，也可以是向作者报告问题的电子邮件地址，方便那些引用包遇到问题的人。如下：
  ```
  { 
    "bugs": {
      "url" : "https://github.com/owner/project/issues",
      "email" : "project@hostname.com"
    }
  }
  ```
  可以指定一个或两个值。如果只提供一个URL，可将“bugs”的值指定为一个字符串，而不是一个对象。
  如果提供了一个URL，可以使用npm bugs命令。

* 证书/license
  
  这个字段，用来指定包的许可证，方便其他用户使用和了解包的限制。
  如果是通用许可证（如BSD-2-Clause或MIT），请添加当前的SPDX许可证ID，如下：
  ```
  { "license" : "BSD-3-Clause" }
  ```
  请查阅[SPDX许可证ID的完整列表](https://spdx.org/licenses/)。理想情况下，应该选择OSI批准的SPDX许可证ID。

  如果包是在多个通用许可证下获得许可的，请使用[SPDX许可证表达式语法版本2.0字符串](https://spdx.org/licenses/)，如下：
  ```
  { "license" : "(ISC OR GPL-3.0)" }
  ```
  如果许可证尚未分配SPDX ID，或者是自定义许可证，可使用如下字符串值：
  ```
  { "license" : "SEE LICENSE IN <filename>" }
  ```
  在包的顶层包含一个名为\<filename\>的文件。

  一些老版本的包使用许可证对象或许可证对象数组： 
  ```
  // Not valid metadata
  { "license" :
    { "type" : "ISC", 
      "url" : "https://opensource.org/licenses/ISC"
    }
  }
  // Not valid metadata
  { "licenses" :
    [
      { "type": "MIT",
        "url": "https://www.opensource.org/licenses/mit-license.php"
      },
      { "type": "Apache-2.0",
        "url": "https://opensource.org/licenses/apache2.0.php"
      }
    ]
  }
  ```
  这些现在已被弃用。相反，使用SPDX表达式，如下：
  ```
  { "license": "ISC" }
  { "license": "(MIT OR Apache-2.0)" }
  ```
  最后，如果不允许其他人在任何目录下使用私有或未发布包，可如下设置： 
  ```
  { "license": "UNLICENSED" }
  ```
  还可以设置"private"：true,以防止意外发布。

* 作者/people fields
  
  **author**
  这个字段，是一个作者，是一个对象，包含name、email和url三个字段，email和url为可选字段，如下
  ```
  {
    "author": { 
      "name" : "Barney Rubble",
      "email" : "b@rubble.com",
      "url" : "http://barnyrubble.tumblr.com/"
    }
  }
  ```
  也可以简写成：
  ```
  {
    "author": "Barney Rubble <b@rubble.com> (http://barneyrubble.tumblr.com/)"
  }
  ```
  **contributors**
  这个字段，是一个贡献者数组

  npm也可以设置maintainers字段，来显示创作者信息

* 文件/files

   这个字段是一个文件格式数组，描述当包作为依赖包安装时所包含的条目。文件格式的语法类似于.gitignore。但当文件格式包括文件、目录或glob格式（\*，\*\*/\*等）时，文件在打包时被写入tarball中。
   省略该字段或设置为["\*"]，这意味着将包含所有文件。

   一些特殊的文件和目录也会包含或排除，不管它们是否存在于文件数组中（见下文）。

   可以在包的根目录或子目录中提供一个.npmignore文件，以防止文件被包含。在包的根目录，不会重写files字段，但在子目录中，会重写。.npmignore文件的工作方式与.gitignore类似。如果有.gitignore文件，但缺少.npmignore，则将使用.gitignore的内容。
   包含*package.json#files*的文件，不能通过.npmignore或.gitignore被排除。
   无论设置如何，某些文件始终会被包含： 
  * package.json
  * README
  * CHANGES/CHANGELOG/HISTORY
  * LICENSE/LICENCE
  * NOTICE
  * main字段包含的文件

  **README CHANGE LICENSE NOTICE**可以包含任何插件

  相反地，以下文件会被忽略：
  * .git
  * CVS
  * .svn
  * .hg
  * .lock-wscript
  * .*.swp
  * .DS_Store
  * ._*
  * npm-debug.log
  * .npmrc
  * node_modules
  * config.gypi
  * *.orig
  * package-lock.json(使用shrinkwrap替代)

* main

  main字段，用来定义项目或工程的主入口。如果包名为foo，且被用户安装和被引用（例如require("foo")），那么该模块的API会被暴露（即返回exports对
  象）。
  对于大多数模块，包含一个主脚本是非常重要的，而其他的通常不太重要。
  
* 客户端/browser

  如果模块被设计为只在客户端使用，那么请使用browser字段而不是main字段，用来提示用户该模块可能依赖node.js不支持的原语（例如窗口）。

* bin
  
  包可以包含一个或多个用户可能安装到路径中的可执行文件。NPM简化了这个过程，实际上，NPM使用这个字段来安装“NPM”可执行文件。
  这个字段，是一个对象，键是命令名，值是本地文件名。全局安装，NPM将该文件与*prefix/bin*建立符号链接，本地安装，则将文件与**./node_modules/.bin/*建立符号链接。 

  举例，
  ```
  { "bin" : { 
    "myapp" : "./cli.js" 
    } 
  }
  ```
  当安装myapp时，将创建cli.js脚本文件和/usr/local/bin/myapp的符号链接。

  如果只有一个可执行文件，并且命令名与包名相同，那么可以省略命令名，将值作为字符串赋值给bin。例如：
  ```
  { "name": "my-program", 
    "version": "1.2.5", 
    "bin": "./path/to/program" 
  }
  ```
  与如下等效：
  ```
  { "name": "my-program", 
    "version": "1.2.5", 
    "bin" : { 
      "my-program" : "./path/to/program" 
    } 
  }
  ```
  请确保bin中引用的文件以**#！/usr/bin/env node*为开头，否则脚本将在没有可执行节点的情况下启动！ 

* man
  
  这个字段，是一个数组，用于设置man程序可查找的单个文件或一组文件。 

  如果只提供了一个文件，那么将安装man\<pkgname\>运行结果，而忽略实际文件名。例如： 
  ```
  { "name" : "foo",
    "version" : "1.2.3",
    "description" : "A packaged foo fooer for fooing foos",
    "main" : "foo.js",
    "man" : "./man/doc.1"
  }
  ```
  将链接./man/doc.1文件，使其成为man foo的目标
  如果文件名不是以包名开头，那么它是以前缀开头的。如下：
  ```
  { "name" : "foo",
    "version" : "1.2.3",
    "description" : "A packaged foo fooer for fooing foos",
    "main" : "foo.js",
    "man" : [ "./man/foo.1", "./man/bar.1" ]
  }
  ```
  将产生文件来执行man foo和man foo-bar。
  man文件必须以数字结尾，如果压缩了，可添加.gz后缀。这个数字指示文件被安装到man的哪个位置。
  ```
  { "name" : "foo",
    "version" : "1.2.3",
    "description" : "A packaged foo fooer for fooing foos",
    "main" : "foo.js",
    "man" : [ "./man/foo.1", "./man/foo.2" ]
  }
  ```
  将生成man foo和man 2 foo条目

* 目录/directories
  
    commonJS包规范详细介绍了几种使用directories对象展示包结构的方法。在[npm's package.json](https://registry.npmjs.org/npm/latest)中，包含doc、lib和man目录。
    在某些时候，这些信息可能会被使用。

  **directories.lib**
  展示主体库位置。lib文件夹没有任何特殊的功能，但是它是有用的元信息。 

  **directories.bin**
    如果在directories.bin中指定bin目录，则在bin文件夹中的所有文件将被添加。
    由于bin指令的工作方式，同时指定bin路径和设置directories.bin会发生错误。如果要指定单个文件，请使用bin，而制定现有bin目录中的所有文件，请使用directories.bin。 

  **directories.man**
  
  一个存放手册页的文件夹。通过移动文件夹生成一个手册数组。

  **directories.doc**
  
  一个存放标记文件的文件夹。最终，这些可能会被很好地展示。 

  **directories.example**
  
  一个存放示例脚本文件的文件夹。这些可能会以某种方式被暴露出来。

  **directories.test**
  
  一个存放测试文件的文件夹。目前，该功能尚未开放。 

* 仓库/repository

  这个字段，用来指定代码所在的位置。值可为字符串或对象，对象中，可设置type和url属性。指定仓库有利于其他人做贡献。
  
  如果git repo在github上，那么npm docs命令可找到相关信息。 

  举例
  ```
  "repository": {
    "type" : "git",
    "url" : "https://github.com/npm/cli.git"
  }

  "repository": {
    "type" : "svn",
    "url" : "https://v8.googlecode.com/svn/trunk/"
  }
  ```
  url必须是一个公开的（可能是只读的）URL，可以不需要任何修改直接传递给VCS程序。不可以是浏览器访问的HTML页的URL，是因为url是为电脑设计的。

  对于GitHub、GitHub Gist、BitBucket或GitLab仓库，可以使用用于NPM安装的快捷方式语法：
  ```
  "repository": "npm/npm"
  "repository": "github:user/repo"
  "repository": "gist:11081aaa281"
  "repository": "bitbucket:user/repo"
  "repository": "gitlab:user/repo"
  ```
  如果包的package.json不在根目录中（例如，它是monorepo的一部分），则可以指定它所在的目录：
  ```
  "repository": {
   "type" : "git",
   "url" : "https://github.com/facebook/react.git",
   "directory": "packages/react-dom"
  }
  ```
* 脚本命令/scripts

  这个字段，是一个字典对象，包含包生命周期不同阶段所运行的script命令，键是生命周期事件，值是这个事件运行所需的命令

  更多包scripts信息请查阅[npm-scripts](https://docs.npmjs.com/misc/scripts)

* 脚本配置/config

  这个字段，是一个对象，可用于设置包脚本中使用的配置参数，这些脚本在升级过程中保持不变。
  
  举例，如果一个包具有以下内容：
  ```
  { "name" : "foo", 
    "config" : { "port" : "8080" }
  }
  ```
  同时，定义一个start命令，这个命令引用了npm_package_config_port环境变量，然后用户可以用**npm config set foo:port 8001**来覆盖环境变量的port。 

  更多包配置信息，请查阅[npm-config](https://docs.npmjs.com/misc/config)、[npm-scripts](https://docs.npmjs.com/misc/scripts)

* 依赖包/dependencies
  
  该字段，是一个简单的对象，以包名:包版本范围形式来保存工程依赖包。依赖包也可以是tarball文件或Git URL。
  
  注意：请不要在dependencies中放置依赖包测试框架或转换器。请参阅*devdependencies*。
  
  包版本范围是一个字符串，其中有一个或多个空格分隔的描述符。有关指定版本范围的详细信息，请参见[semver](https://docs.npmjs.com/misc/semver)。   
   * version 必须与版本完全匹配 
   * \>version 必须大于版本
   * \>=version 必须大于等于版本
   * \<version 必须小于版本
   * \<=version 必须小于等于版本
   * ~version “近似等同于版本”见[semver](https://docs.npmjs.com/misc/semver)
   * ^version “与版本兼容”见[semver](https://docs.npmjs.com/misc/semver)
   * 1.2.x 1.2.0,1.2.1，etc.,but not 1.3.0
   * http://… 请参阅“URLs as Dependencies”
   * * 匹配任何版本
   * "" 空字符串与\*相同
   * version1 - version2 与>=version1 <=version2相同，介于版本1和版本2间
   * range1 || range2 只要满足范围1或范围2即可
   * git… 请参阅“URLs as Dependencies”
   * user/repo 请参见“Github URL”
   * tag 已标记并发布为标记的特定版本，请参见[npm-dist-tag](https://docs.npmjs.com/cli/dist-tag)
   * path/path/path 请参阅“localPaths”
   
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
  
  可以指定tarball URL来代替版本范围。这个tarball将被下载并安装到本地包中。
  
  **Git URLs as Dependencies**
  
  Git URLs的形式如下：
  ```
  <protocol>://[<user>[:<password>]@]<hostname>[:<port>][:][/]<path>[#<commit-ish> | #semver:<semver>]
  ```
  \<protocol\>可以为这些值：git、git+ssh、git+http、git+https、git+file.
   
    如果Git URLs中包含#\<commit-ish\>，则精确复制发生这个提交时的包。
    如果commit-ish包含#semver:\<semver\>，\<semver\>可以是任何有效的semver范围或精确的版本，并且NPM将在远程仓库中查找与这个范围匹配的任何标记或引用。
    如果未指定#\<commit-ish\>或#semver:\<semver\>，则使用master。
   
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
