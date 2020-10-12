# 创建型的经典设计模式    

### 原型模式  
> 原型模式不仅是一种设计模式, 他还是一种编程范式. 在原型模式下, 当我们想要创建一个对象时, 会先找到一个对象作为原型, 然后通过克隆原型
的方式来创建出一个与原型一样(共享一套数据/方法)的对象. 

*在 JavaScript 中, Object.create() 方法就是原型模式的天然实现.* 准确的说只要我们还在借助 prototype 来实现对象的创建和原型的继承, 
那么我们就是在应用原型模式.   

#### 1.JavaScript 中的 "类"  
在ES6中引入的类的定义class, 但他实质上是一个基于 JavaScript 现有的原型继承的语法糖, 而不是真的为 JavaScript 引入了面向对象继承模型.
```javascript
// es6 中声明一个类
class Dog{
  constructor(name, age){
    this.name = name
    this.age = age
  }
  // 类实例方法
  eat(){
    console.log('Dog Eat!')
  }
}

// 上面这个声明的实质实际上是下面方法的语法糖, 两者完全等价
function Dog(name, age) {
  this.name = name
  this.age = age
}
Dog.prototype.eat = function() {
  console.log('Dog Eat!')
}

```
原型模式的出现是为了实现类型之间的解耦. 而 JavaScript 本身是弱类型语言, 不存在类型耦合问题, 所以在 JavaScript 中把原型模式作为一种
编程范式来讨论更合适.
> 编程范式 简单的说就是某一类典型的编程风格 编程范式一般包括三个方面，以OOP为例：
1. 学科的逻辑体系——规则范式：如类/对象、继承、动态绑定、方法改写、对象替换等等机制。
2. 心理认知因素——心理范式：按照面向对象编程之父Alan Kay的观点，“计算就是模拟”。OO范式极其重视隐喻（metaphor）的价值，通过拟人化，按照自然的方式模拟自然。
3. 自然观/世界观——观念范式：强调程序的组织技术，视程序为松散耦合的对象/类的集合，以继承机制将类组织成一个层次结构，把程序运行视为相互服务的对象们之间的对话。
简单的说，编程范式是程序员看待程序应该具有的观点。  

**原型编程范式的核心思想就是利用实例来描述对象, 用实例作为定义对象和继承的基础**

#### 2.JavaScript 中的原型  
在 JavaScript 中, 每个构造函数都用于一个 prototype 属性, 他指向构造函数的原型对象, 这个原型对象中有一个 constructor 属性指回构造函数;
每个实例都有一个 \_\_proto__ 属性指向构造函数的原型对象.  
当我们像上面一样声明一个类时, 他们的实体之间就存在如下图所示的关系  
![原型图1-1](https://github.com/Mrlyk/JavaScriptStudy/blob/master/static/image/原型图1-1.png)  
当我们调用如下两个方法时
```javascript
let dog = new Dog()
dog.eat()
dog.toString()
```
dog 这个实例本身并没有定义这两个方法, 当实例本身并没有时, 他会去搜索原型对象, 而原型对象中刚好有这个方法 eat , 就就行调用; 如果原型
对象中也搜索不到, 就去搜索原型对象的原型对象, 一直搜索到原型对象是 null 为止. 这个搜索的轨迹也就是我们常说的原型链. 如下图所示  
![原型图1-2](https://github.com/Mrlyk/JavaScriptStudy/blob/master/static/image/原型图1-2.png)  

#### 3.对象的深拷贝  
[深拷贝的终极探索](https://segmentfault.com/a/1190000016672263)
```javascript
// 最取巧的使用JSON.stringify(), 这种方式无法拷贝函数和正则值, 如果层级过深会导致栈溢出, 但此方法自身处理了循环引用问题
const origin = {a: 1}
const copy = JSON.parse(JSON.stringify(origin))

// 简单的深拷贝实现, 没有考虑Symbol类型的键, 循环引用, Map, Set 等数据结构. 可以看上面的文章详解
function deepClone(obj) {
  if (typeof obj !== "object" || obj === null){
    return obj
  } 
  let copy = {}
  if (Array.isArray(obj)){
    copy = []
  } 
  for (let k in obj){
    if (obj.hasOwnProperty(k)) {
      copy[k] = deepClone(obj[k])
    }
  } 
  return copy
}
// ps: 判断数据类型的技巧 Object.prototype.toString.call(null) === '[object Null]';

```

**总结**  
JavaScript 中的原型模式因其语言的特性与强类型语言中不同, 更多的是讨论原型范式的使用. 即使用实例来定义和描述对象, 将实例作为继承的基础,
在这个基础上对实例进行扩展. JavaScript 本身也是这么做的, 比如一个 'string' 他本身是没有slice 方法的, 但是在我们调用这个方法时, JavaScript会
创建一个该类型对应的包装对象, new String('string'), 在原型链上寻找 slice 方法, 找到后执行这个方法, 最后执行完销毁该对象.

