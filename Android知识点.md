## Android开发中遇到的陌生的知识点记录

###Matrix类
在绘制气泡自定义控件时，需要绘制气泡边缘的路径（主要是气泡箭头），参考网上代码时他们用到Matrix类，感觉比较陌生，就记录下，主要看的[深入理解 Android 中的 Matrix](https://www.jianshu.com/p/6aa6080373ab)这篇博客。需要理解的关键词：齐次坐标，仿射变换。可以实现复杂的动画效果。

![][2]![][1]

### 加密解密
最近几天在调试接口安全的接口，对之前接触不多的加密解密记录下
- RSA加密，非对称加密，秘钥有公钥私钥之分，公钥负责对明文进行加密，与之对应的私钥用来解密密文，公钥是可以明文传输的，因为获取了公钥对解密没有用处，而私钥是要确保安全的。RSA加密之后的密文一般来说是一堆乱码，而网络传输中请求头对内容有限制，所以需要对密文再进行一次Base64加密。
- Base64加密，按照固定规则加密解密，可以用来对中文等字符转码，得出可以正常传输的数据(Url加密也有这个作用)，android.jar下的Base64类有几种加密模式，如DEFAULT,NO_WARP，Stack Overflow上说默认模式下加密结果含有0x0a字符，导致添加header时报错，使用NO_WARP可以避免，但是服务端jar包中的工具类没有这个模式，还是个问题。
- AES加密，对称加密，只有一个秘钥，可以进行加密解密操作。速度比RSA快很多
- MD5摘要，不可逆 

### RxPermissions
 对比之前项目中使用的PermissionDispatcher，PermissionDispatcher使用注解方式apt生成申请权限代码，每次使用先在activity或fragment中证明权限注解，生成回调之后编写相关代码。RxPermissions直接在需要权限的位置调用，相比来说，非常方便。
 ```
   //申请一组权限，Consymer泛型为Boolean时，直接返回是否授权成功
    new RxPermissions(this)
        .request(Manifest.permission.WRITE_EXTERNAL_STORAGE
                , Manifest.permission.CAMERA
                , Manifest.permission.ACCESS_FINE_LOCATION)
        .subscribe(new Consumer<Boolean>() {
             @Override
              public void accept(Boolean aBoolean) {
                   Toast.makeText(MainActivity.this, "申请权限：" + aBoolean, Toast.LENGTH_SHORT).show();
              }
         });
    //另外一种使用方式
    new RxPermissions(this)
        .requestEach(Manifest.permission.WRITE_EXTERNAL_STORAGE
                , Manifest.permission.CAMERA
                , Manifest.permission.ACCESS_FINE_LOCATION)
        .subscribe(new Consumer<Permission>() {
            @Override
            public void accept(Permission permission) {
                if (permission.granted) {
                    //通过
                } else if (permission.shouldShowRequestPermissionRationale) {
                    //拒绝权限，下次询问
                } else {
                    //拒绝不再询问
                }
            }
        });
 ```
### 相机相册选择
从github上选择了几个开源库，包括RxGalleryFinal和TakePhoto。
- RxGalleryFinal，这个库是将之前的GalleryFinal改进成了响应式框架，迁移到RxGalleryFinal上的。因为之前项目中使用过GalleryFinal，由于存在各种问题（主要是7.0适配问题），无法解决，就改用自己实现的工具类。尝试了下新的Rx库，在配置上出了些问题，而它的7.0崩溃问题还在Q&A中，最终还是没有采用。
- TakePhoto，这个库是github上star最多的，主要的配置参数都可以自定义，但是在8.0以上系统使用时出现了UndeclaredThrowableException异常，没有找到有效解决方案，最终没有采用。

### greendao
GreenDao在查询时不仅可以实现单表查询，还可以实现多表查询，需要使用`@ToOne`和`@ToMany`注解声明关联关系。
```
    @Entity //运动表
    public class SportInfo {
        @Id
        private Long sportId;
        //日期
        private String date = "";
        private Long UserId;
        @ToOne(joinProperty = "UserId")
        private UserInfo userInfo;//关系表
    }
```

```
@Entity //用户表
public class UserInfo {
    @Id
    private Long id;
    //账号
    private String number;
    //密码
    private String password;
    //昵称
    private String nick_name;
    //一对多关联
    @ToMany(referencedJoinProperty = "sportId")
    private List<SportInfo> sportInfo;
}
```
[1]: https://upload-images.jianshu.io/upload_images/912181-6edf7955de7d48fb.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/405
[2]: https://upload-images.jianshu.io/upload_images/912181-a3379095b4bc6095.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/283
<meta http-equiv="refresh" content="1">