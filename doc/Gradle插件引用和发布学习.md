# Gradle插件有两种类型：

- 二进制插件（binary plugins）
  实现方式：二进制插件可以通过实现插件接口以编程方式编写。
  应用方式，有四种：

    1. plugins{ id 'pluginId'} 方式（plugins{}代码块必须位于build.gradle文件最开始）

       这种方式需要将插件发布到了Gradle Plugin Portal（Gradle官方插件网站）或者本地插件仓库。
       这种方式，必须要生成二进制插件文件（pluginId.gradle.plugin），具体怎么生成参见 build.gradle中的 生成二进制插件代码块。

    2. buildscript block 方式（这种方式本质是"导包"，通过添加构建代码依赖来将插件添加进来，依赖进来后也可以直接在build.gradle中使用导入的依赖中的方法）

       这种方式不需要生成 pluginId.gradle.plugin 文件，它通过 src/main/resources/META-INF/gradle-plugins 形式来声明插件ID（io.github.stateofzhao.properties，其中 io.github.stateofzhao 就是pluginId）。

       引用方式如下：

       ```java
       buildscript{
         repositories{
             ...
         }
         dependencies{
             //例如 ： classpath `com.android.tools.build:gradle:3.2.1`
             classpath `group:name:version`
         }
        }
        apply plugin: `pluginId`
       ```

    3. 直接在项目中建立buildSrc来进行gradle插件开发。

    4. Defining the plugin as an inline class declaration inside a build script.

  第一种和第二种比较常用。

- 脚本插件（script plugins）
  实现方式：直接在build.gradle中添加监听回调来实现。
  应用方式，直接在build.gradle中开发即可，无需额外引用。

# Gradle插件发布到远程/本地仓库：

有两种方式：

1. 发布到Gradle Plugin Portal。这种方式仅仅将 xxx.gradle.plugin 文件发布出去，引用时直接 plugins{id `xxx`} 方式引用即可。
2. 发布插件代码到 Maven/ivy 等仓库。这种方式必须采用 buildscript block 方式来引用。

# Tip：

```java
//去Gradle官方插件库寻找插件 or 本地查找
plugins{
    id 'pluginId'
}
```

```java
//只会在buildscript{dependencies{}}依赖中查找插件
apply plugin: pluginId
```

