APP 打包流程及注意事项
===

### 脚本方式打包

> 作用：  使用脚本方式打包，全自动化打包过程

脚本 **app-vue-mint** 工程目录下 `app/app-build.sh`

进入 __app/__ 目录执行：
```bash
bash app-build.sh h5 android ios
```

包括： 
- 代码更新
- H5打包
- Android打包
- iOS打包
- 上传APP到托管平台 [蒲公英](https://www.pgyer.com/)

> 脚本所作的事情，详见如下节 <打包脚本包含流程>

[蒲公英](https://www.pgyer.com/) 目前账号：
- 邮件 duanjigang@chalco-steering.com 
- 密码 ：duanjigang@mail123

[苹果开发者](https://developer.apple.com) 账号：
- Apple ID: wenjianjun@chalco-steering.com
- 密码 ：Wjj123456

### 打包脚本包含流程

1. H5代码更新
    - 保持代码更新，同步 **gitlab**
    - 涉及命令 `git pull --rebase`
1. H5打包
    - 打包方式 webpack
    - 输入vue工程项目
    - 输出H5静态资源文件包
    - 涉及命令 `npm run build:release`
      - 调整样式 
      `.nav-top-40px{top:40px}` 为 `nav-top-40px{top:40px!important}`
      - 资源拷贝
      拷贝DCloud元文件 __manifest.json__ 和 __icon.png__ 到资源包;
      拷贝index.html到资源包，引导作用
1. Android壳代码更新
    - 保持代码更新，同步 **gitlab**
    - 涉及命令 `git clone` 或 `git pull --rebase`
1. 拷贝H5包资源到Android工程
    - 拷贝打包好的H5资源包到Android工程指定目录，替换旧的资源包
1. 构建Android项目，生成APK
    - 使用Gradle构建Android
    - 涉及命令 `gradlew assembleRelease`
    - **gradlew** 工具脚本位于android工程目录下，依赖JAVA和gradle
    - 构建时所需证书已在android `build.gradle` 配置好，参见 **signingConfigs** 节
    - echo APK生成路径
1. 发布APK到 ___蒲公英平台___
    - 上传打包好的 APK 到蒲公英平台
    - 涉及命令 `curl -F "file=@$iosIPK" -F "uKey=$uKey" -F "_api_key=$apiKey" $PgyerUpdateURL`
    - 蒲公英可以做短语和邮件通知
1. iOS壳代码更新
    - 同3 __android__
1. 拷贝H5包资源到iOS工程
    - 同4
1. 构建iOS项目，生成IPK
    - 使用命令行构建方式
    - 构建脚本 iOS项目目录下 **AutoPackageScript.sh**
    - 涉及命令 
    `xcodebuild archive -project ${project_name}.xcodeproj 
                   -scheme ${scheme_name} 
                   -configuration ${build_configuration} 
                   -archivePath ${export_archive_path}`
    和
    `xcodebuild  -exportArchive \
            -archivePath ${export_archive_path} \
            -exportPath ${export_ipa_path} \
            -exportOptionsPlist ${export_options_plist_path} \
            -allowProvisioningUpdates`
    - 需求证书已在Mac主机上安装好
    - Pofile文件在脚本中选择好
    - ⚠️ 上架版本建议还是使用Xcode打包
    - echo IPK生成路径
1. 发布IPK到 ___蒲公英平台___
    - 同6
