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