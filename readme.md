# JavaScript 设计模式核心原理与实践应用  

#### 思维导图  
<img src="./static/imgae/思维导图.png" width="600">   


### 一.理解设计模式  
>设计模式是解决一类问题的工具，贯彻软件工程中"拿来主义"的核心， 类似于数学中解决问题的各类公式。
每一个模式描述了一个在我们周围不断重复发生的问题以及解决该问题的核心。

运用到实践中的方式也与解决数学问题类似：  
识别问题特征 --\> catch问题要运用的知识点  --\> 在脑海中映射出相应的模式策略  
  

**SOLID设计原则**  
>"SOLID" 是由罗伯特·C·马丁在 21 世纪早期引入的记忆术首字母缩略字，指代了面向对象编程和面向对象设计的五个基本原则。

*设计原则是设计模式的指导理论*，它可以帮助我们规避不良的软件设计。SOLID 指代的五个基本原则分别是：  
- 单一功能原则（Single Responsibility Principle）
- 开放封闭原则（Opened Closed Principle）
- 里式替换原则（Liskov Substitution Principle）
- 接口隔离原则（Interface Segregation Principle）
- 依赖反转原则（Dependency Inversion Principle）  

*在JS的设计模式中主要用到的是 “单一功能” 和 “开放封闭” 这两个原则*

**设计模式的核心思想——封装变化**  
在实际开发中，不发生变化的代码可以说是不存在的。业务会随着需求随时变动，我们能做的只有将这个变化造成的影响最小化 —— 将变与不变分离，**确保变化的部分灵活、不变的部分稳定**。
这个过程，就叫“封装变化”；这样的代码，就是我们所谓的“健壮”的代码，它可以经得起变化的考验。而设计模式出现的意义，就是帮我们写出这样的代码。  


**设计模式的术**  
*23种设计模式可按照“创建型”、“行为型”和“结构型”进行划分。*  
#### 创建型  
> 创建型封装了创建对象过程中的变化，比如工厂模式，将创建对象的过程剥离；
- [工厂模式](https://github.com/Mrlyk/JavaScriptStudy/blob/master/document/Creative/creative_factory.md)  
- [单例模式](https://github.com/Mrlyk/JavaScriptStudy/blob/master/document/Creative/creative_singleton.md)

#### 行为型
> 行为型对对象千变万化的行为进行抽离，确保我们能够更安全、更方便地对行为进行更改；  
- *[行为型](https://github.com/Mrlyk/JavaScriptStudy/blob/master/document/Behavioral/behavioral.md)*  

#### 结构型  
> 结构型模式封装的是对象之间组合方式的变化，目的在于灵活地表达对象间的配合与依赖关系。  
- *[结构型](https://github.com/Mrlyk/JavaScriptStudy/blob/master/document/Structural/structural.md)*


---
