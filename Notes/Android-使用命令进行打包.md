# Android-使用命令进行打包

- ## 打.apk的包

  - mac上使用 gradlew 时提示 **permission denied** 时，在终端输入

      ```bash
      chmod +x gradlew
      ```

  - 使用命令进行打包

      ```bash
      ./gradlew assembleRelease
      ```

  - 使用命令进行离线打包

      ```bash
      ./gradlew --offline --no-daemon assembleelease
      ```
  
- ## 打.aab的包（用于Google Play上架）

  ```bash
  ./gradlew bundleRelease
  ```

- ## 自动打包脚本

  参考：[https://www.notion.so/Shell-de70c750c6c9448eafb0581947d72646#62b07c426b5d4ea1b638d3985ba21ceb](https://www.notion.so/de70c750c6c9448eafb0581947d72646?pvs=21)