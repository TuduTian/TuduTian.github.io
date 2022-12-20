
## nest.js基础
> 参考教程：https://blog.csdn.net/qq1195566313/category_11844396.html
> nest官网：https://nestjs.com/

## nest上传图片
1. 安装插件
```js
  yarn add multer   @types/multer
```

2. 在modules中配置存放的地方
```js
  import {MulterModule} from '@nestjs/platform-express' // 从 nestjs/platform-express 拿到
import { diskStorage } from 'multer' // 这张图片存放的位置设置
// 注册
@Module({
  imports:[
    MulterModule.register({
      storage:diskStorage({
        destination:join(__dirname,'./images') // 存放的文件夹，会自动生成的
        filename:(_，file,cb)=>{
          // _ 我们一般不管它，file就是用户发送的文件，cb是回调函数
           const fileName = `${
            new Date().getTime() + extname(file.originalname)
          }`; //拿到文件名 根据当前时间戳随机生成的

             // callback 出去 这个名字
          return cb(null, fileName); // cb的第一个参数是err参数 失败的时候进行填写的
        }//存放的名字 接受一个函数
      })
    })
  ]
})
```
3. 在 controller 中进行配置
```js
  @Post('/upload')
  @UseInterceptors(FileInterceptor('file')) // 通过 使用拦截器装饰器进行 劫持到file
  uploadImage(@UploadedFile() file) { // 这里拿到的file是通过属性装饰器获取用户传递过来的file
    return true; // 返回成功
  }
```

## nest中的中间介
### 全局中间介 常用案例，解决跨域
1. 依赖安装
```js
    yarn add cors
  yarn add @types/cors -D //声明文件
```
2. 配置全局中简介
```js
    //声明中简介函数
  function middleawreAll(req: Request, res: Response, next: NextFunction) {
    console.log('我是全局的中简介');
    next();
  }
  //使用中简介函数  / main.ts
  import * as cors from "cors" //引入的时候这么引入
  async function bootstrap() {
    const app = await NestFactory.create(AppModule);
    app.use(cors()),// 处理跨域
    app.use(middleawreAll); // 和express一摸一样 注册中间介 先去全局的，然后才回去局部的中简介
    app.setGlobalPrefix('api');
    await app.listen(3000);
  }
```

## nest下载图片（流）
1. 依赖安装
```js
  yarn add  compressing
```
2. 引入依赖并且实例化
```ts
import { zip } from 'compressing'; // 将文件压缩成zip格式的


// 例子 service.ts
@Injectable()
export class ShopService {
  async exportShopDataS(res) {
    const tarStream = new zip.Stream(); //实例流
    const file = join(__dirname, '../images/sample.png'); //拿到file的路径
    await tarStream.addEntry(file); // 根据入口去进行流转换 异步的
    tarStream.pipe(res); // pipe出去 是一个zip格式的压缩文件
  }
}
```
