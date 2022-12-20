

# effect思路
> 每次在调用effect函数的时候需要传进去两个参数，第一个参数为一个回调函数，也就是执行的函数，第二个参数是一个options，options可以选填一个lazy属性，默认为false，回调函数会立即执行，如果修改成false的话是不会立即执行的

## 依赖收集
> 我们通过reactive加工出来的数据是进行代理过的，每次访问和修改都会走对应的get函数以及set函数,在effect函数中使用响应式数据的时候他会进行一个依赖收集，以免以后数据更新时重新调用函数
### 强引用和弱引用的区别 （Map和Weakmap的比较）
```ts
  /* 使用map构建出来的容器是强引用的，如果你放进去的对象已经赋值为null了，但是内存中的这个对象的引用地址是不会消失的，因为还有map里面在引用他 */
    const obj={name:'tom'}
    const map = new Map ()
    map.set(obj,'any value')
    obj =null
    map.size ()  // 1 

/* 使用 WeakMap 构建出来的容器时弱引用的，放进去的对象已经赋值为null了，内存中就会被垃圾回收机制进行清理 */
    const obj={name:'tom'}
    const map = new WeakMap ()
    map.set(obj,'any value')
    obj =null
     map.size ()  // 0 
```

### weakMap 的介绍
```js
 const weakMap = new WeakMap()  // 创建一个容器，里面的key只能是对象，并且他有助于垃圾回收 
```

### 依赖收集 => 数据结构的介绍
1. 为了保证是弱引用，采用 WeakMap 进行保存
```js
  const Track = new WeakMap()
```
2. 我们传进去的刚好是一个对象
```js
  const state = reactive({name:'tom'})
```
3. Track的key和value
> track中存放的key是用户传进去的对象,value则是应该收集对象中的哪一个属性
例子：
```ts
    const state =reactive({name:'tom'})
    effect(()=>{
        console.log(state.name ) // 应该收集 name属性并且name改变时调用此函数
    })
    Track =  {{name:'tom'} => Map({"name" :Set(fn,fn,fn,fn)})}
```
4. 3中最后一个就是最好我们想要的结果，他如果多个effect函数收集的话，我们可以进行判断并且push进去

### 依赖收集问题之 effect 中再次调用 effect 使用栈结构规避多次嵌套的问题，能准确拿到最外层的 effect 函数
```ts
        let { reactive, effect } = VueReactive
        const state = reactive({ name: '张三', list: { a: 200 } })
        effect(() => {
            state.name
            effect(() => {
                state.list
            })
            state.list.a
        })

        effect(() => {
            state.name
        })


    // 收集到的
  /*  
   () => {
    state.name
        effect(() => {
        state.list
        })
        state.list.a
} '@@@@@@@@@@@'
() => {
                        state.list
                    } '@@@@@@@@@@@'
      () => {
                    state.name
                } '@@@@@@@@@@@'
                
     */
```