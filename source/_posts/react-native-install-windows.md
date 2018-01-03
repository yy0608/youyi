---
title: react-native_install_windows
date: 2018-01-03 09:26:16
tags:
  - react-native
  - react native windows环境
---
###### 本文主要介绍react native在windows上开发android的环境搭建，前几天第一次搭建成功过，遇到一些坑，时间过了两天，可能有一些遗漏，有问题请自行搜索或在关于页面联系我。早前是使用mac开发iOS的环境，比较顺利很多，晚点会再写一篇使用react native在mac上开发android和iOS的文章。
##### 一、安装软件（建议先翻墙，自行搜索，鉴于某些原因，不做介绍）
- 1.cmder，命令行工具，[下载](http://cmder.net/)
- 2.chocolatey，安装方法，命令行工具输入下面命令，使用管理员身份运行
```
@powershell -NoProfile -ExecutionPolicy Bypass -Command "iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin
```
- 3.listary，全局搜索工具，安装好后默认双击ctrl键（非必需）
- 4.Python 2，（我当时好像没有安装）
```
choco install python2
```
- 5.Node，对于react native不要使用cnpm
```
choco install nodejs.install

更新nodejs，也可使用nvm管理多版本node
choco upgrade nodejs.install
```
- 6.Yarn，替代npm的工具，加速node模块的下载
```
choco install yarn

也可通过npm安装，但需要配置镜像
npm config set registry https://registry.npm.taobao.org --global
npm config set disturl https://npm.taobao.org/dist --global

yarn config set registry https://registry.npm.taobao.org --global
yarn config set disturl https://npm.taobao.org/dist --global
```
- 7.react-native-cli，全局的react native工具
```
npm install -g react-native-cli
```
- 8.Java Development Kit [JDK] 1.8，不要下载更高版本！使用chocolatey安装或是单独[下载](http://www.oracle.com/technetwork/cn/java/javase/downloads/jdk8-downloads-2133151-zhs.html)
```
choco install jdk8

通过javac -version查看版本和是否成功，需要配置环境变量
```
- 9.Android Studio，需要配置环境变量
- 10.Genymotion，比起Android Studio自带的原装模拟器，Genymotion是一个性能更好的选择，但它只对个人用户免费。
##### 二、安装运行
- 1.初始化并运行
```
react-native init AwesomeProject
cd AwesomeProject
react-native run-android
```
- 2.选择真机或者模拟器，使用adb工具
##### 三、打包apk
- 1.生成签名密钥，完成后将my-release-key.keystore文件移动到android/app下
```
keytool -genkey -v -keystore my-release-key.keystore -alias my-key-alias -keyalg RSA -keysize 2048 -validity 10000
```
- 2.gradle配置，打开 android/app 下的 build.gradle 文件，添加如下代码，MYAPP_RELEASE_STORE_FILE 等变量在 gradle.properties 文件中可查看
```
android {
    ...
    defaultConfig { ... }
    signingConfigs {
        release {
            storeFile file(MYAPP_RELEASE_STORE_FILE)
            storePassword MYAPP_RELEASE_STORE_PASSWORD
            keyAlias MYAPP_RELEASE_KEY_ALIAS
            keyPassword MYAPP_RELEASE_KEY_PASSWORD
        }
    }
    ...
    buildTypes {
        release {
              ...
              signingConfig signingConfigs.release
        }
    }
}
```
- 3.打包应用。在 android/app/src/main/ 目录下创建 assets 目录
```
react-native bundle --platform android --dev false --entry-file index.android.js \ --bundle-output android/app/src/main/assets/index.android.bundle \ --assets-dest android/app/src/main/res/

进入android目录，运行命令
gradlew assembleRelease

在 android/app/build/outputs/apk/ 下，找到打包生成的 app-release.apk
```

##### 四、具体代码开发
- 1.react-navigation导航和tab插件

# &nbsp;
> *参考文章，[手把手教你建github技术博客](https://www.jianshu.com/p/701b1095da11)*