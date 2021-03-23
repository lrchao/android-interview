### APK组成

- AndroidManifest.xml 描述配置文件
- classes.dex 是java源码编译后生成的java字节码文件，因方法数限制，可拆分为多个dex.优化重排后生成dex文件，生成的dex文件可以在Dalvik虚拟机执行，且速度比较快
- META-INF 存放签名和证书的目录
- res 主要是存放图片资源
- lib 主要是存放so库，各个cpu架构
- assets 主要存放不需要编译处理的文件
- resources.arsc 是编译后的二进制资源文件，记录了资源id和资源的对应关系。


