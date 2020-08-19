# 创建型的经典设计模式    

### 一.工厂模式  
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
            ...
            }
    return new User(name, age, career, work)
}
```
总结一下：工厂模式的目的，就是为了实现无脑传参，就是为了爽！

工厂模式是将创建对象的过程单独封装，有构造函数的地方我们就应该想到工厂函数，如果一个地方写了大量构造函数和使用了大量的new就可以考虑
是否可以用工厂模式重构了。

#### 2.抽象工厂  

