# vscode使用方法

## 快捷键

> - 打开终端面板：command + j
> - 复制一行：shift + option + ⇩

## 插件安装

> - <font color=steelblue>vetur：</font>  <font color=#7ec699 blue>.vue 文件语法识别</font>


## vscode 格式化代码 与 eslint 有冲突的问题解决
解决方法：在setting.json中加入下面的配置就可以了

```js 
{
   "vetur.format.defaultFormatterOptions": {
      "prettier": {
        "semi": false,
        "singleQuote": true
      },
      "wrap_attributes": "force-aligned"
    },
    "javascript.format.insertSpaceBeforeFunctionParenthesis": true,
    "vetur.format.defaultFormatter.js": "vscode-typescript",
    "vetur.format.defaultFormatter.html": "js-beautify-html" 
}
```

但是这个时候会发现vue文件是可以了，可是js文件还是无效，找到以下方法解决之：
1、安装prettier插件

```js
npm install --save-dev  prettier

```
2、在根目录新增 .prettierrc.json文件，配置如下：

```js
{
	"singleQuote":true,//使用单引号而不是双引号,true就是对
	"semi":false//在语句结尾处打印分号,false就是不打印
}

```

### 常用插件

```
名称	简述
Auto Close Tag	自动闭合HTML标签
Auto Import	Typescript自动import提示
Auto Rename Tag	修改HTML标签时，自动修改匹配的标签
Beautify css/sass/scss/less	css/sass/less格式化
Better Comments	编写更加人性化的注释
Bookmarks	添加行书签
Can I Use	HTML5、CSS3、SVG的浏览器兼容性检查
Code Runner	运行选中代码段（支持大量语言，包括Node）
Code Spellchecker	单词拼写检查
CodeBing	在VSCode中弹出浏览器并搜索，可编辑搜索引擎
Color Highlight	颜色值在代码中高亮显示
Color Info	小窗口显示颜色值，rgb,hsl,cmyk,hex等等
Color Picker	拾色器
Document This	注释文档生成
ESLint	ESLint插件，高亮提示
EditorConfig for VS Code	EditorConfig插件
Emoji	在代码中输入emoji
File Peek	根据路径字符串，快速定位到文件
Font-awesome codes for html	FontAwesome提示代码段
Git Blame	在状态栏显示当前行的Git信息
Git History(git log)	查看git log
GitLens	显示文件最近的commit和作者，显示当前行commit信息
Guides	高亮缩进基准线
Gulp Snippets	Gulp代码段
HTML CSS Class Completion	CSS class提示
HTML CSS Support	css提示（支持vue）
HTMLHint	HTML格式提示
Indenticator	缩进高亮
IntelliSense for css class names	css class输入提示
JavaScript (ES6) code snippets	ES6语法代码段
JavaScript Standard Style	Standard风格
Less IntelliSense	less变量与混合提示
Lodash	Lodash代码段
MochaSnippets	Mocha代码段
Node modules resolve	快速导航到Node模块
Code Outline	展示代码结构树
Output Colorizer	彩色输出信息
Partial Diff	对比两段代码或文件
Path Autocomplete	路径完成提示
Path Intellisense	另一个路径完成提示
PostCss Sorting	css排序
Prettify JSON	格式化JSON
Project Manager	快速切换项目
Quokka.js	不需要手动运行，行内显示变量结果
REST Client	发送REST风格的HTTP请求
React Native Storybooks	storybook预览插件，支持react
React Playground	为编辑器提供一个react组件运行环境，方便调试
React Standard Style code snippets	react standar风格代码块
Sass	sass插件
Settings Sync	VSCode设置同步到Gist
Sort Typescript Imports	typescript的import排序
Sort lines	排序选中行
String Manipulation	字符串转换处理（驼峰、大写开头、下划线等等）
Syncing	vscode设置同步到gist
TODO Parser	Todo管理
TS/JS postfix completion	ts/js前缀提示
TSLint	TypeScript语法检查
Test Spec Generator	测试用例生成（支持chai、should、jasmine）
TypeScript Import	TS自动import
TypeSearch	TS声明文件搜索
Types auto installer	自动安装@types声明依赖
VSCode Great Icons	文件图标拓展
Version Lens	package.json文件显示模块当前版本和最新版本
View Node Package	快速打开选中模块的主页和代码仓库
VueHelper	Vue2代码段（包括Vue2 api、vue-router2、vuex2）
filesize	状态栏显示当前文件大小
ftp-sync	同步文件到ftp
gitignore	.gitignore文件语法
htmltagwrap	快捷包裹html标签
language-stylus	Stylus语法高亮和提示
markdownlint	Markdown格式提示
npm Intellisense	导入模块时，提示已安装模块名称
npm	运行npm命令
stylelint	css/sass/less代码风格
vetur	目前比较好的Vue语法高亮
vscode-database	操作数据库，支持mysql和postgres
vscode-icons	文件图标，方便定位文件
vscode-random	随机字符串生成器
vscode-styled-components	styled-components高亮支持
vscode-styled-jsx	styled-jsx高亮支持
```