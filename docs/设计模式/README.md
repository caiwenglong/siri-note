## 设计模式

### 六大原则

- 单一职责原则
  - 一个程序只做好一件事（如果功能过于复杂就拆分开，每个部分保持独立）
- 开放/封闭原则

  - 对扩展开放，对修改封闭（在增加需求时应扩展代码而不是修改已有的代码）

- 里氏替换原则

  - 子类能覆盖父类
  - 父类能出现的地方，子类也能出现

- 接口隔离原则

  - 保持接口的单一独立（类似单一职责原则）

- 依赖倒转原则

  - 面向接口编程，依赖于抽象而不依赖于具体（使用方只关注接口而不关注具体类的实现）

- 迪米特原则
  - 要降低耦合

::: tip 列如：
promise 就遵循了 单一职责原则和开放封闭原则
单一职责原则：每个 then 中的逻辑就只做一件事
开放封闭原则：如有新增需求则扩展 then 而不是修改原来的 then
:::

### 设计模式分类

#### 创建型

- 单列模式
- 原型模式
- 工厂模式
- 抽象工厂模式
- 建造者模式

##### 单列模式

:::tip 描述:
一个类就只有一个实例，并且提供一个访问它的全局访问点
:::

```js
class LoginForm {
  constructor() {
    this.state = "hide";
  }
  show() {
    if (this.state === "show") {
      alert("已经显示");
      return;
    }
    this.state = "show";
    console.log("登录框显示成功");
  }
  hide() {
    if (this.state === "hide") {
      alert("已经隐藏");
      return;
    }
    this.state = "hide";
    console.log("登录框隐藏成功");
  }
}
LoginForm.getInstance = (function() {
  let instance;
  return function() {
    if (!instance) {
      instance = new LoginForm();
    }
    return instance;
  };
})();

let obj1 = LoginForm.getInstance();
obj1.show();

let obj2 = LoginForm.getInstance();
obj2.hide();

console.log(obj1 === obj2);
```

##### 工厂模式
简单的工厂列子：
```js
// 定义产品
class Product {
  constructor(name) {
    this.name = name
  }
  init() {
    console.log("初始化...")
  }
}

// 定义工厂
class Factory() {
  create(name) {
    return new Product(name)
  }
}

// 实例化工厂对象
const factory = new Factory()
// 通过实例化出来的 工厂对象创建出 car对象
const car = factory.create('car')
car.init()
```

#####

#####
