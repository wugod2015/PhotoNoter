# PhotoNoter

Material Design风格的开源照片笔记。

 [![GitHub release](https://img.shields.io/github/release/yydcdut/PhotoNoter.svg)](https://github.com/yydcdut/PhotoNoter/releases)   [![License](https://img.shields.io/badge/license-Apache%202-4EB1BA.svg)](https://www.apache.org/licenses/LICENSE-2.0.html)   [![Build Status](https://travis-ci.org/yydcdut/PhotoNoter.svg?branch=master)](https://travis-ci.org/yydcdut/PhotoNoter)

下载：

- <a href="http://www.wandoujia.com/apps/com.yydcdut.note">豌豆荚</a>
- <a href="http://android.myapp.com/myapp/detail.htm?apkName=com.yydcdut.note">应用宝</a>
- <a href="http://app.flyme.cn/apps/public/detail?package_name=com.yydcdut.note">Flyme</a>
- <a href="http://shouji.baidu.com/software/item?docid=8071412&from=as">百度</a>
- <a href="http://apk.91.com/Soft/Android/com.yydcdut.note-6.html">91</a>
- <a href="http://apk.hiapk.com/appinfo/com.yydcdut.note/6">安卓市场</a>
- <a href="http://zhushou.360.cn/detail/index/soft_id/3135353?recrefer=SE_D_%E7%85%A7%E7%89%87%E7%AC%94%E8%AE%B0">360</a>

如果发现bug或者有什么建议，**欢迎在issue里面讨论**！

# 编译

如果编译不过，错误日志是：

> Error:A problem was found with the configuration of task ':app:packagexxxxDebug'.

>

> File ‘/xxxxxxxxxx/debug.keystore' specified for property 'signingConfig.storeFile' does not exist.

将{$projectName}/app/build.gradle中的下面代码删除

``` groovy
debug{
            storeFile file("debug.keystore")
     }
```

# 应用截图

## 动画gif

<img width="300" height="553" src="https://raw.githubusercontent.com/yydcdut/PhotoNoter/master/screenshot/animation.gif">

## 界面

<img src="https://raw.githubusercontent.com/yydcdut/PhotoNoter/master/screenshot/screen1.png" width="25%" height="25%" style="max-width:100%;"><img src="https://raw.githubusercontent.com/yydcdut/PhotoNoter/master/screenshot/screen2.png" width="25%" height="25%" style="max-width:100%;"><img src="https://raw.githubusercontent.com/yydcdut/PhotoNoter/master/screenshot/screen3.png" width="25%" height="25%" style="max-width:100%;"><img src="https://raw.githubusercontent.com/yydcdut/PhotoNoter/master/screenshot/screen4.png" width="25%" height="25%" style="max-width:100%;">

<img src="https://raw.githubusercontent.com/yydcdut/PhotoNoter/master/screenshot/screen5.png" width="25%" height="25%" style="max-width:100%;"><img src="https://raw.githubusercontent.com/yydcdut/PhotoNoter/master/screenshot/screen6.png" width="25%" height="25%" style="max-width:100%;"><img src="https://raw.githubusercontent.com/yydcdut/PhotoNoter/master/screenshot/screen7.png" width="25%" height="25%" style="max-width:100%;"><img src="https://raw.githubusercontent.com/yydcdut/PhotoNoter/master/screenshot/screen8.png" width="25%" height="25%" style="max-width:100%;">

# 技术点

1. 整体项目MVP结构(1.2.0之前是MVC)。
2. Dagger2。
3. 相机MVC架构(还没有重构为MVP)，API>=21使用Camera2，API<21使用Camera。
4. 相机的状态机（状态机不对很容易崩哦~还要参数部分）。
5. 照片缓存分为两种，一个是大图，一个是小图，小图是相册界面缩略图的时候加载的，大图是查看图片的时候加载的。
6. 图片处理，这是一个老生常谈的了。但是在App中，发现很多这方面的问题我还没有解决。比如红米1s后置摄像头800W，那么拍一张图是3M左右，但是Camera的照片的0度是我们正常手机视角的90度。那么我们需要把这个3M的图片给翻转过来，又不想失分辨率，诶，臣妾做不到啊！那么现在的解决办法是不去拍摄800W像素的，拍大概400-500W像素的不会OOM的。
7. 沙盒。每次拍完照都是先把数据放到沙盒数据库中，然后再到服务中去作图，做完的话再从数据库中删除掉。作图的Service是和Camera那个Activity绑定的(bind方式)，当不再拍照的时候就退出了Service，然后回到相册界面的时候会去判断沙盒数据库中是否有没有做完的图，没有做完的话另外启一个进程的Service继续作图。
8. Activity退出和进入的动画。这块弄了很久，主要是想模仿Android5.0的那种，但是有些界面做出来超级卡。
9. 一些UI的动画，比如 “ 意见反馈”、 “ 语音输入” 这里面的动画。
10. 主题设置，沉浸式状态栏（5.0）。这部分为了适配国内的ROM，我写的很奇怪很恶心，但是毕竟还是达到了效果的。
11. 切换主题。
12. 可以滑动item和可以拖放item的ListView（<a href="https://github.com/yydcdut/SlideAndDragListView">SlideAndDragListView</a>）。 
13. RxJava + RxAndroid（RxCategory/RxPhotoNote/RxSandBox/RxFeedBack/RxUser）。
14. dex分包处理，虽然还还没有达到65536个方法，但是我还是进行了分包处理，我为什么这样做呢？我想把最先用到的几个类和依赖类放到主dex里面，让主dex的大小小一些，这样在第一次启动的时候速度快一些，同时异步去加载第二个dex！异步！异步！异步！重要的事情要说三遍。目前自己去操作dex优化的结果是比系统配置第一个dex的包要小0.1M.....

# 更新版本说明

<a href="https://github.com/yydcdut/PhotoNoter/blob/master/CHANGELOG.md">ChangeLog</a>

# 致谢

- <a href="https://github.com/markushi/android-ui">android-ui</a>
- <a href="https://github.com/futuresimple/android-floating-action-button">android-floating-action-button</a>
- <a href="https://github.com/yydcdut/SlideAndDragListView">SlideAndDragListView</a>
- <a href="https://github.com/lsjwzh/MaterialLoadingProgressBar">MaterialLoadingProgressBar</a>
- <a href="http://sdk.camera360.com/">Camera360 SDK</a>

# License

Copyright 2015 yydcdut

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at

[http://www.apache.org/licenses/LICENSE-2.0](http://www.apache.org/licenses/LICENSE-2.0)

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.