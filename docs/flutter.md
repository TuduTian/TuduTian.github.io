# flutter 的点点滴滴

## flutter中的圆角图片实现方式
1. 方式一
```dart
  Widget _buildImage() {
  //利用fluuter组件 clipOval 裁剪椭圆  
    return ClipOval(
      child: Image.asset(
        AssetsImage.logoJpg,
        width: 150,
        height: 150,
        fit: BoxFit.cover, // 图片的填充方式 覆盖
      ),
    );
  }
```
2. 方式二
```dart
   Container(
      width: 150,
      height: 150,
      /* 圆角图片 */
      decoration: BoxDecoration(
        // 这里用的是container的背景图片 然后设置圆角边框来达成了目的的
        image: const DecorationImage(
            image: ExactAssetImage(AssetsImage.logoJpg), fit: BoxFit.cover),
        color: Colors.red,
        borderRadius: BorderRadius.circular(100),
      ),
    );
```

## flutter中的分割线组件
> Divider 组件 可以设置app中个人中心每一行的的分割线 用它来做简直不要太简单 附dem
```dart
ListView(
        children: [
          ListTile(
            title: const Text('修改密码'),
            leading: const Icon(Icons.lock),
            onTap: () {
              print('click');
            },
          ),
          const Divider(),
          const ListTile(title: Text('优惠卷'), leading: Icon(Icons.access_alarm)),
          const Divider(), //一条线
          const ListTile(
              title: Text('我的订单'), leading: Icon(Icons.propane_tank_sharp)),
          const Divider(),
          const ListTile(title: Text('我的发布'), leading: Icon(Icons.subject)),
          const Divider(),
          const ListTile(title: Text('我的列表'), leading: Icon(Icons.list)),
          const Divider(),
        ],
      ),
```


## flutter 案例部分
### 登录框
> 效果图

<img src="https://download4.caiyun.feixin.10086.cn/storageWeb/servlet/downloadServlet?code=SzAxODExbjN0MVRCMjAxNjgxN3Rva3NMMjhi&un=BDE488008F3DDE00E3975DA8F4051118EF88B5B8A1F9F4F345C1E5F801510EAE&dom=D970&rate=0&txType=0">

> 代码模块
```dart
  import 'package:flutter/material.dart';

class DemoLogin extends StatefulWidget {
  const DemoLogin({super.key});

  @override
  State<DemoLogin> createState() => _DemoLoginState();
}

class _DemoLoginState extends State<DemoLogin> {
  // 控制器
  late TextEditingController _usernameController, _passwordController;
  //焦点控制
  late FocusNode _usernameFocus, _passwordFocus;
  @override
  void initState() {
    // TODO: implement initState
    super.initState();
    _usernameController = TextEditingController();
    _passwordController = TextEditingController();
    _usernameFocus = FocusNode();
    _passwordFocus = FocusNode();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      //  SafeArea  ios刘海屏幕的按钮区域
      body: SafeArea(
        child: Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              Padding(
                padding: const EdgeInsets.only(left: 20, right: 20),
                // 文本编辑器 TextField
                child: TextField(
                  focusNode: _usernameFocus,
                  textInputAction:
                      TextInputAction.next, // 类型，比如 是搜索 还是提交 还是下一步 还是其他的 这个可以控制
                  onSubmitted: (input) {
                    _usernameFocus.unfocus(); //取消焦点
                     // 这里是手机号点击提交那个按钮的时候 让他自身失去焦点，然后把焦点传递给password的那个focus
                    FocusScope.of(context).requestFocus(
                        _passwordFocus);
                  },
                  keyboardType: TextInputType.phone, // 弹出的键盘类型， 这个是弹出专门输入手机号的键盘
                  controller: _usernameController,
                  decoration: const InputDecoration(
                    labelText: '手机号',
                    icon: Icon(Icons.person),
                  ),
                ),
              ),
              Padding(
                padding: const EdgeInsets.only(left: 20, right: 20),
                child: TextField(
                  focusNode: _passwordFocus,
                  obscureText: true, // 隐藏你输入的内容 密码输入框
                  onSubmitted: (input) {
                    _passwordFocus.unfocus(); //失去焦点
                  },
                  textInputAction: TextInputAction.done, // 点击提交时的动作 done 代表完成了，没有下一步的操作了.
                  controller: _passwordController,
                  decoration: const InputDecoration(
                    labelText: '密码',
                    icon: Icon(Icons.lock),
                  ),
                ),
              ),
              ButtonBar(
                alignment: MainAxisAlignment.center,
                children: [
                  ElevatedButton(
                      onPressed: () {
                        String username = _usernameController.text;
                        String pwd = _passwordController.text;
                        print('username:$username 和 password:$pwd');
                      },
                      child: const Text('LOGIN'))
                ],
              )
            ],
          ),
        ),
      ),
    );
  }
}
// end 
```

