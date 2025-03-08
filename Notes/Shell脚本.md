# Shell脚本

- ## sed: 1: "...": invalid command code 错误处理

  ```shell
  #下面命令在mac上运行会报 sed: 1: "...": invalid command code 的错误
  sed -i "s/$oldtag/$tag/g" `grep -rl "$oldtag" ./`
  
  #这个错误是因为 Mac 的 sed 命令和 linux 的 sed 命令使用方式不同，Mac 上默认修改时是有备份机制的。
  
  #所以，在 Mac 下使用 sed 命令可选择不使用备份：
  sed -i "" "s/$oldtag/$tag/g" `grep -rl "$oldtag" ./`
  ```

- ## copy文件夹脚本

  ```shell
  #!/bin/bash
  
  # 需要复制的源文件夹
  SOURCE_DIR_PATH="/Users/mac/WorkSpace/Projects/ProjectName"
  # 复制到的目录的文件夹
  TARGET_DIR_PATH="/Users/mac/WorkSpace/Projects/ProjectNameCache"
   
  
  copyDir() {
  	if [ ! -d $1 ] ; then
  	    echo "the source dir not exist [$1]"	
  		exit 0
  	fi
  	checkDirExist $2
  	for file in `ls $1`
  	do
  		if [ -d "$1/$file" ] ; then
  			if [ $file = "build" ] ; then
  				# echo "$file is build cache, ready to delete"
  				rm -rf $1"/"$file
  			else
  			    # echo "folder: `pwd`/$file"
  			    cd $1"/"$file
  			    copyDir $1"/"$file $2"/"$file
  			fi
  		# 是文件
  		else
  			copyFile $1"/"$file $2"/"$file
  		fi
  	done
  }
  
  copyFile() {
  	# echo "copyFile from:$1 , to:$2"
  	cp -R $1 $2
  }
  
  checkDirExist() {
  	if [ ! -d $1 ] ; then
  		# echo "The path not exist, create it now"
  		mkdir $1
  		return 1
  	else
  		return 0
  	fi
  }
  
  # 如果是文件夹
  echo "ready to copy dir:$SOURCE_DIR_PATH"
  if [ -d $SOURCE_DIR_PATH ] ; then
  	echo "copying...please wait..."
  	# copyDir $SOURCE_DIR_PATH $TARGET_DIR_PATH
  	echo "copy all file in dir done 
  	from:[$SOURCE_DIR_PATH]
  	to :[$TARGET_DIR_PATH]"
  else
  	echo "The source dir not exist [$SOURCE_DIR_PATH]"
  fi
  
  exit 0
  ```

