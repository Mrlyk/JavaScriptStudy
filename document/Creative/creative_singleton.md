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

*使用闭包实现*  
```javascript
class SingleDog{}

const SingleDog.getInstance = (function() { // 注意要形成闭包. 这里是自执行函数, SingleDog 这个变量保持了对返回值的引用
  let instance = null
  return function() {
    // 该子函数的词法环境包含了对 instance 这个变量的引用, instance 不会被销毁
    if (!instance){
      instance = new SingleDog()
    } 
    return instance
  } 
})()
let s1 = SingleDog.getInstance()
let s2 = SingleDog.getInstance()
s1 === s2 //true
```

#### 1.Vuex 中对单例模式的实现  
Vuex 作为 vue 的全局状态管理工具, 为了保证全局只有一个 Store 来保存应用的状态, 使用的就是单例模式  
```javascript
// 引入vuex
Vue.use(vuex)
new Vue({
  el: '#app',
  store,
})

// 源码中对 vuex install 的实现
let Vue // 这个Vue的作用和上面的instance作用一样
...

export function install (_Vue) {
  // 判断传入的Vue实例对象是否已经被install过Vuex插件（是否有了唯一的state）
  if (Vue && _Vue === Vue) {
    if (process.env.NODE_ENV !== 'production') {
      console.error(
        '[vuex] already installed. Vue.use(Vuex) should be called only once.'
      )
    }
    return
  }
  // 若没有，则为这个Vue实例对象install一个唯一的Vuex
  Vue = _Vue
  // 将Vuex的初始化逻辑写进Vue的钩子函数里
  applyMixin(Vue)
}

```

#### 2.使用单例模式实现一个全局模态框  
前端最初对单例模式的使用也是这样一个全局的模态框, 使用单例模式可以保证全局只有唯一的一个模态框  
```javascript
class Modal{
  static getModal(){
    if (!Modal.modal){ // 这个静态属性是全局唯一的
      let modal
      modal = document.createElement('div')
      modal.innerHTML = '这是个模态框'
      modal.id = 'modal'
      modal.style.display = 'none'
      document.body.appendChild(modal)
      Modal.modal = modal
    } 
    return Modal.modal
  }
}

function open(){
  const modal = Modal.getModal()
  modal.style.display = 'block'
}

function close(){
  const modal = Modal.getModal()
  modal.style.display = 'none'
}
```

**总结**  
单例模式的作用就是用来保证全局只有一个我们需要的对象, 某些场景下某个对象也只能全局存在一个. 同时也能减少对象创建和内存开销.

