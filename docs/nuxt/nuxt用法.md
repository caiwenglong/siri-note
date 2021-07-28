## nuxt

### 路由守卫

路由守卫分为路由前置守卫跟后置守卫

- 前置守卫可通过中间件 middleware 和 插件来实现

  - 全局守卫：nuxt.config 中的 middleware 和 layout 中的 middleware
  - 组件独享守卫：pages 中的 middleware
  - 插件

  ```js
  // nuxt.config.js
  export default {
    plugins: ["~/plugins/router.js"]
  };
  ```

  ```js
  // plugins/router.js
  export default ({ app, redirect, parmas, query, store }) => {
    // 路由全局前置守卫
    app.router.beforeEach((to, from, next) => {
      // 这边next的参数只能传入true 或者 false， 不能跳转到其他页面 比如 next('/login')
      // 如果要跳转到其他页面，得用 redirect 函数进行跳转
      next();
    });

    // 全局路由后置守卫
    app.router.afterEach((to, from) => {});
  };
  ```

- 后置守卫：使用 vue 中的 beforeRouteLeave 钩子来实现 或者使用插件的形式

---

### 数据交互和跨域

数据交互需要安装两个包：@nuxtjs/axios @nuxtjs/proxy

并且需要在 nuxt.config.js 的 modules 中注册@nuxtjs/axios 模块

```js
// nuxt.config.js
modules: [
    // https://go.nuxtjs.dev/axios
    '@nuxtjs/axios',<template>
      <div class="">

      </div>
    </template>
    <script lang="ts">
    import { defineComponent, reactive } from 'vue'
    export default defineComponent({

      name: '',
      setup() {
        const data = reactive({

        })

        return {
          data
        }
      }
    })
    </script>
    <style lang="scss" scoped>

    </style>
  ],

```

### nuxt 使用 scss

> 在nuxt 中使用 ::v-deep className 来获取子元素的 class名称

1. 需要安装 style-resources sass-loader node-sass

   > npm i -D @nuxtjs/style-resources sass-loader node-sass

2. 在 css 中直接引入文件即可
```js
// nuxt.config.js
css: [
  './assets/css/vivify.min.css',
  './assets/scss/index.scss',
  'element-ui/lib/theme-chalk/index.css',
],
```

3. 使用变量和 mixin 即要使 available.scss 中的变量或者 mixin.scss 的变量生效，需要在nuxt.config.js中设置styleResources

```js
// nuxt.config.js
styleResources: {
  scss: ['./assets/scss/variable.scss', './assets/scss/mixin.scss']
},

```

```js
// nuxt.config
// Global CSS: https://go.nuxtjs.dev/config-css
  css: [
    './assets/scss/index.scss',
    'element-ui/lib/theme-chalk/index.css'
  ],

```

### 使用 svg

1、新建 assets/icons/svg 来存放 svg 文件

2、安装 svg-sprite-loader

```shell
yarn add svg-sprite-loader
```

3、创建 components/SvgIcon.vue 组件

```vue
<template>
  <div
    v-if="isExternal"
    :style="styleExternalIcon"
    class="svg-external-icon svg-icon"
    v-on="$listeners"
  />
  <svg v-else :class="svgClass" aria-hidden="true" v-on="$listeners">
    <use :xlink:href="iconName" />
  </svg>
</template>

<script>
export default {
  name: "SvgIcon",
  props: {
    iconClass: {
      type: String,
      required: true
    },
    className: {
      type: String,
      default: ""
    }
  },
  computed: {
    isExternal() {
      return /^(https?:|mailto:|tel:)/.test(this.iconClass);
    },
    //通过iconClass 获取svg文件名
    iconName() {
      return `#icon-${this.iconClass}`;
    },
    svgClass() {
      if (this.className) {
        return "svg-icon " + this.className;
      } else {
        return "svg-icon";
      }
    },
    //返回url请求位置
    styleExternalIcon() {
      return {
        mask: `url(${this.iconClass}) no-repeat 50% 50%`,
        "-webkit-mask": `url(${this.iconClass}) no-repeat 50% 50%`
      };
    }
  }
};
</script>

<style scoped>
.svg-icon {
  width: 1em;
  height: 1em;
  vertical-align: -0.15em;
  fill: currentColor;
  overflow: hidden;
}