### chip标签系列
1. 标签系列案例
>> 效果图
<img src="https://download.caiyun.feixin.10086.cn/storageWeb/servlet/GetFileByURLServlet?root=/mnt/wfs72&fileid=K257a286d91f68904e13bf5d14d01e2e29.png&ct=1&type=1&code=3C3BB37CFD54DF0173E93EABEC393343AD443155DC5A026F4E3C0576BEB5E3AE&account=MTgxMW4zdDFUQjIw&p=0&ui=1811n3t1TB20&ci=1811n3t1TB2017720230102184224ga3&userSiteId=usersite-s&cn=chip%E6%95%88%E6%9E%9C%E5%9B%BE&oprChannel=10000034&dom=D970">
>> 案例代码
```dart
  import 'package:flutter/material.dart';

class DemoTags extends StatefulWidget {
  const DemoTags({super.key});

  @override
  State<DemoTags> createState() => _DemoTagsState();
}

class _DemoTagsState extends State<DemoTags> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Chip(
          label: const Text('联盟商家'),
          //  CircleAvatar 可以做成圆形照片,这里的可玩性比较高
          avatar: CircleAvatar(
            backgroundColor: Colors.green.shade900,
            child: const Text('01'),
          ),
          // 右边点击x号的回调函数，如果不写回调函数的话他不会有那个x号，也可以自定义，
          onDeleted: () {
            print('我要被删除了');
          },
        ),
      ),
    );
  }
}
```

2. 可选择标签
> 效果图
<img src="https://download4.caiyun.feixin.10086.cn/storageWeb/servlet/downloadServlet?code=SzIxODExbjN0MVRCMjAxNjgxN3Rva3NyNzBl&un=ADEAF51C4F1D97CACE601B02314847971345EB808DC4E67916057E8B074A2B32&dom=D970&rate=0&txType=0">
> 代码片段
```dart
  import 'package:flutter/material.dart';

class DemoSelect extends StatefulWidget {
  const DemoSelect({super.key});

  @override
  State<DemoSelect> createState() => _DemoSelectState();
}

class _DemoSelectState extends State<DemoSelect> {
  bool _select = false;
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        // 使用 filterChip
        child: FilterChip(
          label: const Text('吃饭'),
          onSelected: (bool value) {
            setState(() {
              _select = value;
            });
          },
          selected: _select,
          selectedColor: Colors.green,
        ),
      ),
    );
  }
}
```

## listView 组件的使用 (解释) 配合 ListTile
```dart
body: ListView(
        children: List.generate(20, (index) {
          return Container(
            padding: const EdgeInsets.only(left: 10),
            child: Column(
              children: [
                ListTile(
                  isThreeLine: false, //是否三行显示
                  leading: const Padding(
                    padding: EdgeInsets.only(top: 6),
                    child: Icon(Icons.person),
                  ), // 前面添加的组件
                  title: const Text('田志超'), // 一级标题
                  subtitle: const Text('17630700315'), // 二级标题
                  minLeadingWidth: 8.toDouble(), // 控制 leading 和 title 之间的距离
                  minVerticalPadding: 6.toDouble(),
                  trailing: const Text('1223'), // 后面添加的组件
                  textColor: Colors.red, // 字体颜色
                  horizontalTitleGap: 20.toDouble(),
                  dense: true, // item 直观感受是整体大小
                ),
                const Divider(),
              ],
            ),
          );
        }),
      ),
```

## 打包时丢失网络权限
> 文件目录 
>> C:\Users\Administrator\Desktop\app\vidio_demo\android\app\src\main\AndroidManifest.xml

```dart
  在 </manifest> 标签前面添加 
  >>  权限授权
  <uses-permission android:name="android.permission.READ_PHONE_STATE" />
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
```
