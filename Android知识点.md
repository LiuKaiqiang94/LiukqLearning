## Android开发中遇到的陌生的知识点记录

###Matrix类
在绘制气泡自定义控件时，需要绘制气泡边缘的路径（主要是气泡箭头），参考网上代码时他们用到Matrix类，感觉比较陌生，就记录下，主要看的[深入理解 Android 中的 Matrix](https://www.jianshu.com/p/6aa6080373ab)这篇博客。需要理解的关键词：齐次坐标，仿射变换。可以实现复杂的动画效果。

![][2]![][1]

### 加密解密
最近几天在调试接口安全的接口，对之前接触不多的加密解密记录下
- RSA加密，非对称加密，秘钥有公钥私钥之分，公钥负责对明文进行加密，与之对应的私钥用来解密密文，公钥是可以明文传输的，因为获取了公钥对解密没有用处，而私钥是要确保安全的。RSA加密之后的密文一般来说是一堆乱码，而网络传输中请求头对内容有限制，所以需要对密文再进行一次Base64加密。
- Base64加密，按照固定规则加密解密，可以用来对中文等字符转码，得出可以正常传输的数据(Url加密也有这个作用)，android.jar下的Base64类有几种加密模式，如DEFAULT,NO_WARP，Stack Overflow上说默认模式下加密结果含有0x0a字符，导致添加header时报错，使用NO_WARP可以避免，但是服务端jar包中的工具类没有这个模式，还是个问题。
- AES加密，对称加密，只有一个秘钥，可以进行加密解密操作。速度比RSA快很多
- 


[1]: https://upload-images.jianshu.io/upload_images/912181-6edf7955de7d48fb.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/405
[2]: https://upload-images.jianshu.io/upload_images/912181-a3379095b4bc6095.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/283
<meta http-equiv="refresh" content="1">