---
layout: post
title: Android Heroes Reading Notes 1
categories: android
date: 2015-11-25 19:17:32
---
《Android群英传》读书笔记 (1) 第一章 Android体系与系统架构 + 第二章 Android开发工具新接触 <!--more-->

#### **第一章 Android体系与系统架构**

1.Dalvik 和 ART
在Dalvik中应用好比是一辆可折叠的自行车，平时是折叠的，只有骑的时候，才需要组装起来用。
在ART中应用好比是一辆组装好了的自行车，装好就可以骑了。

2.一个可以查看Android源代码网站：[http://androidxref.com/](http://androidxref.com/)

目录结构：
`Makefile` （描述Android各个组件间的联系并指导它们进行自动化编译）
`bionic` (bionic C库)
`bootable` (启动引导相关代码)
`build` (系统编译规则等基础开发包配置)
`cts` （Google兼容性测试标准）
`dalvik` （Dalvik虚拟机）
`development` （应用程序开发相关）
`external` （android使用的一些开源模块）
`frameworks` （Framework框架核心）
`hardware` （厂商硬件适配层HAL）
`out` （编译完成后的代码输出目录）
`packages` （应用程序包）
`prebuilt` （x86和arm架构下预编译资源）
`sdk` （sdk及模拟器）
`system` （底层文件系统库、应用及组件）
`vendor` （厂商定制代码）

3.Android系统目录
`/system`和`/data`是开发者特别关心的两个目录

`/system`目录下主要有：`/app`，`/fonts`，`/framework`，`/lib`，`/media`，`/usr`等子目录
例如，查看系统的属性信息文件 `/system/build.prop`：
```
shell@falcon_umts:/system $ cat build.prop
# begin build properties
# autogenerated by buildinfo.sh
ro.build.id=LMY47M.M003
ro.build.display.id=LMY47M.M003
ro.build.version.incremental=8
ro.build.version.sdk=22
ro.build.version.codename=REL
ro.build.version.all_codenames=REL
ro.build.version.release=5.1
ro.build.date=Wed Aug 19 10:44:57 PDT 2015
ro.build.date.utc=1440006297
ro.build.type=user
ro.build.user=hdsplat
ro.build.host=buildlinux16
ro.build.tags=release-keys
ro.build.flavor=falcon_gpe-user
ro.product.model=XT1032
ro.product.brand=motorola
ro.product.name=falcon_gpe
ro.product.device=falcon_umts
ro.product.board=MSM8226
......
```

`/data`目录下主要有`/app`，`/data`，`/system`，`/misc`等子目录，其中`/data/data`是开发者访问最多的目录，这里包含了app的数据信息、文件信息以及数据库信息等，以包名的方式来区别不同的应用。

<br/>
#### **第二章 Android开发工具新接触**

1.adb命令的来源
[`/system/core/toolbox`](http://androidxref.com/6.0.1_r10/xref/system/core/toolbox/)和[`/frameworks/base/cmds`](http://androidxref.com/6.0.1_r10/xref/frameworks/base/cmds/)是所有adb命令和shell命令的来源，此处链接的是Android 6.0的源码路径。

2.常用的android命令
`android list avds` 列出所有创建的android模拟器
`android list targets` 列出我们所有的SDK可用版本
```
hujiawei-MBPR:hexoblog hujiawei$ android list targets
Available Android targets:
----------
id: 1 or "android-8"
     Name: Android 2.2
     Type: Platform
     API level: 8
     Revision: 3
     Skins: HVGA, QVGA, WQVGA400, WQVGA432, WVGA800 (default), WVGA854
 Tag/ABIs : default/armeabi
----------
id: 2 or "android-10"
     Name: Android 2.3.3
     Type: Platform
     API level: 10
     Revision: 2
     Skins: HVGA, QVGA, WQVGA400, WQVGA432, WVGA800 (default), WVGA854
 Tag/ABIs : default/armeabi
----------
id: 3 or "android-15"
     Name: Android 4.0.3
     Type: Platform
     API level: 15
     Revision: 5
     Skins: HVGA, QVGA, WQVGA400, WQVGA432, WSVGA, WVGA800 (default), WVGA854, WXGA720, WXGA800
 Tag/ABIs : no ABIs.
----------
```

3.常用的adb命令
`adb push <local> <remote>`，`adb pull <remote> <local>` （文件传输）
`adb install xxx`，`adb uninstall yyy` （apk安装和卸载）
`adb usb`，`adb tcpip <port>`，`adb connect`，`adb devices` （连接相关命令）
`adb start-server`，`adb kill-server`，`adb reboot`，`adb remount` (重新挂载系统分区，使系统分区重新可写)

`adb shell`相关命令：
`adb shell df` （查看系统盘符）
`adb shell input keyevent` (模拟按键输入，例如`adb shell input keyevent 3`表示按下HOME键)
`adb shell input touchscreen` (模拟触屏输入，例如`adb shell input touchscreen swipe 18 665 18 350` )

`adb shell dumpsys activity activities` （查看activity运行状态）
```
hujiawei-MBPR:hexoblog hujiawei$ adb shell dumpsys activity activities
ACTIVITY MANAGER ACTIVITIES (dumpsys activity activities)
Display #0 (activities from top to bottom):
  Stack #0:
    Task id #279
    * TaskRecord{2fbcccec #279 A=com.android.launcher U=0 sz=1}
      userId=0 effectiveUid=u0a15 mCallingUid=1000 mCallingPackage=android
      affinity=com.android.launcher
      intent={act=android.intent.action.MAIN cat=[android.intent.category.HOME] flg=0x10000000 cmp=com.android.launcher/com.android.launcher2.Launcher}
      realActivity=com.android.launcher/com.android.launcher2.Launcher
      autoRemoveRecents=false isPersistable=true numFullscreen=1 taskType=1 mTaskToReturnTo=0
      rootWasReset=false mNeverRelinquishIdentity=true mReuseTask=false
      Activities=[ActivityRecord{74b834e u0 com.android.launcher/com.android.launcher2.Launcher t279}]
      askedCompatMode=false inRecents=true isAvailable=true
      lastThumbnail=null lastThumbnailFile=/data/system/recent_images/279_task_thumbnail.png
      hasBeenVisible=true firstActiveTime=1448539994507 lastActiveTime=1448539994507 (inactive for 58s)
```

`adb pm xxx` （Package管理信息）
例如，查看所有的packages
```
hujiawei-MBPR:hexoblog hujiawei$ adb shell pm list packages -f
package:/system/app/YouTube/YouTube.apk=com.google.android.youtube
package:/system/priv-app/TelephonyProvider/TelephonyProvider.apk=com.android.providers.telephony
package:/system/app/MediaShortcuts/MediaShortcuts.apk=com.google.android.gallery3d
package:/data/app/com.support.android.designlibdemo-1/base.apk=com.support.android.designlibdemo
package:/system/priv-app/Velvet/Velvet.apk=com.google.android.googlequicksearchbox
package:/system/priv-app/CalendarProvider/CalendarProvider.apk=com.android.providers.calendar
package:/data/app/com.imooc.animatedselector-1/base.apk=com.imooc.animatedselector
package:/system/priv-app/MediaProvider/MediaProvider.apk=com.android.providers.media
package:/system/priv-app/GoogleOneTimeInitializer/GoogleOneTimeInitializer.apk=com.google.android.onetimeinitializer
package:/data/app/com.wandoujia-1/base.apk=com.wandoujia
package:/system/app/Bug2GoStub/Bug2GoStub.apk=com.motorola.bug2go
package:/data/app/com.sina.weibo.sdk.gensign-1/base.apk=com.sina.weibo.sdk.gensign
package:/data/app/com.sohu.inputmethod.sogou-1/base.apk=com.sohu.inputmethod.sogou
package:/system/priv-app/WallpaperCropper/WallpaperCropper.apk=com.android.wallpapercropper
package:/data/app/com.jredu.netease.news-1/base.apk=com.jredu.netease.news
```

`adb am xxx` (Activity管理信息)
例如，启动一个activity `adb shell am start -n packageName[+className]`
```
hujiawei-MBPR:hexoblog hujiawei$ adb shell am start com.wandoujia
Starting: Intent { act=android.intent.action.MAIN cat=[android.intent.category.LAUNCHER] pkg=com.wandoujia }
```

OK，本节结束，谢谢阅读。
