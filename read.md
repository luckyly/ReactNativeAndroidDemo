# 集成reactnative到android项目
1.新建文件夹，创建子目录andorid,将android项目移动至android目录下。

2.在根目录下创建package.json文件

{
  "name": "MyReactNativeApp",
  "version": "0.0.1",
  "private": true,
  "scripts": {
    "start": "node node_modules/react-native/local-cli/cli.js start"
  },
  "dependencies": {
    "react": "16.0.0-alpha.6",
    "react-native": "0.44.3"
  }
}

其中，dependencies中react、react-antive版本号根据需要选择，可使用npm info react 和npm info react-native获取版本信息。

3.在根目录下使用npm install 安装react和react-naive，会在根目录下生成nodemodule目录。

4.配置app/build.gradle

dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
  ...
    compile "com.facebook.react:react-native:+"  // From node_modules.
}
5.配置项目/build.gradle

allprojects {
    repositories {
        maven {
            // All of React Native (JS, Android binaries) is installed from npm
            url "$rootDir/../node_modules/react-native/android"
        }
    }
}

6.配置androidmanifest

添加网络权限
<uses-permission android:name="android.permission.INTERNET" />

7.创建react-native 入口文件index.js

8.切换目录到android项目根目录下，执行gradlew  installDebug  ,也可在androidstudio中编译运行。注意启动npm服务器(npm start)

9.打包：先进行jsbundle文件的打包即生成jsbundle离线文件。

命令如下

react-native  bundle --platform  andorid --dev false --entry-file index.js  --bundle-output  android/app/src/main/assets/index.android.bundle  --assets-dest  android/src/main/res/

如果提示无index.js ,请将index.andorid.js复制一份命名为index.js 存放于根目录下，与node_modules同级。

如果提示无index.android.bundle 命令，则需要在项目src/main下新建assets目录，并在根目录下执行下条命令将生成index.andorid.bundle和index.andorid.bundle.meta两个文件。

经过以上准备，就可以进行android的release打包了。
