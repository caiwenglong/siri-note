## 制作 button 组件需要功能

```ts
interface IButtonProps {
  type: string; // 组题样式
  icon?: string; // 组件配套的ICON
  position: string; // 图标的位置
  enableThrottle: boolean; // 是否开启节流
  delay: number; // 节流延迟的时间
  disabled: boolean; // 是否可点击
  loading: boolean; // 是否是loading 按钮
}
```

### 组题样式 type

```ts
// 默认有以下6种样式
const typeArray = ['default', 'primary', 'success', 'info', 'warning', 'danger']

// 规定传入的type只能是以下单词其中的一种
type IButtonType = PropType<'default' | 'primary' | 'success' | 'info' | 'warning' | 'danger'>

props: {
  type: {
    //  接收的type属性只能是字符串 且 只能是 'default' | 'primary' | 'success' | 'info' | 'warning' | 'danger' 其中的一种
    type: String as IButtonType,
    default: 'default', // 默认是 default

    // 如果不让用户进行样式类型扩展， 则使用validator进行限制
    // 如果传入的值不在 typeArray 中， 那么就直接抛出错误提示
    validator(type: string) {
      if (!typeArray.includes(type)) {
        throw Error(
          '类型“type”参数值错误，值只能是' +
            typeArray.join('、') +
            '中的一种。'
        )
      }
      return true
    }
  }
}
```

### icon 图标
```ts
const positionArray = ['left', 'right']
type IButtonPosi = PropType<'left' | 'right'>
props: {
  icon: String, // 图标的class
  // 图标的位置
  position: {
      type: String as IButtonPosi, // 值只能是字符串类型的 且 只能是 left 和 right 中的一个
      default: 'left', // 默认是 left
      require: false, // 不是必须要传值
      // 如果传入的值不是 left 或者 right 那么就直接抛出错误
      validator(type: string) {
        if (!positionArray.includes(type)) {
          throw Error(
            `属性“position”传入的值错误，值只能是${positionArray.join(
              '、'
            )}中的一种。`
          )
        }
        return true
      }
    },
}
```

### 是否开启节流模式
:::tip
 节流模式：用于限制按钮触发的频率，每一个delay 间隔时间内只能触发一次事件，
          如果是在delay期间触发的，那么就会失效，第一次的触发会`立即`执行函数
:::
```ts
props: {
  enableThrottle: boolean, // 是否开启节流模式
  delay: number // 延迟的时间，单位为毫秒
}
```

### 是否可点击 
```ts
props: {
  disabled: boolean
}
```

### 是否是加载状态
```ts
props: {
  loading: boolean
}
```