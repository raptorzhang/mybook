
**adb是什么\?**

adb的全称为Android Debug Bridge。通过adb我们可以在Eclipse中方面通过DDMS来调试Android程序，说白了就是debug工具，默认端口为5037。

**adb有什么用\?**

借助adb工具，我们可以管理设备或手机模拟器的状态。还可以进行很多手机操作，如安装软件、系统升级、运行shell命令等等。其实简而言说，adb就是连接Android手机与PC端的桥梁，可以让用户在电脑上对手机进行全面的操作\!

**环境搭建**

本文以windows为例：

adb 的安装可以有多种方式：

- 直接下载adb相关文件到本地。
- 通**SDK 平台工具**\([https://developer.android.com/studio/releases/platform-tools\?hl=zh\_cn](https://developer.android.com/studio/releases/platform-tools\?hl=zh\_cn))下载 SDK Platform Tools。
- 下载Android 的SDk\(文件较大，包含上述两个文件\)。

这里以下载 SDK Platform Tools 为例：

- 访问SDK平台工具\([https://developer.android.com/studio/releases/platform-tools\?hl=zh\_cn](https://developer.android.com/studio/releases/platform-tools\?hl=zh\_cn))， 在"下载"一栏中选择 "下载适用于Windows的SDK Platform-Tools"，点击直接下载即可。
- 解压已下载的SDK Platform-Tools文件夹，路径最好不要存在中文，以免出现不必要的麻烦。
- 添加环境变量：找到 “此电脑”，右键选择属性-->高级系统设置-->环境变量,在系统变量中找到 “Path” 项，选中并点击“编辑”，将“SDK Platform-Tools”文件夹的绝对路径添加到Path中，依次点击确定即可。
- 按住 win+R 键打开cmd窗口，输入"adb version",展示adb版本信息即表示安装成功。

———————————————————————————————

**基本用法**

**启动/停止:**

启动 adb server 命令：

  

```text
adb start-server
```

停止 adb server 命令：

  

```text
adb kill-server
```

**查看 adb 版本命令**：

  

```text
adb version
```

示例输出：

  

```text
Android Debug Bridge version 1.0.41
```

指定 adb server 的网络端口：

  

```text
adb -P <port> start-server
```

默认端口为 5037。

**设备连接管理**

查询已连接设备/模拟器:

  

```text
adb devices
```

输出示例：

  

```text
List of devices attached
APH7N19529019277        device
```

- 输出格式为 \[serialNumber\] \[state\]，serialNumber 即SN，state 有如下几种：
- \`offline\` —— 表示设备未连接成功或无响应。
- \`device\` —— 设备已连接。注意这个状态并不能标识 Android 系统已经完全启动和可操作，在设备启动过程中设备实例就可连接到 adb，但启动完毕后系统才处于可操作状态。
- \`no device\` —— 没有设备/模拟器连接。

以上输出显示当前已经连接了三台设备/模拟器

常见异常输出：

- 没有设备/模拟器连接成功。

  

```text
List of devices attached
```

- 设备/模拟器未连接到 adb 或无响应。

  

```text
List of devices attached
cf264b8f offline
```

**USB 连接**

通过 USB 连接来正常使用 adb 需要保证几点：

- 硬件状态正常。
- 包括 Android 设备处于正常开机状态，USB 连接线和各种接口完好。
- Android 设备的开发者选项和 USB 调试模式已开启。
- 可以到「设置」-「开发者选项」-「Android 调试」查看。
- 如果在设置里找不到开发者选项，那需要通过一个彩蛋来让它显示出来：在「设置」-「关于手机」连续点击「版本号」7 次。
- 设备驱动状态正常。
- 这一点貌似在 Linux 和 Mac OS X 下不用操心，在 Windows 下有可能遇到需要安装驱动的情况，确认这一点可以右键「计算机」-「属性」，
- 到「设备管理器」里查看相关设备上是否有黄色感叹号或问号，如果没有就说明驱动状态已经好了。否则可以下载一个手机助手类程序来安装驱动先。

  

- 通过 USB 线连接好电脑和设备后确认状态。

```text
adb devices
```

如果能看到

  

```text
xxxxxx device
```

说明连接成功。

**应用管理**

查看应用列表

  

```text
adb shell pm list packages [-f] [-d] [-e] [-s] [-3] [-i] [-u] [--user USER_ID] [FILTER]
```

即在 \`adb shell pm list packages\` 的基础上可以加一些参数进行过滤查看不同的列表，支持的过滤参数如下：

<table data-draft-node="block" data-draft-type="table" data-size="normal" data-row-style="normal"><tbody><tr><td>参数‍</td><td>显示列表</td></tr><tr><td>无</td><td>所有应用</td></tr><tr><td>-f</td><td>显示应用关联的 apk 文件参数</td></tr><tr><td>-d</td><td>只显示 disabled 的应用</td></tr><tr><td>-e</td><td>只显示 enabled 的应用</td></tr><tr><td>-s</td><td>只显示系统应用</td></tr><tr><td>-3</td><td>只显示第三方应用</td></tr><tr><td>-i</td><td>显示应用的 installer</td></tr><tr><td>-u</td><td>包含已卸载应用</td></tr><tr><td>`&lt;FILTER&gt;`</td><td>包名包含 `&lt;FILTER&gt;` 字符串</td></tr></tbody></table>

所有应用命令：

  

```text
adb shell pm list packages
```

输出示例：

  

```text
package:com.taobao.taobao
package:com.huawei.ca
package:com.huawei.skytone
package:com.huawei.assetsyncservice
package:jnhyjt.tjgamsfwpt.tjga
package:com.qq.qcloud
package:com.huawei.aod
package:com.huawei.def
package:com.huawei.hbm
package:com.huawei.hff
package:com.huawei.ims
package:com.huawei.lbs
package:com.android.bluetooth
package:com.xiaomi.hm.health
package:com.ggyd.EarPro
package:com.android.providers.contacts
package:com.netease.cloudmusic
package:tv.danmaku.bili
package:com.android.captiveportallogin
package:com.huawei.audioaccessorymanager
package:com.huawei.hiaction
package:com.huawei.trustedthingsauth
package:com.eg.android.AlipayGphone
package:com.huawei.android.airsharing
package:cn.wps.moffice_eng
package:com.huawei.rcsserviceapplication
package:com.lucky.luckyclient
package:com.huawei.ohos.famanager
...
```

查看系统应用：

  

```text
adb shell pm list packages -s
```

查看第三方应用：

  

```text
adb shell pm list packages -3
```

包名包含某字符串的应用：

  

```text
adb shell pm list packages jingdong
```

使用 grep 过滤\(windows 改用 findstr\)：

  

```text
adb shell pm list packages | grep jingdong
```

安装 APK命令格式：

  

```text
adb install [-lrtsdg] <path_to_apk>
```

参数：

\`adb install\` 后面可以跟一些可选参数来控制安装 APK 的行为，可用参数及含义如下：

<table data-draft-node="block" data-draft-type="table" data-size="normal" data-row-style="normal"><tbody><tr><td>参数</td><td>含义</td></tr><tr><td>-l</td><td>将应用安装到保护目录 /mnt/asec</td></tr><tr><td>-r</td><td>允许覆盖安装(常用)</td></tr><tr><td>-t</td><td>允许安装 AndroidManifest.xml 里 application 指定 `android:testOnly="true"` 的应用</td></tr><tr><td>-s</td><td>将应用安装到 sdcard</td></tr><tr><td>-d</td><td>允许降级覆盖安装(常用)</td></tr><tr><td>-g</td><td>授予所有运行时权限(常用)</td></tr></tbody></table>

运行后如果见到类似如下输出（状态为 \`Success\`）代表安装成功：

  

```text
[100%] /data/local/tmp/1.apk
pkg: /data/local/tmp/1.apk
Success
```

而如果状态为 \`Failure\` 则表示安装失败，比如：

  

```text
[100%] /data/local/tmp/map-20160831.apk
        pkg: /data/local/tmp/map-20160831.apk
Failure [INSTALL_FAILED_ALREADY_EXISTS]
```

卸载应用命令：

  

```text
adb uninstall [-k] <packagename>
```

\`\<packagename>\` 表示应用的包名，\`-k\` 参数可选，表示卸载应用但保留数据和缓存目录。

命令示例：

  

```text
adb uninstall -k com.jingdong.app.mall
```

表示卸载 京东App。

**清除应用数据与缓**存命令：

  

```text
adb shell pm clear <packagename>
```

命令示例：

  

```text
adb shell pm clear  com.jingdong.app.mall
```

清除 京东App的数据和缓存。

**查看前台 Activity命令：**

  

```text
adb shell dumpsys activity activities | grep mResumedActivity
```

输出示例：

  

```text
 mResumedActivity: ActivityRecord{c6b5f44 u0 com.tencent.mm/.ui.LauncherUI t36978}
```

其中的 \`com.tencent.mm/.ui.LauncherUI\` 就是当前处于前台的 Activity。

\*在 Windows 可以尝试 \`adb shell dumpsys activity activities | findstr mResumedActivity\`

查看正在运行的 Services命令：

  

```text
adb shell dumpsys activity services [<packagename>]
```

**查看应用详细信息命令：**

  

```text
adb shell dumpsys package <packagename>
```

输出中包含很多信息，包括 Activity Resolver Table、Registered ContentProviders、包名、userId、安装后的文件资源代码等路径、版本信息、权限信息和授予状态、签名版本信息等。

\`\<packagename>\` 表示应用包名。

输出示例：

  

```text
Activity Resolver Table:
  Full MIME Types:
      vnd.android.cursor.item/vnd.com.tencent.mm.chatting.profile:
        be9b872 com.tencent.mm/.plugin.account.ui.ContactsSyncUI filter be5cecc
          Action: "android.intent.action.VIEW"
          Action: "com.tencent.mm.login.ACTION_LOGIN"
          Category: "android.intent.category.DEFAULT"
          Type: "vnd.android.cursor.item/vnd.com.tencent.mm.login"
          Type: "vnd.android.cursor.item/vnd.com.tencent.mm.chatting.profile"
          Type: "vnd.android.cursor.item/vnd.com.tencent.mm.chatting.voip"
          Type: "vnd.android.cursor.item/vnd.com.tencent.mm.chatting.voip.video"
          Type: "vnd.android.cursor.item/vnd.com.tencent.mm.plugin.sns.timeline"
          Type: "vnd.android.cursor.item/vnd.com.tencent.mm.chatting.phonenum"
      vnd.android.cursor.item/vnd.com.tencent.mm.plugin.sns.timeline:
        be9b872 com.tencent.mm/.plugin.account.ui.ContactsSyncUI filter be5cecc
          Action: "android.intent.action.VIEW"
          Action: "com.tencent.mm.login.ACTION_LOGIN"
          Category: "android.intent.category.DEFAULT"
          Type: "vnd.android.cursor.item/vnd.com.tencent.mm.login"
          Type: "vnd.android.cursor.item/vnd.com.tencent.mm.chatting.profile"
          Type: "vnd.android.cursor.item/vnd.com.tencent.mm.chatting.voip"
          Type: "vnd.android.cursor.item/vnd.com.tencent.mm.chatting.voip.video"
          Type: "vnd.android.cursor.item/vnd.com.tencent.mm.plugin.sns.timeline"
          Type: "vnd.android.cursor.item/vnd.com.tencent.mm.chatting.phonenum"
      application/*:
        be5c38a com.tencent.mm/.ui.tools.ShareImgUI filter be2b1c3
          Action: "android.intent.action.SEND"
          Category: "android.intent.category.DEFAULT"
          Type: "image"
          Type: "video"
          Type: "text"
          Type: "application"
          mPriority=0, mOrder=0, mHasPartialTypes=true
        be5c398 com.tencent.mm/.ui.tools.AddFavoriteUI filter be2b1ea
          Action: "android.intent.action.SEND"
          Category: "android.intent.category.DEFAULT"
          Type: "image"
          Type: "video"
          Type: "text"
          Type: "application"
          Type: "audio"
          mPriority=0, mOrder=0, mHasPartialTypes=true
      vnd.android.cursor.item/vnd.com.tencent.mm.chatting.voip:
        be9b872 com.tencent.mm/.plugin.account.ui.ContactsSyncUI filter be5cecc
          Action: "android.intent.action.VIEW"
          Action: "com.tencent.mm.login.ACTION_LOGIN"
          Category: "android.intent.category.DEFAULT"
          Type: "vnd.android.cursor.item/vnd.com.tencent.mm.login"
          Type: "vnd.android.cursor.item/vnd.com.tencent.mm.chatting.profile"
          Type: "vnd.android.cursor.item/vnd.com.tencent.mm.chatting.voip"
          Type: "vnd.android.cursor.item/vnd.com.tencent.mm.chatting.voip.video"
          Type: "vnd.android.cursor.item/vnd.com.tencent.mm.plugin.sns.timeline"
          Type: "vnd.android.cursor.item/vnd.com.tencent.mm.chatting.phonenum"
      text/*:
        be5c38a com.tencent.mm/.ui.tools.ShareImgUI filter be2b1c3
          Action: "android.intent.action.SEND"
          Category: "android.intent.category.DEFAULT"
          Type: "image"
          Type: "video"
          Type: "text"
          Type: "application"
          mPriority=0, mOrder=0, mHasPartialTypes=true
        be5c398 com.tencent.mm/.ui.tools.AddFavoriteUI filter be2b1ea
          Action: "android.intent.action.SEND"
          Category: "android.intent.category.DEFAULT"
          Type: "image"
          Type: "video"
          Type: "text"
          Type: "application"
          Type: "audio"
          mPriority=0, mOrder=0, mHasPartialTypes=true
      audio/*:
        be5c398 com.tencent.mm/.ui.tools.AddFavoriteUI filter be2b1ea
          Action: "android.intent.action.SEND"
          Category: "android.intent.category.DEFAULT"
          Type: "image"
          Type: "video"
          Type: "text"
          Type: "application"
          Type: "audio"
          mPriority=0, mOrder=0, mHasPartialTypes=true
      vnd.android.cursor.item/vnd.com.tencent.mm.chatting.phonenum:
        be9b872 com.tencent.mm/.plugin.account.ui.ContactsSyncUI filter be5cecc
          Action: "android.intent.action.VIEW"
          Action: "com.tencent.mm.login.ACTION_LOGIN"
          Category: "android.intent.category.DEFAULT"
          Type: "vnd.android.cursor.item/vnd.com.tencent.mm.login"
          Type: "vnd.android.cursor.item/vnd.com.tencent.mm.chatting.profile"
          Type: "vnd.android.cursor.item/vnd.com.tencent.mm.chatting.voip"
          Type: "vnd.android.cursor.item/vnd.com.tencent.mm.chatting.voip.video"
          Type: "vnd.android.cursor.item/vnd.com.tencent.mm.plugin.sns.timeline"
          Type: "vnd.android.cursor.item/vnd.com.tencent.mm.chatting.phonenum"
      vnd.android.cursor.item/vnd.com.tencent.mm.chatting.voip.video:
        be9b872 com.tencent.mm/.plugin.account.ui.ContactsSyncUI filter be5cecc
          Action: "android.intent.action.VIEW"
          Action: "com.tencent.mm.login.ACTION_LOGIN"
          Category: "android.intent.category.DEFAULT"
          Type: "vnd.android.cursor.item/vnd.com.tencent.mm.login"
          Type: "vnd.android.cursor.item/vnd.com.tencent.mm.chatting.profile"
          Type: "vnd.android.cursor.item/vnd.com.tencent.mm.chatting.voip"
          Type: "vnd.android.cursor.item/vnd.com.tencent.mm.chatting.voip.video"
          Type: "vnd.android.cursor.item/vnd.com.tencent.mm.plugin.sns.timeline"
          Type: "vnd.android.cursor.item/vnd.com.tencent.mm.chatting.phonenum"
      vnd.android.cursor.item/vnd.com.tencent.mm.login:
        be9b872 com.tencent.mm/.plugin.account.ui.ContactsSyncUI filter be5cecc
          Action: "android.intent.action.VIEW"
          Action: "com.tencent.mm.login.ACTION_LOGIN"
          Category: "android.intent.category.DEFAULT"
          Type: "vnd.android.cursor.item/vnd.com.tencent.mm.login"
          Type: "vnd.android.cursor.item/vnd.com.tencent.mm.chatting.profile"
          Type: "vnd.android.cursor.item/vnd.com.tencent.mm.chatting.voip"
          Type: "vnd.android.cursor.item/vnd.com.tencent.mm.chatting.voip.video"
          Type: "vnd.android.cursor.item/vnd.com.tencent.mm.plugin.sns.timeline"
          Type: "vnd.android.cursor.item/vnd.com.tencent.mm.chatting.phonenum"
      video/*:
        be5c38a com.tencent.mm/.ui.tools.ShareImgUI filter be2b1c3
          Action: "android.intent.action.SEND"
          Category: "android.intent.category.DEFAULT"
          Type: "image"
          Type: "video"
          Type: "text"
          Type: "application"
          mPriority=0, mOrder=0, mHasPartialTypes=true
        be5c391 com.tencent.mm/.ui.tools.ShareToStatusUI filter be2b1dd
          Action: "android.intent.action.SEND"
          Category: "android.intent.category.DEFAULT"
          Type: "image"
          Type: "video"
          mPriority=0, mOrder=0, mHasPartialTypes=true
        be5c398 com.tencent.mm/.ui.tools.AddFavoriteUI filter be2b1ea
          Action: "android.intent.action.SEND"
          Category: "android.intent.category.DEFAULT"
          Type: "image"
          Type: "video"
          Type: "text"
          Type: "application"
          Type: "audio"
          mPriority=0, mOrder=0, mHasPartialTypes=true
      */heic:
        be5c3bb com.tencent.mm/.ui.tools.ShareScreenImgUI filter be5ce23
          Action: "android.intent.action.VIEW"
          Category: "android.intent.category.DEFAULT"
          Type: "image"
          Type: "*/heic"
          mPriority=0, mOrder=0, mHasPartialTypes=true
        be5c3c9 com.tencent.mm/.ui.tools.ShareScreenToTimeLineUI filter be5ce3d
          Action: "android.intent.action.VIEW"
          Category: "android.intent.category.DEFAULT"
          Type: "image"
          Type: "*/heic"
          mPriority=0, mOrder=0, mHasPartialTypes=true
      image/*:
        be5c38a com.tencent.mm/.ui.tools.ShareImgUI filter be2b1c3
          Action: "android.intent.action.SEND"
          Category: "android.intent.category.DEFAULT"
          Type: "image"
          Type: "video"
          Type: "text"
          Type: "application"
          mPriority=0, mOrder=0, mHasPartialTypes=true
        be5c38a com.tencent.mm/.ui.tools.ShareImgUI filter be2b1d0
          Action: "android.intent.action.SEND_MULTIPLE"
          Category: "android.intent.category.DEFAULT"
          Type: "image"
          mPriority=0, mOrder=0, mHasPartialTypes=true
        be5c391 com.tencent.mm/.ui.tools.ShareToStatusUI filter be2b1dd
          Action: "android.intent.action.SEND"
          Category: "android.intent.category.DEFAULT"
          Type: "image"
          Type: "video"
          mPriority=0, mOrder=0, mHasPartialTypes=true
        be5c398 com.tencent.mm/.ui.tools.AddFavoriteUI filter be2b1ea
          Action: "android.intent.action.SEND"
          Category: "android.intent.category.DEFAULT"
          Type: "image"
          Type: "video"
          Type: "text"
          Type: "application"
          Type: "audio"
          mPriority=0, mOrder=0, mHasPartialTypes=true
        be5c398 com.tencent.mm/.ui.tools.AddFavoriteUI filter be5ce09
          Action: "android.intent.action.SEND_MULTIPLE"
          Category: "android.intent.category.DEFAULT"
          Type: "image"
          mPriority=0, mOrder=0, mHasPartialTypes=true
        be5c39f com.tencent.mm/.ui.tools.ShareToTimeLineUI filter be5ce16
          Action: "android.intent.action.SEND"
          Category: "android.intent.category.DEFAULT"
          Type: "image"
          mPriority=0, mOrder=0, mHasPartialTypes=true
        be5c3bb com.tencent.mm/.ui.tools.ShareScreenImgUI filter be5ce23
          Action: "android.intent.action.VIEW"
          Category: "android.intent.category.DEFAULT"
          Type: "image"
          Type: "*/heic"
          mPriority=0, mOrder=0, mHasPartialTypes=true
        be5c3c9 com.tencent.mm/.ui.tools.ShareScreenToTimeLineUI filter be5ce3d
          Action: "android.intent.action.VIEW"
          Category: "android.intent.category.DEFAULT"
          Type: "image"
          Type: "*/heic"
          mPriority=0, mOrder=0, mHasPartialTypes=true
```

**查看应用安装路径命令:**

  

```text
adb shell pm path <PACKAGE>
```

输出应用安装路径

输出示例:

  

```text
adb shell pm path com.tencent.mm
package:/data/app/com.tencent.mm-Khc5y5damU6A9MMQerHRgA==/base.apk
```

**启动应用/ 调起 Activity**

- 指定Activity名称启动命令格式：

  

```text
adb shell am start [options] <INTENT>
```

例如：

  

```text
adb shell am start -n com.tencent.mm/.ui.LauncherUI
```

表示调起微信主界面。

‍

**强制停止应用命令：**

  

```text
adb shell am force-stop <packagename>
```

命令示例：

  

```text
adb shell am force-stop com.tencent.mm
```

表示停止 微信的一切进程与服务。

**文件管理**

复制设备里的文件到电脑命令：

  

```text
adb pull <手机里的文件路径> [电脑上的存放目录]
```

其中 \`电脑上的目录\` 参数可以省略，默认复制到当前目录。

例：

  

```text
adb pull /sdcard/adn.mp4 ~/tmp/
```

**复制电脑里的文件到设备命令：**

  

```text
adb push <电脑上的文件路径> <设备里的目录>
```

例：

  

```text
adb push ~/adb.mp4 /sdcard/
```

**模拟按键/输入**

\`adb shell input\` ，通过它可以做很多事情。

比如使用 \`adb shell input keyevent \<keycode>\` 命令，不同的 keycode 能实现不同的功能，完整的 keycode 列表详\([https://developer.android.com/reference/android/view/KeyEvent.html](https://link.zhihu.com/?target=https%3A//developer.android.com/reference/android/view/KeyEvent.html)\)，摘引部分如下：

<table data-draft-node="block" data-draft-type="table" data-size="normal" data-row-style="normal"><tbody><tr><td>keycode</td><td>含义</td></tr><tr><td>3</td><td>HOME 键</td></tr><tr><td>4</td><td>返回键</td></tr><tr><td>5</td><td>打开拨号应用</td></tr><tr><td>6</td><td>挂断电话</td></tr><tr><td>24</td><td>增加音量</td></tr><tr><td>25</td><td>降低音量</td></tr><tr><td>26</td><td>电源键</td></tr><tr><td>27</td><td>拍照（需要在相机应用里）</td></tr><tr><td>64 ‍‍</td><td>打开浏览器</td></tr><tr><td>82</td><td>菜单键</td></tr><tr><td>85</td><td>播放/暂停</td></tr><tr><td>86</td><td>停止播放</td></tr><tr><td>87‍</td><td>播放下一首‍</td></tr><tr><td>88</td><td>播放上一首</td></tr><tr><td>122</td><td>移动光标到行首或列表顶部</td></tr><tr><td>123</td><td>移动光标到行末或列表底部</td></tr></tbody></table>

<table data-draft-node="block" data-draft-type="table" data-size="normal" data-row-style="normal"><tbody><tr><td>126</td><td>恢复播放</td></tr><tr><td>127</td><td>暂停播放</td></tr><tr><td>164</td><td>静音</td></tr><tr><td>176</td><td>打开系统设置</td></tr><tr><td>187</td><td>切换应用</td></tr><tr><td>207</td><td>打开联系人</td></tr><tr><td>208</td><td>打开日历</td></tr><tr><td>209</td><td>打开音乐</td></tr><tr><td>210 ‍</td><td>打开计算器</td></tr><tr><td>220</td><td>降低屏幕亮度</td></tr><tr><td>221</td><td>提高屏幕亮度</td></tr><tr><td>223</td><td>系统休眠</td></tr><tr><td>224‍</td><td>点亮屏幕</td></tr><tr><td>231</td><td>打开语音助手</td></tr><tr><td>276</td><td>如果没有 wakelock 则让系统休眠</td></tr></tbody></table>

下面是 \`input\` 命令的一些用法举例。

电源键命令：

  

```text
adb shell input keyevent 26
```

执行效果相当于按电源键。

菜单键命令：

  

```text
adb shell input keyevent 82
```

其它命令使用方式如上，这里不再做过多演示

滑动解锁

锁屏通过滑动手势解锁，可以通过 \`input swipe\` 来实现。：

  

```text
adb shell input swipe 300 1000 300 500
```

参数 \`300 1000 300 500\` 分别表示\`起始点x坐标 起始点y坐标 结束点x坐标 结束点y坐标\`。

输入文本

点击某文本框后，可以通过 \`input\` 命令来输入文本：

  

```text
adb shell input text hello
```

现在 \`hello\` 出现在文本框了。

**查看日志**

Android 日志命令：

  

```text
[adb] logcat [<option>] ... [<filter-spec>] ...
```

常用用法列举如下：

\#### 按级别过滤日志

Android 的日志优先级（priority）：

- V —— Verbose（最低，输出得最多）
- D —— Debug
- I —— Info
- W —— Warning
- E —— Error
- F —— Fatal
- S —— Silent（最高，啥也不输出）

按某级别过滤日志则会将该级别及以上的日志输出。

比如，命令：

  

```text
adb logcat *:W
```

会将 Warning、Error、Fatal 和 Silent 日志输出。

按 tag 和级别过滤日志

\`\<filter-spec>\` 可以由多个 \`\<tag>\[:priority\]\` 组成。

比如，命令：

  

```text
adb logcat ActivityManager:I MyApp:D *:S
```

表示输出 tag \`ActivityManager\` 的 Info 以上级别日志，输出 tag \`MyApp\` 的 Debug 以上级别日志，及其它 tag 的 Silent 级别日志（即屏蔽其它 tag 日志）。

日志格式

可以用 \`adb logcat \-v \<format>\` 选项指定日志输出格式。

日志支持按以下几种 \`\<format>\`：

\* brief

默认格式。格式为：

  

```text
<priority>/<tag>(<pid>): <message>
```

示例：

  

```text
D/HeadsetStateMachine( 1785): Disconnected process message: 10, size: 0
```

\* process

格式为：

  

```text
<priority>(<pid>) <message>
```

示例：

  

```text
 D( 1785) Disconnected process message: 10, size: 0  (HeadsetStateMachine)
```

\* tag

格式为\<priority>/\<tag>: \<message>

示例：

  

```text
D/HeadsetStateMachine: Disconnected process message: 10, size: 0
```

\* raw

格式为：

  

```text
 <message>
```

示例：

  

```text
Disconnected process message: 10, size: 0
```

\* time

格式为：

  

```text
<datetime> <priority>/<tag>(<pid>): <message>
```

示例：

  

```text
08-28 22:39:39.974 D/HeadsetStateMachine( 1785): Disconnected process message: 10, size: 0
```

\* threadtime

格式为：

  

```text
<datetime> <pid> <tid> <priority> <tag>: <message>
```

示例：

  

```text
08-28 22:39:39.974  1785  1832 D HeadsetStateMachine: Disconnected process message: 10, size: 0
```

\* long

格式为：

  

```text
 [ <datetime> <pid>:<tid> <priority>/<tag> ]
  <message>
```

示例：

  

```text
[ 08-28 22:39:39.974  1785: 1832 D/HeadsetStateMachine ]

  Disconnected process message: 10, size: 0
```

指定格式可与上面的过滤同时使用。比如：

  

```text
adb logcat -v long ActivityManager:I *:S
```

清空日志

  

```text
adb logcat -c
```

内核日志

  

```text
adb shell dmesg
```

输出示例：

  

```text
<6>[14201.684016] PM: noirq resume of devices complete after 0.982 msecs
<6>[14201.685525] PM: early resume of devices complete after 0.838 msecs
<6>[14201.753642] PM: resume of devices complete after 68.106 msecs
<4>[14201.755954] Restarting tasks ... done.
<6>[14201.771229] PM: suspend exit 2016-08-28 13:31:32.679217193 UTC
<6>[14201.872373] PM: suspend entry 2016-08-28 13:31:32.780363596 UTC
<6>[14201.872498] PM: Syncing filesystems ... done.
```

中括号里的 \`\[14201.684016\]\` 代表内核开始启动后的时间，单位为秒。

通过内核日志我们可以做一些事情，比如衡量内核启动时间，在系统启动完毕后的内核日志里找到 \`Freeing init memory\` 那一行前面的时间就是。

**查看设备信息**

型号命令：

  

```text
adb shell getprop ro.product.model
```

输出示例：

  

```text
VOG-AL10
```

电池状况

  

```text
adb shell dumpsys battery
```

输入示例：

  

```text
Current Battery Service state:
  AC powered: false
  USB powered: true
  Wireless powered: false
  Max charging current: 500000
  Max charging voltage: 4718000
  Charge counter: 450000
  status: 2
  health: 2
  present: true
  level: 85
  scale: 100
  voltage: 4203
  temperature: 240
  technology: Li-poly
```

其中 \`scale\` 代表最大电量，\`level\` 代表当前电量。上面的输出表示还剩下 44\% 的电量。

屏幕分辨率

  

```text
adb shell wm size
```

输出示例：

  

```text
Physical size: 1080x2340
```

该设备屏幕分辨率为 1080px \* 2340px。

屏幕密度

  

```text
adb shell wm density
```

输出示例：

  

```text
Physical density: 480
```

显示屏参数

  

```text
adb shell dumpsys window displays
```

输出示例：

  

```text
WINDOW MANAGER DISPLAY CONTENTS (dumpsys window displays)
  Display: mDisplayId=0
    init=1080x2340 480dpi cur=1080x2340 app=1080x2265 rng=1080x1005-2265x2265
    deferred=false mLayoutNeeded=false mTouchExcludeRegion=SkRegion((0,0,1080,2458))

  mLayoutSeq=355690
  mDeferredRotationPauseCount=0
  mCurrentFocus=Window{c5dd371 u0 com.tencent.mm/com.tencent.mm.ui.LauncherUI}
  mFocusedApp=AppWindowToken{c67d8b1 token=Token{c617f21 ActivityRecord{c04d409 u0 com.tencent.mm/.ui.LauncherUI t37102}}}
  mLastStatusBarVisibility=0xa518

  displayId=0
  mWallpaperTarget=null
  mLastWallpaperX=0.5 mLastWallpaperY=0.5

  mDeferUpdateImeTargetCount=0
  ...
```

android\\\_id

  

```text
adb shell settings get secure android_id
```

输出示例：

  

```text
aadf190ed474a1ae
```

‍

Android 系统版本

  

```text
adb shell getprop ro.build.version.release
```

输出示例：

  

```text
10
```

CPU 信息

  

```text
adb shell cat /proc/cpuinfo
```

输出示例：

  

```text
Processor       : AArch64 Processor rev 0 (aarch64)
processor       : 0
BogoMIPS        : 3.84
Features        : fp asimd evtstrm aes pmull sha1 sha2 crc32 atomics fphp asimdhp cpuid asimdrdm lrcpc dcpop asimddp
CPU implementer : 0x41
CPU architecture: 8
CPU variant     : 0x1
CPU part        : 0xd05
CPU revision    : 0

processor       : 1
BogoMIPS        : 3.84
Features        : fp asimd evtstrm aes pmull sha1 sha2 crc32 atomics fphp asimdhp cpuid asimdrdm lrcpc dcpop asimddp
CPU implementer : 0x41
CPU architecture: 8
CPU variant     : 0x1
CPU part        : 0xd05
CPU revision    : 0

processor       : 2
BogoMIPS        : 3.84
Features        : fp asimd evtstrm aes pmull sha1 sha2 crc32 atomics fphp asimdhp cpuid asimdrdm lrcpc dcpop asimddp
CPU implementer : 0x41
CPU architecture: 8
CPU variant     : 0x1
CPU part        : 0xd05
CPU revision    : 0

processor       : 3
BogoMIPS        : 3.84
Features        : fp asimd evtstrm aes pmull sha1 sha2 crc32 atomics fphp asimdhp cpuid asimdrdm lrcpc dcpop asimddp
CPU implementer : 0x41
CPU architecture: 8
CPU variant     : 0x1
CPU part        : 0xd05
CPU revision    : 0

processor       : 4
BogoMIPS        : 3.84
Features        : fp asimd evtstrm aes pmull sha1 sha2 crc32 atomics fphp asimdhp cpuid asimdrdm lrcpc dcpop asimddp
CPU implementer : 0x48
CPU architecture: 8
CPU variant     : 0x1
CPU part        : 0xd40
CPU revision    : 0

processor       : 5
BogoMIPS        : 3.84
Features        : fp asimd evtstrm aes pmull sha1 sha2 crc32 atomics fphp asimdhp cpuid asimdrdm lrcpc dcpop asimddp
CPU implementer : 0x48
CPU architecture: 8
CPU variant     : 0x1
CPU part        : 0xd40
CPU revision    : 0

processor       : 6
BogoMIPS        : 3.84
Features        : fp asimd evtstrm aes pmull sha1 sha2 crc32 atomics fphp asimdhp cpuid asimdrdm lrcpc dcpop asimddp
CPU implementer : 0x48
CPU architecture: 8
CPU variant     : 0x1
CPU part        : 0xd40
CPU revision    : 0

processor       : 7
BogoMIPS        : 3.84
Features        : fp asimd evtstrm aes pmull sha1 sha2 crc32 atomics fphp asimdhp cpuid asimdrdm lrcpc dcpop asimddp
CPU implementer : 0x48
CPU architecture: 8
CPU variant     : 0x1
CPU part        : 0xd40
CPU revision    : 0

Hardware        : vendor Kirin980
```

这是华为P30pro 的 CPU 信息

内存信息

  

```text
adb shell cat /proc/meminfo
```

输出示例：

  

```text
MemTotal:        7789200 kB
MemFree:          552980 kB
MemAvailable:    3076176 kB
Buffers:            8228 kB
Cached:          2675048 kB
SwapCached:        35740 kB
Active:          1518432 kB
Inactive:        2521564 kB
Active(anon):     313244 kB
Inactive(anon):  1172720 kB
Active(file):    1205188 kB
Inactive(file):  1348844 kB
Unevictable:      154676 kB
Mlocked:          154676 kB
SwapTotal:       4194300 kB
SwapFree:        1386624 kB
Dirty:                76 kB
Writeback:             0 kB
AnonPages:       1505672 kB
Mapped:          1369448 kB
Shmem:             42384 kB
Slab:             558428 kB
SReclaimable:     215332 kB
SUnreclaim:       343096 kB
SlabLarge:         16224 kB
KernelStack:      113808 kB
PageTables:       141496 kB
NFS_Unstable:          0 kB
Bounce:                0 kB
WritebackTmp:          0 kB
CommitLimit:     8088900 kB
Committed_AS:   118311256 kB
VmallocTotal:   262143936 kB
VmallocUsed:           0 kB
VmallocChunk:          0 kB
CmaTotal:        1187840 kB
CmaFree:               0 kB
IonTotalCache:         0 kB
IonTotalUsed:     282752 kB
Protected:             0 kB
RsvTotalUsed:     361988 kB
```

其中，\`MemTotal\` 就是设备的总内存，\`MemFree\` 是当前空闲内存。

**修改设置**

\*\*注：\*\* 修改设置之后，运行恢复命令有可能显示仍然不太正常，可以运行 \`adb reboot\` 重启设备，或手动重启。

修改设置的原理主要是通过 settings 命令修改 /data/data/com.android.providers.settings/databases/settings.db 里存放的设置值。

分辨率

  

```text
adb shell wm size 480x1024
```

表示将分辨率修改为 480px \* 1024px。

恢复原分辨率命令：

  

```text
adb shell wm size reset
```

屏幕密度

  

```text
adb shell wm density 160
```

表示将屏幕密度修改为 160dpi。

恢复原屏幕密度命令：

  

```text
adb shell wm density reset
```

显示区域

  

```text
adb shell wm overscan 0,0,0,200
```

四个数字分别表示距离左、上、右、下边缘的留白像素，以上命令表示将屏幕底部 200px 留白。

恢复原显示区域命令：

  

```text
adb shell wm overscan reset
```

关闭 USB 调试模式

恢复：

用命令恢复不了了，毕竟关闭了 USB 调试 adb 就连接不上 Android 设备了。

去设备上手动恢复吧：「设置」-「开发者选项」-「Android 调试」。

允许/禁止访问非 SDK API

允许访问非 SDK API：

  

```text
adb shell settings put global hidden_api_policy_pre_p_apps 1
adb shell settings put global hidden_api_policy_p_apps 1
```

禁止访问非 SDK API：

  

```text
adb shell settings delete global hidden_api_policy_pre_p_apps
adb shell settings delete global hidden_api_policy_p_apps
```

不需要设备获得 Root 权限。

命令最后的数字的含义：

<table data-draft-node="block" data-draft-type="table" data-size="normal" data-row-style="normal"><tbody><tr><td>值</td><td>含义</td></tr><tr><td>0</td><td>禁止检测非 SDK 接口的调用。该情况下，日志记录功能被禁用，并且令 strict mode API，即 detectNonSdkApiUsage() 无效。不推荐</td></tr><tr><td>1</td><td>仅警告——允许访问所有非 SDK 接口，但保留日志中的警告信息，可继续使用 strick mode API</td></tr><tr><td>2</td><td>禁止调用深灰名单和黑名单中的接口</td></tr><tr><td>3</td><td>禁止调用黑名单中的接口，但允许调用深灰名单中的接口</td></tr></tbody></table>

状态栏和导航栏的显示隐藏

本节所说的相关设置对应 Cyanogenmod 里的「扩展桌面」。

  

```text
adb shell settings put global policy_control <key-values>
```

\`\<key-values>\` 可由如下几种键及其对应的值组成，格式为 \`\<key1>=\<value1>:\<key2>=\<value2>\`。

<table data-draft-node="block" data-draft-type="table" data-size="normal" data-row-style="normal"><tbody><tr><td>key</td><td>含义</td></tr><tr><td>immersive.full</td><td>同时隐藏</td></tr><tr><td>immersive.status</td><td>隐藏状态栏</td></tr><tr><td>immersive.navigation</td><td>隐藏导航栏</td></tr><tr><td>mmersive.preconfirms</td><td>?</td></tr></tbody></table>

这些键对应的值可则如下值用逗号组合：

<table data-draft-node="block" data-draft-type="table" data-size="normal" data-row-style="normal"><tbody><tr><td>value</td><td>含义</td></tr><tr><td>apps</td><td>所有应用</td></tr><tr><td>*</td><td>所有界面</td></tr><tr><td>packagename</td><td>指定应用</td></tr><tr><td>-packagename</td><td>排除指定应用</td></tr></tbody></table>

例如：

  

```text
adb shell settings put global policy_control immersive.full=*
```

表示设置在所有界面下都同时隐藏状态栏和导航栏。

  

```text
adb shell settings put global policy_control immersive.status=com.package1,com.package2:immersive.navigation=apps,-com.package3
```

表示设置在包名为 \`com.package1\` 和 \`com.package2\` 的应用里隐藏状态栏，在除了包名为 \`com.package3\` 的所有应用里隐藏导航栏。

**实用功能**

屏幕截图

截图保存到电脑：

  

```text
adb exec-out screencap -p > sc.png
```

如果 adb 版本较老，无法使用 \`exec-out\` 命令，这时候建议更新 adb 版本。无法更新的话可以使用以下麻烦点的办法：

先截图保存到设备里：

  

```text
adb shell screencap -p /sdcard/sc.png
```

然后将 png 文件导出到电脑：

  

```text
adb pull /sdcard/sc.png
```

录制屏幕

录制屏幕以 mp4 格式保存到 /sdcard：

  

```text
adb shell screenrecord /sdcard/filename.mp4
```

需要停止时按 \<kbd>Ctrl-C\</kbd>，默认录制时间和最长录制时间都是 180 秒。

如果需要导出到电脑：

  

```text
adb pull /sdcard/filename.mp4
```

可以使用 \`adb shell screenrecord \--help\` 查看 \`screenrecord\` 命令的帮助信息，下面是常见参数及含义：

<table data-draft-node="block" data-draft-type="table" data-size="normal" data-row-style="normal"><tbody><tr><td>参数</td><td>含义</td></tr><tr><td>--size WIDTHxHEIGHT</td><td>视频的尺寸，比如 `1280x720`，默认是屏幕分辨率。</td></tr><tr><td>--bit-rate RATE</td><td>视频的比特率，默认是 4Mbps。</td></tr><tr><td>--time-limit TIME</td><td>录制时长，单位秒</td></tr><tr><td>--verbose</td><td>输出更多信息。</td></tr></tbody></table>

设置系统日期和时间

**\*\*注：需要 root 权限。\*\***

  

```text
adb shell
su
date -s 20160823.131500
```

表示将系统日期和时间更改为 2016 年 08 月 23 日 13 点 15 分 00 秒。

重启手机

  

```text
adb reboot
```

检测设备是否已 root

  

```text
adb shell
```

此时命令行提示符是 \`\$\` 则表示没有 root 权限，是 \`#\` 则表示已 root。

使用 Monkey 进行压力测试

Monkey 可以生成伪随机用户事件来模拟单击、触摸、手势等操作，可以对正在开发中的程序进行随机压力测试。

简单用法：

  

```text
adb shell monkey -p <packagename> -v 500
```

表示向 \`\<packagename>\` 指定的应用程序发送 500 个伪随机事件。

Monkey 的详细用法参考 官方文档\([https://developer.android.com/studio/test/monkey.html](https://link.zhihu.com/?target=https%3A//developer.android.com/studio/test/monkey.html)\)。

Android 系统是基于 Linux 内核的，所以 Linux 里的很多命令在 Android 里也有相同或类似的实现，在 \`adb shell\` 里可以调用。本文档前面的部分内容已经用到了 \`adb shell\` 命令。

查看进程

  

```text
adb shell ps
```

输出示例：

  

```text
USER           PID  PPID     VSZ    RSS WCHAN            ADDR S NAME
root             1     0   78156   3180 0                   0 S init
root             2     0       0      0 0                   0 S [kthreadd]
root             3     2       0      0 0                   0 I [rcu_gp]
root             7     2       0      0 0                   0 I [mm_percpu_wq]
root             8     2       0      0 0                   0 S [ksoftirqd/0]
root             9     2       0      0 0                   0 I [rcu_preempt]
root            10     2       0      0 0                   0 I [rcu_sched]
root            11     2       0      0 0                   0 I [rcu_bh]
root            12     2       0      0 0                   0 S [migration/0]
root            13     2       0      0 0                   0 S [watchdog/0]
root            14     2       0      0 0                   0 S [cpuhp/0]
root            15     2       0      0 0                   0 S [cpuhp/1]
root            16     2       0      0 0                   0 S [watchdog/1]
root            17     2       0      0 0                   0 S [migration/1]
root            18     2       0      0 0                   0 S [ksoftirqd/1]
root            20     2       0      0 0                   0 I [kworker/1:0H]
root            21     2       0      0 0                   0 S [cpuhp/2]
root            22     2       0      0 0                   0 S [watchdog/2]
root            23     2       0      0 0                   0 S [migration/2]
root            24     2       0      0 0                   0 S [ksoftirqd/2]
root            27     2       0      0 0                   0 S [cpuhp/3]
root            28     2       0      0 0                   0 S [watchdog/3]
root            29     2       0      0 0                   0 S [migration/3]
root            30     2       0      0 0                   0 S [ksoftirqd/3]
root            32     2       0      0 0                   0 I [kworker/3:0H]
root            33     2       0      0 0                   0 S [cpuhp/4]
root            34     2       0      0 0                   0 S [watchdog/4]
root            35     2       0      0 0                   0 S [migration/4]
root            36     2       0      0 0                   0 S [ksoftirqd/4]
root            38     2       0      0 0                   0 I [kworker/4:0H]
root            39     2       0      0 0                   0 S [cpuhp/5]
root            40     2       0      0 0                   0 S [watchdog/5]
root            41     2       0      0 0                   0 S [migration/5]
root            42     2       0      0 0                   0 S [ksoftirqd/5]
root            44     2       0      0 0                   0 I [kworker/5:0H]
root            45     2       0      0 0                   0 S [cpuhp/6]
root            46     2       0      0 0                   0 S [watchdog/6]
root            47     2       0      0 0                   0 S [migration/6]
root            48     2       0      0 0                   0 S [ksoftirqd/6]
root            50     2       0      0 0                   0 I [kworker/6:0H]
root            51     2       0      0 0                   0 S [cpuhp/7]
root            52     2       0      0 0                   0 S [watchdog/7]
root            53     2       0      0 0                   0 S [migration/7]
root            54     2       0      0 0                   0 S [ksoftirqd/7]
root            56     2       0      0 0                   0 I [kworker/7:0H]
root            58     2       0      0 0                   0 D [bbox_main]
root            60     2       0      0 0                   0 D [bbox_cleartext]
root            62     2       0      0 0                   0 S [mailbox-10]
root            63     2       0      0 0                   0 S [mailbox-11]
root            64     2       0      0 0                   0 S [mailbox-12]
root            65     2       0      0 0                   0 S [mailbox-13]
root            66     2       0      0 0                   0 S [mailbox-14]
root            67     2       0      0 0                   0 S [mailbox-15]
root            68     2       0      0 0                   0 S [mailbox-16]
root            69     2       0      0 0                   0 S [mailbox-17]
root            70     2       0      0 0                   0 S [mailbox-18]
root            71     2       0      0 0                   0 S [mailbox-23]
root            72     2       0      0 0                   0 S [mailbox-25]
root            73     2       0      0 0                   0 S [mailbox-26]
root            74     2       0      0 0                   0 S [mailbox-27]
root            75     2       0      0 0                   0 S [mailbox-28]
root            76     2       0      0 0                   0 S [mailbox-29]
root            77     2       0      0 0                   0 S [ao-mailbox-0]
root            79     2       0      0 0                   0 S [khungtaskd]
root            80     2       0      0 0                   0 I [init_wdt_wkq]
root            81     2       0      0 0                   0 S [oom_reaper]
root            82     2       0      0 0                   0 I [writeback]
root            83     2       0      0 0                   0 S [kcompactd0]
root            84     2       0      0 0                   0 I [crypto]
root            85     2       0      0 0                   0 I [kblockd]
root            86     2       0      0 0                   0 I [io_latency_work]
root            87     2       0      0 0                   0 I [dsm_wq]
root            88     2       0      0 0                   0 S [spi3]
root            90     2       0      0 0                   0 S [irq/148-hisi_pm]
root            91     2       0      0 0                   0 S [sys_pool]
root            92     2       0      0 0                   0 S [sys_heap]
root            93     2       0      0 0                   0 S [camera_daemonhe]
root            94     2       0      0 0                   0 S [camera_heap]
root            95     2       0      0 0                   0 I [devfreq_wq]
root            96     2       0      0 0                   0 S [rdr_lpm3_thread]
root            97     2       0      0 0                   0 D [hwxcollie]
root            98     2       0      0 0                   0 I [cfg80211]
root            99     2       0      0 0                   0 I [watchdogd]
root           100     2       0      0 0                   0 I [usb_analog_hs_p]
root           101     2       0      0 0                   0 I [usb_analog_hs_p]
root           103     2       0      0 0                   0 I [slimbus_lost_sy]
root           105     2       0      0 0                   0 I [mptcp_wq]
root           106     2       0      0 0                   0 S [dspdumplog]
root           107     2       0      0 0                   0 I [hifi_om_work_vo]
root           108     2       0      0 0                   0 I [hifi_om_work_au]
root           109     2       0      0 0                   0 I [hifi_om_work_vo]
root           110     2       0      0 0                   0 I [hifi_om_work_vo]
root           111     2       0      0 0                   0 I [hifi_om_work_sm]
root           112     2       0      0 0                   0 I [hifi_om_work_au]
root           113     2       0      0 0                   0 I [hifi_om_work_au]
root           114     2       0      0 0                   0 D [mailboxNormal]
root           115     2       0      0 0                   0 D [mailboxHigh]
root           117     2       0      0 0                   0 S [siqthread/0]
root           118     2       0      0 0                   0 S [ipihelper]
root           120     2       0      0 0                   0 S [agent_rpmb]
root           121     2       0      0 0                   0 S [tuid]
root           122     2       0      0 0                   0 S [smc_svc_thread]
root           123     2       0      0 0                   0 S [interface_msg_p]
root           162     2       0      0 0                   0 S [kauditd]
root           163     2       0      0 0                   0 S [kswapd0]
root           164     2       0      0 0                   0 S [ecryptfs-kthrea]
root           165     2       0      0 0                   0 I [cifsiod]
root           166     2       0      0 0                   0 I [cifsoplockd]
root           167     2       0      0 0                   0 S [fid_async_t]
root           168     2       0      0 0                   0 I [EIMA]
root           173     2       0      0 0                   0 I [HW_KERNEL_STP_M]
root           174     2       0      0 0                   0 I [HW_KERNEL_STP_S]
root           218     2       0      0 0                   0 I [kthrotld]
root           219     2       0      0 0                   0 I [fb0_dss_debug]
root           220     2       0      0 0                   0 I [fb0_ldi_underfl]
root           221     2       0      0 0                   0 I [pipe_clk_updt_w]
root           222     2       0      0 0                   0 I [fb0_hiace_end]
root           223     2       0      0 0                   0 I [fb0_gmp_lut]
```

各列含义：

<table data-draft-node="block" data-draft-type="table" data-size="normal" data-row-style="normal"><tbody><tr><td>列名</td><td>含义</td></tr><tr><td>USER</td><td>所属用户</td></tr><tr><td>PID</td><td>进程 ID</td></tr><tr><td>PPID</td><td>父进程 ID</td></tr><tr><td>NAME</td><td>进程名</td></tr></tbody></table>

查看实时资源占用情况

  

```text
adb shell top
```

输出示例：

  

```text
Tasks: 938 total,   1 running, 937 sleeping,   0 stopped,   0 zombie
  Mem:      7.4G total,      7.0G used,      380M free,      8.1M buffers
 Swap:      4.0G total,      2.8G used,      1.1G free,      2.6G cached
800%cpu  13%user   0%nice  16%sys 763%idle   0%iow   6%irq   2%sirq   0%host
  PID USER         PR  NI VIRT  RES  SHR S[%CPU] %MEM     TIME+ ARGS
 1612 system       18  -2  15G 451M 257M S  7.0   5.9 1682:20.6 system_server
  787 system       -2   0 3.2G  22M  12M S  5.0   0.2 2966:24.2 surfaceflinger
  514 u0_a139      20   0 3.2G 196M 192M S  4.6   2.5   7:17.01 com.UCMobile
16810 shell        20   0 4.3G  29M  29M S  4.3   0.3 492:30.40 apkagent.cli
 1098 system       20   0  87M 1.7M 1.3M S  3.6   0.0 237:31.60 vendor.huawei.h+
 7514 shell        20   0  30M 4.7M 3.3M R  2.6   0.0   0:00.14 top
 7356 u0_a157      20   0 7.3G 164M 151M S  2.6   2.1  48:54.08 com.sohu.inputm+
 3243 system       20   0 6.9G 130M  94M S  1.6   1.7 105:44.14 com.huawei.HwOP+
 2844 u0_a139      20   0 2.0G  99M  86M S  1.0   1.2   0:04.83 com.UCMobile:Do+
 2486 system       20   0 6.5G 129M 101M S  1.0   1.6  58:59.61 com.huawei.syst+
 5023 root         20   0    0    0    0 I  0.6   0.0   0:03.81 [kworker/u16:4]
 1059 root         20   0    0    0    0 I  0.6   0.0   0:06.91 [kworker/u16:1]
 3547 u0_a139      20   0 2.0G 114M  96M S  0.6   1.4   0:11.31 com.UCMobile:ch+
 2917 u0_a139      20   0 2.1G 113M  93M S  0.6   1.4   4:00.37 com.UCMobile:Me+
 2771 system       20   0 6.3G 110M  87M S  0.6   1.4 118:19.66 com.huawei.iawa+
 1567 root         -2   0    0    0    0 S  0.6   0.0 227:06.46 [hisi_hcc]
  692 root         20   0 2.6G 4.7M 2.9M S  0.6   0.0  28:35.16 netd
 6558 root         20   0    0    0    0 I  0.3   0.0   0:00.06 [kworker/5:3]
25705 u0_a365      20   0 2.4G 205M 133M S  0.3   2.6   1:51.42 com.lite.lanxin
24441 u0_a152      30  10 8.2G 245M 183M S  0.3   3.2   1:31.13 com.tencent.mob+
 ...
```

各列含义：

<table data-draft-node="block" data-draft-type="table" data-size="normal" data-row-style="normal"><tbody><tr><td>列名</td><td>含义</td></tr><tr><td>PID</td><td>进程 ID</td></tr><tr><td>PR</td><td>优先级</td></tr><tr><td>CPU%</td><td>当前瞬间占用 CPU 百分比</td></tr><tr><td>S</td><td>进程状态（R=运行，S=睡眠，T=跟踪/停止，Z=僵尸进程）</td></tr><tr><td>#THR</td><td>线程数</td></tr><tr><td>VSS</td><td>Virtual Set Size 虚拟耗用内存（包含共享库占用的内存)</td></tr><tr><td>RSS</td><td>Resident Set Size 实际使用物理内存（包含共享库占用的内存）</td></tr><tr><td>PCY</td><td>调度策略优先级，SP_BACKGROUND/SPFOREGROUND</td></tr><tr><td>UID‍</td><td>进程所有者的用户 ID ‍</td></tr><tr><td>NAME</td><td>进程名</td></tr></tbody></table>

|

\`top\` 命令还支持一些命令行参数，详细用法如下：

  

```text
Usage: top [ -m max_procs ] [ -n iterations ] [ -d delay ] [ -s sort_column ] [ -t ] [ -h ]
    -m num  最多显示多少个进程
    -n num  刷新多少次后退出
    -d num  刷新时间间隔（单位秒，默认值 5）
    -s col  按某列排序（可用 col 值：cpu, vss, rss, thr）
    -t      显示线程信息
    -h      显示帮助文档
```

查看进程 UID

有两种方案：

1\. \`adb shell dumpsys package \<packagename> | grep userId=\`

如：

  

```text
$ adb shell dumpsys package org.mazhuang.guanggoo | grep userId=
      userId=10394
```

2\. 通过 ps 命令找到对应进程的 pid 之后 \`adb shell cat /proc/\<pid>/status | grep Uid\`

如：

  

```text
$ adb shell
   gemini:/ $ ps | grep org.mazhuang.guanggoo
   u0_a394   28635 770   1795812 78736 SyS_epoll_ 0000000000 S org.mazhuang.guanggoo
   gemini:/ $ cat /proc/28635/status | grep Uid
   Uid:    10394   10394   10394   10394
   gemini:/ $
```

其它

如下是其它常用命令的简单描述，前文已经专门讲过的命令不再额外说明：

<table data-draft-node="block" data-draft-type="table" data-size="normal" data-row-style="normal"><tbody><tr><td>命令</td><td>功能</td></tr><tr><td>cat</td><td>显示文件内容</td></tr><tr><td>cd</td><td>切换目录</td></tr><tr><td>chmod</td><td>改变文件的存取模式/访问权</td></tr><tr><td>df</td><td>查看磁盘空间使用情况</td></tr><tr><td>grep</td><td>过滤输出</td></tr><tr><td>kill</td><td>杀死指定 PID 的进程</td></tr><tr><td>ls</td><td>列举目录内容</td></tr><tr><td>mount</td><td>挂载目录的查看和管理</td></tr><tr><td>mv ‍</td><td>查看正在运行的进程‍</td></tr><tr><td>ps</td><td>查看正在运行的进程</td></tr><tr><td>rm</td><td>删除文件</td></tr><tr><td>top</td><td>查看进程的资源占用情况</td></tr></tbody></table>

**常见问题**

启动 adb server 失败

\*\*出错提示\*\*

  

```text
error: protocol fault (couldn't read status): No error
```

\*\*可能原因\*\*

adb server 进程想使用的 5037 端口被占用。

\*\*解决方案\*\*

找到占用 5037 端口的进程，然后终止它。以 Windows 下为例：

  

```text
netstat -ano | findstr LISTENING
```

TCP 0.0.0.0:5037 0.0.0.0:0 LISTENING 1548

这里 1548 即为进程 ID，用命令结束该进程：

  

```text
taskkill /PID 1548
```

然后再启动 adb 就没问题了。

com.android.ddmlib.AdbCommandRejectedException

在 Android Studio 里新建一个模拟器，但是用 adb 一直连接不上，提示：

  

```text
com.android.ddmlib.AdbCommandRejectedException: device unauthorized.
This adb server's $ADB_VENDOR_KEYS is not set
Try 'adb kill-server' if that seems wrong.
Otherwise check for a confirmation dialog on your device.
```

在手机上安装一个终端然后执行 su 提示没有该命令，这不正常。

于是删除该模拟器后重新下载安装一次，这次就正常了。
