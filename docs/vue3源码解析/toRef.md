**注意： 该md文档代码是伪代码,只是为了帮助理解**

# toref
## 功能
> 用来结构 对象中的一个属性 如果这个对象是响应式的，那么该属性也是响应式的，如果这个对象不是响应式的，那么该对象也不是响应式的
```ts
    const state = reactive({name:'tom',age:22})
    const name  = toRef(state,'name')  // 响应式 因为对象是reactive包裹的
    const state  = {name:'tom',age:22}
    const name = toRef(state,'name') //不是响应式的,没有使用 reactive 包裹
```

## 实现步骤
1. 创建一个类，用来保存创建出来的对象的信息
2. 使用类的属性访问器，get以及set
3. 在get的时候返回值，set的时候修改值就可以了

## 代码实现
```ts
  class objectRefImpl {
        constructor(target,key){
            this.target = target
            this.key=key
        }        

        get value (){
            return this.target[this.key]
        }

        set value (newValue){
            this.target[this.key] =  newValue 
        }
    }

    export function toRef(target,key){
        return new objectRefImpl(target,key)
    }
```

# torefs
## 功能
> 对一个对象进行全部属性结构，并且返回这个对象，如果该对象是响应式的，那么他就是响应式的，如果该对象不是响应式的，那么它就不是响应式的

## 代码实现
```ts
  
    funciton isArray (target) {
        return Array.isArray(target)
    }

    export function toRefs (target) {
        // 先判断 target 的类型，数组还是对象
        
        /* 
            ret是后续准备返回的数据
            如果是数组就生成 相同长度的数组 
            如果是对象就生成 一个空的对象
        */
        let ret =isArray(target) ? new Array(target.length) :{}
        for(let key in target) {
            ret[key] = toRef(target,key)
        }
        return ret 
    }
```