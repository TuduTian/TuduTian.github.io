

## ts中的装饰器
### 类装饰器 主要是通过@符号添加装饰器
```ts
function decotators(target: any) {
  // target 拿到的是Person 可以给prototype添加属性或者是方法
  target.prototype.name = 'tzc';
}

@decotators
class Xiaoman {}
const xiaoman: any = new Xiaoman();
console.log(xiaoman.name); //tzc
```

### 属性装饰器 使用@符号给属性添加装饰器
> 他会返回两个参数 1.原形对象  2.属性的名称
```ts
  {
  function tzc(target: any, key: string | symbol) {
    /* 
      target 原型对象
      key 当前的key
    */
    console.log(target, key);
    target[key] = 'tzc'; // 这里修改了
  }
  class Person {
    @tzc
    public name: string;
  }

  const p: any = new Person();
  console.log(p.name); //tzc
}
```

### 参数装饰器 样使用@符号给属性添加装饰器
>  他会返回三个参数 1.原形对象 2.方法的名称  3.参数的位置从0开始
```ts
{
  const tzc = (target, key, index) => {
    /* 
      target: 原型对象
      key 方法的名称
      index 当前参数所在的位置， 0代表是第一个参数 1代表是第二个参数
      注意：往后往前去执行的
      */
    console.log(target, key, index);
    target.age = 'tzc';
  };

  const tzc3 = (target, key, index) => {
    console.log(target, key, index);
    target.name = '123123';
    target.age = 66;
  };

  class Person {
    getName(@tzc name, @tzc age, @tzc3 path) {
      console.log(name);
    }
  }
  const p: any = new Person();
  console.log(p.name);
}

```


##  装饰器全部案例代码

```ts
{
  function decotators(target: any) {
    target.prototype.name = '小满';
  }

  @decotators
  class Xiaoman {}
  const xiaoman: any = new Xiaoman();
}

{
  function tzc(target: any, key: string | symbol) {
    /* 
      target 原型对象
      key 当前的key
    */
    target[key] = 'tzc'; // 这里修改了
  }

  class Person {
    @tzc
    public name: string;
  }

  const p: any = new Person();
}

/* 参数装饰器demo */
{
  const tzc = (target, key, index) => {
    /* 
      target: 原型对象
      key 方法的名称
      index 当前参数所在的位置， 0代表是第一个参数 1代表是第二个参数
      注意：往后往前去执行的
      */
    console.log(target, key, index);
    target.age = 'tzc';
  };

  const tzc3 = (target, key, index) => {
    console.log(target, key, index);
    target.name = '123123';
    target.age = 66;
  };

  class Person {
    getName(@tzc name, @tzc age, @tzc3 path) {
      console.log(name);
    }
  }
  const p: any = new Person();
  console.log(p.name);
}

```
