# 让scss文件生效

```js
// 1、安装loader
// yarn add -D sass-loader@7.3.1 node-sass@4.14.1 // 安装scss-lodaer
 
// 2、添加index.scss文件到/docs/.vuepress/styles/index.scss
// 3、在/docs/.vuepress/enhanceApp.js文件中输入以下内容
import './styles/index.scss';

export default ({
  Vue, // the version of Vue being used in the VuePress app
  options, // the options for the root Vue instance
  router, // the router instance for the app
  siteData // site metadata
}) => {}
```

<sr-button></sr-button>