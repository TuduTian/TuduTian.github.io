# fluuter中的wieget使用
## 路由使用
1. 配置 MaterialApp 中的routes属性 他为一个map对象
2. initialRoute代表的是初始化的路由

> 代码实现
```dart
  class MyApp extends StatelessWidget {
    const MyApp({super.key});
    @override
    Widget build(BuildContext context) {
      return MaterialApp(
        //重点
          initialRoute: '/',
          routes: {
            "/": (ctx) => const ConfigPage(),
            "/news": (ctx) => const News(),
          },
          builder: EasyLoading.init(),
          theme: ThemeData(primarySwatch: ThemeConfig.bgColor));
    }
}
```
3.页面跳转 在触摸事件中使用 Navigator.pushNamed的方式进行跳转 因为这里写了路由的名称了，所以可以使用
```dart
 //build 
 @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('ios 测试'),
      ),
      body: Center(
        child: Column(
          children: [
            ElevatedButton(
                onPressed: () {
                  // 重点api
                  Navigator.pushNamed(context, '/news');
                },
                child: const Text('news page'))
          ],
        ),
      ),
    );
  }
```

4.如何进入详情页？ 方式有很多种这里举的例子数据套模版
 - 先写一个模版 wieget 它里面的内容都是外部传递过来的
 - 我们在进行循环点击列表的时候通过 Navigator.of(context).push() 的方法去构建一个routes出来就可以了
```dart
              onTap: () {
                // 因为她只是模版，所以没有去设置路由 通过 MaterialPageRoute builder 方法去构建出来一个模版 传递数据实现的动态路由
                  Navigator.of(context).push(MaterialPageRoute(
                      builder: (ctx) => NewsDetail(
                            post:item,
                          )));
                },
```






# wieget 的特殊使用
## 蒙层效果？触摸时会有一个水波纹从内向外的展出
1. AspectRatio 这个组件是让图片按照指定比例进行裁剪
2. InkWell 可以有一个触摸事件 她的属性  splashColor  highlightColor 可以设置水波纹效果的透明度一系列设置
3. Positioned.fill 如果没有设置的话 她默认是填充满的 Positioned.fill方法将填充Stack的整个空间。
```dart
      Stack(children: [
            Card(
              child: Column(
                children: [
                  /* 
                  AspectRatio 这个部件设置显示比例 这里设置的是16/9
                   */
                  AspectRatio(
                    aspectRatio: 16 / 9,
                    child: Image.network(
                      item['image'],
                      fit: BoxFit.cover,
                    ),
                  ),
                  Text(item['name']),
                  Text(item['age']),
                  const SizedBox(
                    height: 10,
                  )
                ],
              ),
            ),
            Positioned.fill(
                child: Material(
              color: Colors.transparent,
              child: InkWell(
                splashColor: Colors.white.withOpacity(0.3),
                highlightColor: Colors.white.withOpacity(0.1),
                onTap: () {
                  Navigator.of(context).push(MaterialPageRoute(
                      builder: (ctx) => NewsDetail(
                            post:item,
                          )));
                },
              ),
            ))
          ]);
```
