# Android-反编译操作步骤

- ### 1. 将.apk文件改成.zip后缀，进行解压缩。

- ### 2. 使用dex2ar工具将dex文件反编译成jar文件。

- macOS如果出现 **permission denied: d2j-dex2jar.bat**，在终端输入：
  
    ```bash
    sudo chmod +x d2j_invoke.sh
    //然后键入命令：sh [d2j-dex2jar.sh](http://d2j-dex2jar.sh/) classes.dex进行反编译操作。
    ```
    
- ### 3. 使用apktool对apk文件进行反编译（apktool macOS 用命令brew install apktool进行安装）：

  - 需要注意的是，在哪个目录下使用的apktool命令，它就会将反编译的文件编译到该文件夹中作为子目录。

      ```bash
      apktool d /Users/macos/WorkSpace/Android/反编译/Need/kakaotalk.apk
      ```
  
- ### 4. 对AndroidManifest.xml中的增加`android:debuggable="true"`属性，然 后保存。

- ### 5. 使用./reapk.sh kakaotalk进行回编译：

  ```bash
   ./reapk.sh kakaotalk
  ```

- ### 6. 生成新的签名文件：

  ```bash
  keytool -genkey -alias kaotalk.keystore -keyalg RSA -validity 40000 -keystore /Users/macos/WorkSpace/Android/反编译/签名文件/kaotalk/kaotalk.keystore
  ```

- ### 7. 使用keytool和生成的新的签名文件对apk重新进行打包：

  ```bash
  jarsigner -verbose -keystore /Users/macos/WorkSpace/Android/反编译/签名文件/kaotalk/kaotalk.keystore -signedjar _signed.apk Decompile.apk /Users/macos/WorkSpace/Android/反编译/签名文件/kaotalk/kaotalk.keystore
  ```

## 第二种方式

1. 将apk的后缀名换成zip，然后进行解压，得到dex文件。
2. 通多dex2jar工具将dex文件转换成jar文件。
    1. 将第1步得到classes.dex文件复制到dex2jar工具的文件夹中
    2. 打开cmd，cd到dex2jar工具下的目录中
    3. 执行以下命令得到jar文件
       
        ```bash
        d2j-dex2jar.bat classes.dex
        //mac下使用[d2j-dex2jar.sh](http://d2j-dex2jar.sh/) classes.dex
        ```
    
3. 使用jd-gui工具查看jar下的混淆过后的源码。