# ref 只处理了普通类型的情况，对象的情况还没有处理

## 逻辑梳理
1. 创建一个类
2. 采用属性访问器进行访问，
3. get中手机依赖，set中触发依赖更新


## 伪代码实现
```js
      /* 类中还有其他的属性，具体可以看 Vue 源码 */
    class createRefImpl {
        public _value 
        constructor(target) {
            this.target = target 
            this._value =target
        }
        get value (){
            // 收集依赖的代码
            return this._value
        }

        set value (newValue){
            // 判断老值和新值是否一样，一样的话就直接返回，这里写不一样的逻辑
            // 触发依赖 的代理
            this._value = newValue
        }
    }

    export function ref(target){
        return createRefImpl(target)
    }
```

**比较重要的是依赖的收集以及依赖的触发**