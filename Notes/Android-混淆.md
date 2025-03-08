# Android-混淆

- **混淆文件采用白名单法，意思是不在白名单里面的都要混淆。**
- **开启混淆**
  
    在 **build.gradle** 中打开
    
    ```bash
    buildTypes {
        release {
                    #minifyEnabled 混淆开关，  true：打开   false：关闭        minifyEnabled true
                    #去掉没有引用的资源，可以减少应用的体积，但这个只有在混淆开启后才有效                shrinkResources true
                    #proguard-android.txt Android自带一个混淆规则文件                #proguard-rules.pro   自定义的混淆配置文件        proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'    }}
    ```
    
- **混淆规则基本语法**
  
    ```bash
    # 代表行注释符- 表示一条规则的开始
    ```
    
    一般规则用连起来单词表示，主要有：
    
    ```bash
    keep 保留，例如keepattributes：表示保留属性
    dont 不要，例如dontwarn：表示不要提示警告
    ignore 忽略，例如ignorewarning：表示忽略警告
    ```
    
    混淆配置文件不检查规则是否重复，如果两条规则冲突，则采用白名单的，比如设置了开启优化和不优化两个选项后，无论顺序，最终都会执行不优化的操作。
    
- **优化控制**
  
    这个是用于控制混淆是否开启优化代码，例如一些if/else语句可以被简化等这些操作：
    
    ```bash
    # 不优化-dontoptimize# 代码循环优化次数，0-7，默认为5-optimizationpasses 5
    ```
    
    值得注意的是默认混淆配置文件开启了`dontoptimize`。
    
- **优化进阶**
  
    开启优化后可以设置下面的规则，**assumenosideeffects** 表示指定的代码无效，可以优化，最终效果表现为不执行。
    
    ```bash
    # 混淆时所采用的优化规则-optimizations !code/simplification/arithmetic,!field/*,!class/merging/*# 关闭log-assumenosideeffects class android.util.Log {
        public static boolean isLoggable(java.lang.String, int);    public static int v(...);    public static int i(...);    public static int w(...);    public static int d(...);    public static int e(...);}
    ```
    
- **基本混淆规则**
  
    下面这些一般混淆规则都要加入，其中前两个在默认文件中已经配置：
    
    ```bash
    # 包名不使用大小写混合 aA Aa-dontusemixedcaseclassnames# 不混淆第三方引用的库-dontskipnonpubliclibraryclasses# 不做预校验-dontpreverify# 忽略警告-ignorewarning
    ```
    
- **输出混淆记录**
  
    混淆后由于阅读困难性提高，所以为了方便自己查阅，可以输出mapping对应文件，可以利用AndroidSDK.bat打开混淆工具，利用retrace结合mapping和stacktrace调试遇到的错误
    
    ```bash
    # 混淆后生产映射文件 map 类名->转化后类名的映射# 存放在app\build\outputs\mapping\release中-verbose# 混淆前后的映射-printmapping mapping.txt
    # apk 包内所有 class 的内部结构-dump class_files.txt
    # 未混淆的类和成员-printseeds seeds.txt
    # 列出从 apk 中删除的代码-printusage unused.txt
    ```
    
- **保留源代码行号**
  
    即使使用retrace工具，还是很难定位到错误的时候，可以暂时先保留行号，观察错误修改后再关闭掉
    
    ```bash
    # 抛出异常时保留代码行号# 这个最后release的时候关闭掉-keepattributes SourceFile, LineNumberTable
    ```
    
- **基本组件白名单**
  
    Android中的基本组件不能混淆，为了方便，下面提供了兼容性比较高的规则：
    
    ```bash
    -keep public class * extends android.app.Fragment
    -keep public class * extends android.app.Activity
    -keep public class * extends android.app.Application
    -keep public class * extends android.app.Service
    -keep public class * extends android.content.BroadcastReceiver
    -keep public class * extends android.content.ContentProvider
    -keep public class * extends android.app.backup.BackupAgentHelper
    -keep public class * extends android.preference.Preference
    ```
    
- **Support包规则**
  
    ```bash
    # 如果有引用v4包可以添加下面这行-keep public class * extends android.support.v4.app.Fragment
    # 如果引用了v4或者v7包-dontwarn android.support.**
    ```
    
- **不混淆native方法**
  
    ```bash
    # 保持 native 方法不被混淆-keepclasseswithmembernames class * {
        native <methods>;        static native <methods>;}
    ```
    
- **WebView混淆规则**
  
    使用了WebView的JS功能则开启下面规则，这个规则在自定义规则文件中已经用注释说明了：
    
    ```bash
    # WebView使用javascript功能则需要开启-keepclassmembers class fqcn.of.javascript.interface.for.webview {
        public *;}
    ```
    
- **注解、泛型和反射混淆**
  
    ```bash
    # 保护注解-keepattributes *Annotation*-keep class * extends java.lang.annotation.Annotation { *; }# 泛型-keepattributes Signature
    # 反射-keepattributes EnclosingMethod
    ```
    
- **内部类混淆**
  
    ```bash
    # 不混淆内部类-keepattributes InnerClasses
    # 如果说你想指定不混淆某个内部类-keep class com.test.ts.WrapperClass$InnerClass { *; }
    ```
    
- **第三方混淆参考规则**
    - **Gson**
      
        ```bash
        # gson-dontwarn com.google.**-keep class com.google.gson.** { *; }# 当使用gson时，同事也需要不混淆javaBean文件-keep class com.test.ts.Bean { *; }# 如果你的所有javaBean文件放在同一个目录（如：javabean 目录下）下，则可以使用-keep class com.test.ts.javabean.** { *; }
        ```
        

- **保持该包名下所有类以及该包下所有子包下的所有类不被混淆**：`-keep class cn.hadcn.test.`**只保持该路径下的所有类不被混淆，但该包下的子包里的类还是会被混淆**：`-keep class cn.hadcn.test.` **保持所有extends Actiivty 的类不被混淆**：`-keep public class * extends android.app.Activity`
