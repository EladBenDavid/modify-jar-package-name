## 使用jarjar.jar修改jar包的包名

### 前言

> 最近在Android项目中使用到了RSA加密🔐，用到了`commons-codec`包，在进行单元测试时没有问题，但是运行在真机上时却遇到了错误❌。

错误如下

```bash
java.lang.NoSuchMethodError: No static method encodeBase64String([B)Ljava/lang/String
```

提示找不到该方法，于是查了下资料，发现了原因，原来在`Android`的`Framework层`也引用了该库，运行时执行的是`Framework层`中引用的库，报错应该是`Framework层`引用的该库没有对应的该方法导致的。

### 解决方法

通过修改包名来解决，这里使用到的工具为`jarjar.jar`

下载地址为: [https://code.google.com/p/jarjar/](https://code.google.com/p/jarjar/)

然后下载要修改包名的jar包，这里使用`commons-codec`包

下载地址为: [http://commons.apache.org/proper/commons-codec/download_codec.cgi](http://commons.apache.org/proper/commons-codec/download_codec.cgi?)

修改名称为`c1.jar`，运行以下命令即可

```bash
java -jar jarjar.jar process rule.text source/c1.jar result/my.jar
```

这里的`rule.text`定义了修改规则，如下

```ba sh
rule org.apache.commons.codec.** org.apache.mybase64.@1
```

这句代表了将原包名`org.apache.commons.codec`修改为`org.apache.mybase64`

具体的规则定义可以参考[这篇](https://www.cnblogs.com/yejiurui/p/4283505.html)文章

最后在`result`文件夹可看到修改包名后的jar包