- ## Android自动打包脚本

  ```shell
  #!/bin/bash
  
  WORK_DIR_PATH="/Users/mac/WorkSpace/Projects/ProjectName"
  # 需要复制的源文件夹
  SOURCE_DIR_PATH=$WORK_DIR_PATH
  # 复制到的目录的文件夹
  TARGET_DIR_PATH="$WORK_DIR_PATH-build"
  # 项目文件夹名
  PROJECT_NAME_INNER="HPrj"
  # 正式版的apk文件输出路径
  APK_OUTPUT_RELEASE_PATH="./HPrj/build/outputs/apk/prod/release"
  # 正式版的apk文件输出文件
  APK_OUTPUT_RELEASE_FILE="$APK_OUTPUT_RELEASE_PATH/*.apk"
  
  # JKS签名相关
  # 签名文件存放目录
  SIGN_JKS_DIR="HPrj/config"
  # 签名文件
  SIGN_JKS_FILE_NAME="weppv1.jks"
  # 当将jks进行一次pck12后，会产生一个这样的文件，需要删除
  SIGN_JKS_NEED_DELETE_FILE_NAME="weppv1.jks.old"
  RELEASE_KEY_ALIAS="wepjad"
  RELEASE_STORE_PASSWORD="dj84k*f8&j4"
  RELEASE_KEY_PASSWORD="dj84k*f8&j4"
  
  # 包名前缀
  PKGNAME_PREFIX="HPrj/src/main/java/im"
  # 是否使用固定的包名
  PKGNAME_USE_FIXED=0
  # 固定的包名
  PKGNAME_FIXED_NAME="usaofmt"
  
  set -e
  
  # 先复制一份源文件进行修改包名后build
  copyDir() {
   if [ ! -d $1 ] ; then
       echo "the source dir not exist [$1]" 
    exit 0
   fi
   checkDirExist $2
   for file in ls $1
   do
    if [ -d "$1/$file" ] ; then
     if [ $file = "build" ] ; then
      # echo "$file is build cache, ready to delete"
      rm -rf $1"/"$file
     else
         # echo "folder: pwd/$file"
         cd $1"/"$file
         copyDir $1"/"$file $2"/"$file
     fi
    # 是文件
    else
     copyFile $1"/"$file $2"/"$file
    fi
   done
  }
  
  copyFile() {
   # echo "copyFile from:$1 , to:$2"
   cp -R $1 $2
  }
  
  checkDirExist() {
   if [ ! -d $1 ] ; then
    # echo "The path:$1 not exist, create it now"
    mkdir $1
   fi
  }
  
  # 如果是文件夹
  echo "ready to copy dir:$SOURCE_DIR_PATH"
  if [ -d $SOURCE_DIR_PATH ] ; then
   echo "copying...please wait..."
   copyDir $SOURCE_DIR_PATH $TARGET_DIR_PATH
   echo "copy all file in dir done 
   from:[$SOURCE_DIR_PATH]
   to :[$TARGET_DIR_PATH]"
  else
   echo "The source dir not exist [$SOURCE_DIR_PATH]"
   exit 0
  fi
  
  # 重新生成签名文件
  echo "recreate sign jks file"
  #进入原来的签名文件存放目录
  cd $TARGET_DIR_PATH/$SIGN_JKS_DIR
  
  #删除原来的签名文件
  rm -rf $SIGN_JKS_FILE_NAME
  
  #生成新的签名jks文件
  keytool -genkeypair -keyalg RSA -dname "CN=localhost" -alias $RELEASE_KEY_ALIAS -keystore $SIGN_JKS_FILE_NAME -keypass $RELEASE_KEY_PASSWORD -storepass $RELEASE_STORE_PASSWORD
  keytool -importkeystore -srckeystore $SIGN_JKS_FILE_NAME -destkeystore $SIGN_JKS_FILE_NAME -deststoretype pkcs12 -srcstorepass $RELEASE_KEY_PASSWORD
  
  #删除生成的jks.old文件
  rm -rf $SIGN_JKS_NEED_DELETE_FILE_NAME
  echo "recreate sign jks file done"
  
  echo "rename package name"
  # 到工作目录
  cd $TARGET_DIR_PATH
  # 修改包名
  # 是否使用固定包名
  if [ $PKGNAME_USE_FIXED = 1 ] ; then
      tag=$PKGNAME_FIXED_NAME
  else
      # 生成随机包名 10位小写
      export LC_CTYPE=C
      tr -dc a-z < /dev/urandom | head -c10 > newtag.txt
      tag=cat newtag.txt
  fi
  echo "new package name:$tag"
  oldtag=wealthpp
  
  # 修改目录名称
  mv $PKGNAME_PREFIX/$oldtag $PKGNAME_PREFIX/$tag
  echo "rename dir's package name done"
  
  # 修改jks文件名称
  # mv $RE_FILE_NAME/$SIGN_JKS_FILE_NAME $RE_FILE_NAME/${tag}_release.jks  
  
  # 替换所有文件中包名
  cd $PROJECT_NAME_INNER
  export LC_CTYPE=C 
  sed -i "" "s/$oldtag/$tag/g" grep -rl "$oldtag" ./
  echo "rename all files' content package name done"
  
  # 拉取新依赖，或者第一次，先手动下面命令再执行脚本
  gradle assembleRelease
  # 打包 --offline离线打包：再不需要其他依赖的情况下使用，第一次不适用
  # gradle  --offline --no-daemon assembleRelease
  
  # cp -f $APK_OUTPUT_RELEASE_FILE $TARGET_DIR_PATH
  
  open $APK_OUTPUT_RELEASE_PATH
  
  exit 0
  ```