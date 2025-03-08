# ADB-常用命令

[查看栈信息](##查看栈信息)
[查看当前运行app的Activity的名称](##查看当前运行app的Activity的名称)
[使用adb给目标device安装apk文件](##使用adb给目标device安装apk文件)
[显示系统中全部设备](##显示系统中全部设备)
[开启ADB服务](##开启ADB服务)
[关闭ADB服务](##关闭ADB服务)
[连接设备](##连接设备)
[断开设备](##断开设备)
[卸载apk文件](##卸载apk文件)
[卸载 app 但保留数据和缓存文件](##卸载 app 但保留数据和缓存文件)
[列出手机装的所有app的包名](##列出手机装的所有app的包名)
[列出系统应用的所有包名](##列出系统应用的所有包名)
[列出除了系统应用的第三方应用包名](##列出除了系统应用的第三方应用包名)
[清除应用数据与缓存](##清除应用数据与缓存)
[启动应用](##启动应用)
[强制停止应用](##强制停止应用)
[删除系统应用](##删除系统应用)
[杀死某个进程](##杀死某个进程)
[获取文件的读写权限](##获取文件的读写权限)
[pull和push文件](##pull和push文件)
[查看日志](##查看日志)
[查看屏幕分辨率](##查看屏幕分辨率)



- ## 查看栈信息

  ```bash
  adb shell dumpsys activity activities
  ```

- ## 查看当前运行app的Activity的名称

  ```bash
  **adb shell "dumpsys window | grep mCurrentFocus"
  或者
  adb shell dumpsys activity | grep -i run**
  ```

- ## 使用adb给目标device安装apk文件

  ```bash
  adb install /Users/macos/WorkSpace/Android/反编译/Need/kakaotalk.apk
  ```

- ## 显示系统中全部设备

  ```bash
  adb devices
  ```

- ## 开启ADB服务

  ```bash
  adb start-server
  ```

- ## 关闭ADB服务

  ```bash
  adb kill-server
  ```

- ## 连接设备

  ```bash
  adb connect 192.168.1.61
  ```

- ## 断开设备

  ```bash
  adb disconnect 192.168.1.61
  ```

- ## 卸载apk文件

  ```bash
  adb uninstall （apk包名）
  ```

- ## 卸载 app 但保留数据和缓存文件

  ```bash
  adb uninstall -k （apk包名）
  ```

- ## 列出手机装的所有app的包名

  ```bash
  adb shell pm list packages
  ```

- ## 列出系统应用的所有包名

  ```bash
  adb shell pm list packages -s
  ```

- ## 列出除了系统应用的第三方应用包名

  ```bash
  adb shell pm list packages -3
  ```

- ## 清除应用数据与缓存

  ```bash
  adb shell pm clear （apk包名）
  ```

- ## 启动应用

  ```bash
  adb shell am start -n com.helloshan.demo/.MianActivity
  ```

- ## 强制停止应用

  ```bash
  adb shell am force-stop （apk包名）
  ```

- ## 删除系统应用

  ```bash
  adb remount //（重新挂载系统分区，使系统分区重新可写）。
  adb shell
  cd system/app/
  ls
  rm *.apk
  ```

- ## 杀死某个进程

  ```bash
  adb shell
  ps //ps是查看进程命令，kill pid 你想结束的进程
  kill pid
  ```

- ## 获取文件的读写权限

  ```bash
  //有些设备并不能直接adb remount，必须要先以root身份进入，先执行adb root，在执行adb remount
  adb remount
  ```

- ## pull和push文件

  ```bash
  adb push (文件路径) (想要push的路径)
  adb pull (文件路径) (想要pull的路径)
  ```

- ## 查看日志

  ```bash
  adb logcat
  ```

- ## 查看屏幕分辨率
  ```bash
  adb shell wm size
  ```