.svg-external-icon {
  background-color: currentColor;
  mask-size: cover !important;
  display: inline-block;
}
</style>
```

4、创建 plugins/svg-icon.js 插件

5、配置 nuxt.config.js

```js
// nuxt.config.js

// Nuxt.js允许您在运行Vue.js应用程序之前执行js插件.
// 我们需要在程序运行前配置好这个插件。
plugins:{
  '@/plugins/svg-icon' //注册svg插件文件
},

build: {
    extend (config,ctx) {
      // 排除 nuxt 原配置的影响,Nuxt 默认有vue-loader,会处理svg,img等
      // 找到匹配.svg的规则,然后将存放svg文件的目录排除
      const svgRule = config.module.rules.find(rule => rule.test.test('.svg'))
      svgRule.exclude = [resolve(__dirname, 'assets/icons/svg')]
      //添加loader规则
      config.module.rules.push({
        test: /\.svg$/, //匹配.svg
        include: [resolve(__dirname, 'assets/icons/svg')], //将存放svg的目录加入到loader处理目录
        use: [{ loader: 'svg-sprite-loader',options: {symbolId: 'icon-[name]'}}]
      })
    }
 },
```

6、页面中使用

```html
<!-- icon-class 的值,即svg文件名  -->

<div style="font-size:16px;color:green;">
  <svg-icon icon-class="test" />
</div>
```

### 使用 Lottie

1、安装 vue-lottie

```shell
yarn add vue-lottie
```

2、创建 assets/animation 文件夹存放 json 文件

3、创建 components/Lottie.vue 文件

```vue
<template>
  <div @click="play" :style="style" ref="lavContainer"></div>
</template>

<script>
import lottie from "lottie-web";
export default {
  props: {
    options: {
      type: Object,
      required: true
    },
    height: Number,
    width: Number
  },

  data() {
    return {
      style: {
        width: this.width ? `${this.width}px` : "100%",
        height: this.height ? `${this.height}px` : "100%",
        overflow: "hidden",
        margin: "0 auto",
        cursor: "pointer"
      }
    };
  },
  mounted() {
    console.log(this.options.animationData.default);
    this.anim = lottie.loadAnimation({
      container: this.$refs.lavContainer,
      renderer: "svg",
      // loop: this.options.loop !== false,
      loop: false,
      // autoplay: this.options.autoplay !== false,
      autoplay: false,
      animationData: this.options.animationData.default, // ここだけ変更何故かデフォルトを入れないと動かなかった… (要検証)
      rendererSettings: this.options.rendererSettings
    });
    this.$emit("animCreated", this.anim);
  },
  methods: {
    play() {
      this.anim.play();
    }
  }
};
</script>
```

4. 封装组件
```vue

<template>
  <div class="lottie-value" @mouseenter="handlePlay" @mouseleave="handleStop">
    <span class="lottie__icon">
      <Lottie
        :options="options"
        :width="width"
        :height="height"
        @animCreated="handleAnimation"
      />
    </span>
    <span class="lottie__value">
      {{ value }}
    </span>
  </div>
</template>

<script>
import Lottie from 'com/Lottie.vue'

export default {
  components: {
    Lottie
  },

  props: {
    options: {
      required: true,
      type: Object
    },
    value: {
      type: String,
      default: ''
    },
    width: {
      type: Number,
      default: 24
    },
    height: {
      type: Number,
      default:  24
    }
  },

  methods: {
    handleAnimation (anim) {
      this.anim = anim
    },
    
    handlePlay () {
      this.anim.play();
    },

    handleStop () {
      this.anim.stop();
    }
  }
}
</script>

<style lang="scss" scoped>
.lottie-value {
  display: inline-block;
  font-size: 14px;
  color: $deepGrey;
  padding: 8px;
  cursor: pointer;
  & + & {
    margin-left: 20px;
  }
}
.lottie__icon, 
.lottie__value {
  @include align-center
}
</style>

```

5. 在组件中使用
```vue

<template>
  <div><CellLottieValue :options="avatarOptions" :value="'123123'"></CellLottieValue></div>
</template>

<script>
import * as loAvatar from 'assets/animation/avatar.json'
export default {
  components: {
    CellLottieValue
  },

  data () {
    return {
      avatarOptions: { animationData: loAvatar }
    }
  }


}
</script>

```