---
layout: post
title:  "如何使用ADB调试工具打开黑阈等App"
---

# 一、下载platform-tools

platform-tools是android的sdk调试工具，可以在[这里](https://developer.android.google.cn/studio/releases/platform-tools)下载。

# 二、使用platform-tools

打开platform-tools所在的文件夹。在按住shift键，在文件夹的空白处右击。选择`在此处打开命令窗口`。这样就打开了windows的cmd命令行工具。

## （一）黑阈

在打开的命令行工具中输入以下命令。

`.\adb -d shell sh /data/data/me.piebridge.brevent/brevent.sh`

即可自动完成ADB调试。然后黑阈App就可以打开了。

## （二）app ops

`.\adb shell sh /sdcard/Android/data/moe.shizuku.privileged.api/files/start.sh`

app ops是通过该App之外的另一个配套App(Shizuku Manager)来管理ADB调试的。Shizuku Manager同时也可以打开冰箱App，所以下面冰箱的ADB调试不必单独调试。

## （三）冰箱

`.\adb shell dpm set-device-owner com.catchingnow.icebox/.receiver.DPMReceiver`

如果要单独调试，就是用这个命令。

# 三、其他

当然，这里简略了很多步骤，主要有手机要连接电脑，而且手机要打开USB调试，并且保持USB调试一直在打开状态，并且不能调试之后就关闭手机的usb调试开关。建议手机上usb使用模式选择“仅充电”。因为经过我多次尝试发现，手机通过数据线连接电脑的时候，每次都是自动选择通过“仅充电”的方式连接电脑，而不是通过文件传输或者照片传输的方式。但是呢，如果你要将手机上的文件通过数据线传输到电脑上，就要选择传输文件的方式连接电脑，这时候，因为USB使用模式的变化，会使得上面的黑阈等App不能使用，必须重新调试才能使用。
