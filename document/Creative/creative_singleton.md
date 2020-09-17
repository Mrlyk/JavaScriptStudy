# 创建型的经典设计模式    

### 单例模式 
> 保证一个类仅有一个实例, 并提供一个全局访问他的访问点, 这也模式就叫单例模式.
  
*创建简单的单例模式类*
```javascript
class SingleDog {
  static getInstance(){
    if (!SingleDog.instance) {
      SingleDog.instance = new SingleDog()
    }
    return SingleDog.instance
  }
}

let s1 = SingleDog.getInstance()
let s2 = SingleDog.getInstance()
s1 === s2 // true
```

#### 1.


**总结**  

