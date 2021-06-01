在根工程的`build.gradle`中添加：

```java
buildscript {
    repositories {
        maven {
            url uri('./../../repo')
        }
        dependencies {
            classpath 'com.zfei.funmodule:GradleMoudlePlugin:0.0.4'
        }
    }
}
apply plugin: 'com.zfei.funmodule'
  
funModule {
    mainAppName = "app"	//主（壳）工程的module名称
    runType = 1	//有三种类型
    buildType = 1 //releases时传递2，debug时传递1
    libName = ["modulea","moduleb"] //作为lib的module名称，注意如果没有界面那么可以不用在这里声明
}
```

**对runType的三种类型重点解释下：**

```
//1 只有壳app可以安装运行，所有 lib 的 Manifest 中的 launchActivity 会在build时去掉launch属性，build完成后会重新还原此 Manifest。
//2 运行壳app时，不过滤 lib 的 Manifest 中的 launchActivity ，所有 lib 中的 launchActivity 都会显示到桌面上（Android默认的Manifest合并模式）。
//3 各个 lib 可以独立运行，壳app不能运行。
```
