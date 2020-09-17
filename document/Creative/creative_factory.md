# 创建型的经典设计模式    

### 工厂模式  
>设计模式的核心操作是观察代码中变与不变的部分，并将它们分离，使可变的部分灵活，不变的部分稳定。工厂模式其实就是将创建对象的过程单独封装
  

**区分变与不变**  
如果在使用构造器模式（构造函数初始化对象）的时候，我们本质上是去抽象了每个对象实例的变与不变（参数值改变， 拥有的属性不变）。
那么使用工厂模式时，我们要做的就是去抽象不同构造函数（类）之间的变与不变。

#### 1.简单工厂

```javascript
function Coder(name , age) {
    this.name = name
    this.age = age
    this.career = 'coder' 
    this.work = ['写代码','写系分', '修Bug']
}
function ProductManager(name, age) {
    this.name = name 
    this.age = age
    this.career = 'product manager'
    this.work = ['订会议室', '写PRD', '催更']
}
```
如上两个构造函数，他们都拥有4个属性name, age, career, work的共性， 但是每个人又有不同的个性值，
如果要求更多career 或 work类型，那就要写更多的构造函数，非常的麻烦。
在找到他们的共性（不变）与个性（变）之后, 就可以使用工厂模式进行封装
```javascript
function User(name , age, career, work) {
    this.name = name
    this.age = age
    this.career = career 
    this.work = work
}
// 工厂
function Factory(name, age, career) {
    let work
    switch(career) {
        case 'coder':
            work =  ['写代码','写系分', '修Bug'] 
            break
        case 'product manager':
            work = ['订会议室', '写PRD', '催更']
            break
        case 'boss':
            work = ['喝茶', '看报', '见客户']
            break
        //case 'xxx':
            // 其它工种的职责分配
            // ...
            }
    return new User(name, age, career, work)
}
```
总结一下：工厂模式的目的，就是为了实现无脑传参，就是为了爽！

工厂模式是将创建对象的过程单独封装，有构造函数的地方我们就应该想到工厂函数，如果一个地方写了大量构造函数和使用了大量的new就可以考虑
是否可以用工厂模式重构了。

#### 2.抽象工厂  
抽象工厂用来处理更复杂的类, 其核心思想依然是"变与不变"的区分, 将一个类一层层的剥离. 从抽象工厂到具体工厂 ,从抽象产品到具体产品. 抽象
类只约束类最基本的组成, 及类最基本的相同点, 其他什么也不做. 然后一层层往下细分粒度, 这样在你要拓展产品的功能的时候, 就只需要对具体工厂
进行拓展, 而不会对原有的工厂造成任何影响. 这也符合设计原则中的 "开放封闭" 原则, 该开放的开放, 该封闭的封闭, 互不影响, 降低耦合.  

举例如下: 生产一个各种品牌手机的组装工厂
```javascript
// 首先创建一个手机的抽象工厂
class PhoneFactory{
  // 包含创建手机的基本组成 系统, 硬件
  createOS(){
    throw new Error('抽象工厂方法, 不能调用, 需要具体工厂重写')
  }
  createHardWare(){
    throw new Error('抽象工厂方法, 不能调用, 需要具体工厂重写')
  }
}

// 创建一个操作系统的抽象产品工厂, 包含操作系统行为
class OS{
  controlHardWare(){
    throw new Error('抽象产品方法, 不能调用, 需要具体产品工厂重写')
  }
}

// 生产一个安卓系统, 包含具体操作型为 - 具体产品
class AndroidOS extend OS{
  controlHardWare(){
    console.log('使用安卓的方式操作硬件')
  }
}

// 同样生产一个苹果系统 - 具体产品
class IOS extend OS{
  controlHardWare(){
    console.log('使用苹果的方式操作硬件')
  }
}

// 创建一个硬件的抽象产品工厂, 包含硬件行为
class HardWare{
  controlOS(){
    throw new Error('抽象产品方法, 不能调用, 需要具体产品工厂重写')
  }
}

// 生产一个高通硬件 - 具体产品
class QualCommHardWare extend HardWare{
  controlOS(){
    console.log('使用高通的方式运转')
  }
}

// 生产一个华为硬件 - 具体产品
class HuaWei extend HardWare{
  controlOS(){
    console.log('使用华为的方式运转')
  }
}

// 到此最顶层的PhoneFactory抽象工厂的基本组成也被实现了, 接下来就可以随意生产各种品牌的手机了
// 生产一个具体品牌 Mi 牌的手机 - 具体工厂
class MiPhone extend PhoneFactory{
  // 具体的手机创建方法
  // 使用安卓操作系统
  createOS(){
    return new AndroidOS()
  }
  // 使用高通硬件
  createHardWare(){
    return new QualCommHardWare()
  }
}
// 接下来无论我们要新增生产什么品牌 都可以直接在顶层的抽象工厂上进行拓展, 也不会对原产品线造成任何影响, 完美的符合"开放封闭"原则

// 使用创建一个 MiPhone
const phone = new MiPhone()
// 给他操作系统
const os = phone.createOS()
// 给他硬件
const hd = phone.createHardWare()
// 启动操作系统
os.controlHardWare() // 使用安卓的方式操作硬件
// 启动硬件
hd.controlOS() // 使用高通的方式运转

```
**总结**  
从上面的例子可以看出, 一个抽象工厂是为了解决更复杂的类, 将其一层层剥离, 分离出其中的"变与不变"    
最顶层的抽象工厂只约束最基本的组成, 到后面由抽象产品工厂来做更细一层的约束, 最后到具体产品工厂来实现我们的具体产品.围绕一个顶层的超级
工厂来创建具体工厂.
- 抽象工厂（抽象类，它不能被用于生成具体实例）： 用于声明最终目标产品的共性。在一个系统里，抽象工厂可以有多个（大家可以想象我们的手机厂后来被一个更大的厂收购了，这个厂里除了手机抽象类，还有平板、游戏机抽象类等等），每一个抽象工厂对应的这一类的产品，被称为“产品族”。
- 具体工厂（用于生成产品族里的一个具体的产品）： 继承自抽象工厂、实现了抽象工厂里声明的那些方法，用于创建具体的产品的类。
- 抽象产品（抽象类，它不能被用于生成具体实例）： 上面我们看到，具体工厂里实现的接口，会依赖一些类，这些类对应到各种各样的具体的细粒度产品（比如操作系统、硬件等），这些具体产品类的共性各自抽离，便对应到了各自的抽象产品类。
- 具体产品（用于生成产品族里的一个具体的产品所依赖的更细粒度的产品）： 比如我们上文中具体的一种操作系统、或具体的一种硬件等。
