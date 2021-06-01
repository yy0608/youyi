---
title: 前端设计模式
date: 2021-05-31 20:35:06
tags:
  - 设计模式
---

#### 一、SOLID 五大设计原则

1. S - 单一职责

2. O - 开放封闭(扩展开放，修改封闭)

3. L - 李氏置换(TS 多态)

4. I - 接口独立(TS 接口单一独立，与外观模式"胖接口"冲突)

5. D - 依赖导致(TS 面向接口编程)

#### 二、23 种设计模式（创建型，组合型，行为型）

1. 创建型（工厂模式、单例模式、原型模式）
2. 结构型（适配器模式、装饰器模式、代理模式、外观模式 等）
3. 行为型（观察者模式、迭代器模式、状态模式 等）

#### 三、1.工厂模式

1. jQuery
2. React.createElement

```
window.$ = function(selector) {
  return new jQuery(selector)
}
```

#### 三、2.单例模式

1. 购物车，登录框
2. React.createElement

```
class Singleton {
    private static instance: Singleton;

    private constructor() { }

    public static getInstance(): Singleton {
        if (!Singleton.instance) {
            Singleton.instance = new Singleton();
        }

        return Singleton.instance;
    }
}

function clientCode() {
    const s1 = Singleton.getInstance();
    const s2 = Singleton.getInstance();

    if (s1 === s2) {
        console.log('Singleton works, both variables contain the same instance.');
    } else {
        console.log('Singleton failed, variables contain different instances.');
    }
}

clientCode();
```

#### 三、3.原型模式

```
class Prototype {
    public primitive: any;
    public component: object;
    public circularReference: ComponentWithBackReference;

    public clone(): this {
        const clone = Object.create(this);

        clone.component = Object.create(this.component);

        clone.circularReference = {
            ...this.circularReference,
            prototype: { ...this },
        };

        return clone;
    }
}

class ComponentWithBackReference {
    public prototype;

    constructor(prototype: Prototype) {
        this.prototype = prototype;
    }
}

function clientCode() {
    const p1 = new Prototype();
    p1.primitive = 245;
    p1.component = new Date();
    p1.circularReference = new ComponentWithBackReference(p1);

    const p2 = p1.clone();
    if (p1.primitive === p2.primitive) {
        console.log('Primitive field values have been carried over to a clone. Yay!');
    } else {
        console.log('Primitive field values have not been copied. Booo!');
    }
    if (p1.component === p2.component) {
        console.log('Simple component has not been cloned. Booo!');
    } else {
        console.log('Simple component has been cloned. Yay!');
    }

    if (p1.circularReference === p2.circularReference) {
        console.log('Component with back reference has not been cloned. Booo!');
    } else {
        console.log('Component with back reference has been cloned. Yay!');
    }

    if (p1.circularReference.prototype === p2.circularReference.prototype) {
        console.log('Component with back reference is linked to original object. Booo!');
    } else {
        console.log('Component with back reference is linked to the clone. Yay!');
    }
}

clientCode();
```

#### 三、4.适配器模式

1. 封装旧接口
2. Vue computed

```
class Adaptor {
  specificRequest() {
    return '香港插头'
  }
}

class Target {
  constructor() {
    this.adaptor = new Adaptor()
  }

  request() {
    const info = this.adaptor.specificRequest()
    return info
  }
}

const target = new Target()
target.request()
```

#### 三、5.装饰器模式

1. 类的装饰器
2. 方法装饰器
3. 访问器装饰器
4. 属性装饰器
5. 参数装饰器

#### 三、6.代理模式

1. 事件代理
2. proxy

#### 三、7.外观模式

1. bindEvent 一个方法包含多种传参模式的"胖接口"

#### 三、8.观察者模式

1. 网页事件绑定
2. Vue 自定义事件
3. Promise

#### 三、9.迭代器模式

#### 三、10.状态模式
