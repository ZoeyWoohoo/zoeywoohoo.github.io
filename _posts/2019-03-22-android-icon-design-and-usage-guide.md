---
title: Android 图标设计 & 使用指南
date: 2019-03-22 04:31:51 +0800
category: Design
tags: [Icon, Android]
excerpt: 这是一篇关于 Android 图标设计规范和使用的文章，包括了传统图标 (Legacy launcher icons) 以及最新的自适应图标 (Adaptive icons)，以供一些个人开发者做一些参考。(本文图片资源较多，请保证网络畅通)
---

## 前言

图标分几种，包括产品图标、系统图标、应用快捷图标、动效图标等 (当一个人说到等字的时候，就是他列举不下去了)，本文要讲的则是其中的产品图标 (Product icons)，也是我们常说的应用图标或启动图标。

而说到官方设计就不得不提到 Material Design 了，近几年不管是设计还是开发，Google 都更新的很快，图标的设计也不例外。从拟物化变为扁平，再到 Material Design 的推出，这种质感设计也逐渐流行开来，这段时期的图标设计，在 Android 上现在被称之为传统启动图标 (Legacy launcher icons) 设计。

这里还要插一句，即使是 Material Design 时期的图标设计，早期的时候依然没有完全摆脱拟物化时期的风格影响，可能主要还是在用色方面上，后期的用色鲜艳凸显活力，更多的是汲取了扁平化设计 (Flat Design) 的优点。

这个时期，大家都力图追求独特的外形，与众不同的设计能够让自己的应用有足够高的辨识度，让自己的图标在一众图标中脱颖而出。如下拿 [Roman] 的 [Plaid] 作品中的一个图标来说：

![DistinctShapes.png](https://www.z4a.net/images/2019/03/22/DistinctShapes.png)

在也正如同拟物化设计时期一样，这种开放自由式的设计，在后期，各个应用的图标为了提高辨识度，以牺牲用户体验为代价，让自己的图标尽可能显得独特，这使得图标界面整体**失去了统一性**。(下图：理想 VS 现实)

![IdeaVSReality.png](https://www.z4a.net/images/2019/03/22/IdeaVSReality.png)

也正如 [Nick Butcher] 所说：

> When everything is unique, nothing is unique.
>
> (当一切都变得独特的时候，也就没有了独特)

这个时候，想要让各个应用自觉地在设计上以用户体验为主几乎是不可能了，所以各个 OEM (原始设备制造商) 为了让自己的「系统」界面更加统一以提高自家用户体验，开始限制桌面启动图的显示，包括缩放填充，以及强制性的统一外形背景包裹。这个相信在座的各位都深有体会，这种图标背景多以圆角矩形和圆形为主。以下拿三星 Galaxy S7 为例 ([图源])：

![iconWithBg.jpg](https://www.z4a.net/images/2019/03/22/iconWithBg.jpg)

而 Google 也曾在 Android 7.1 的时候介绍过 RoundIcon，以试图获得这种图标的统一性，但是在 OEM 的各种限制下，效果不是很好。直至 Android O (8.0 API 26)，Google 终于出手了，推出了自适应图标 (Adaptive Icon)。

自适应图标在设计层面来说，主要由三个概念构成，即：**背景、前景和遮罩**。背景就是整个图标的背景，多以纯色填充背景为主；前景就是你的图标设计，其中设计时会有一个安全区域，图标设计时，大小最好限制在安全区域内，已防止遮罩裁切后显示不全或大小比例位置失调；遮罩就是各个 OEM 桌面图标显示形状的一个「裁剪区域」。用一张动图来解释就很明了了：

![adaptiveIconDesign.gif](https://www.z4a.net/images/2019/03/22/adaptiveIconDesign.gif)

不同的手机设备厂商会采用不同的遮罩，自适应图标会根据其遮罩自动显示相应的效果：

![IconMaskEffect.gif](https://www.z4a.net/images/2019/03/23/IconMaskEffect.gif)

而从效果层面来看，这种图标设计之所以被称之为自适应，除了在不同形状遮罩下，都能获得一致的体验外，在图标的动效方面，使得图标变得更加灵动，因为原生的 Android 桌面加入了手势响应的各种动效设计，包括了点击及各方向的拉拽的弹性效果。同样的，一图胜千言：

![adaptiveIconEffect.gif](https://www.z4a.net/images/2019/03/22/adaptiveIconEffect.gif)

但是一码归一码，国内环境大家都清楚，目前自用小米手机默认的遮罩就是圆角矩形，可以的话还是用 Nova Launcher 吧，至少能够获得和原生近乎一致的体验，还能够自定义遮罩形状。(下图为左小米，中 Nova Launcher，右自定义遮罩形状设置)

![IconNow.png](https://www.z4a.net/images/2019/03/22/IconNow.png)

不过好像直到现在，Google 自家的好些应用图标也没遵循这些规范，大概是各个团队都太忙了吧 :) 虽然我也挺喜欢一些如 CandyCons 图标包里的图标设计，但这些居多都还是未遵循这种设计规范，希望未来会有越来越多的应用能够采用这种自适应图标设计，我相信一个好的设计师，应该不会被其「束缚」住吧 :p

## 设计规范

随着 Material Design 的更新，文档也随之迭代更新，老的文档会被归档，而有的时候还是需要老的文档做一些信息补充，你可以通过这两个链接访问：[老的设计规范]、[新的设计规范]。

因为 Android 8.0 以上才开始支持 Adaptive icons，而我们不可能最低才兼容到 8.0，最起码 minSDK 21 (Android 5.0) 还是要支持的。所以在设计的时候，**整体设计以自适应设计为主**，依据两套规范输出两套图标，在 8.0 以上的手机使用自适应图标，以下则使用传统图标。

如果有条件，推荐观看 YouTube 上的一个教学视频以跳过下文设计规范部分：[(YouTube) Design Adaptive Icons using Google Material Design Guidelines - Illustrator CC 2018 Tutorial]

下面是收集整理的一些参考线模板资源：

| 设计工具 | 传统图标参考线模板下载地址 | 自适应图标参考线模板下载地址  |
| -- | -- | -- |
| Adobe Illustrator | [AI Legacy] | [AI Adaptive] |
| Sketch | @null | [Sketch Adaptive] |
| Figma | @null | [Figma Adaptive] |
| Affinity Designer | @null | [Affinity Designer Adaptive] |
| Adobe XD | @null | [Adobe XD Adaptive] |

### 传统图标

传统图标设计的网格及参考线如下所示，以 AI 中为例，创建画板宽高为 192px，参考形状分四种，宽高均为 152px 的圆角矩形、宽高为 176px 和 128px 的水平或垂直圆角矩形、直径为 176px 的圆形，其中圆角矩形的圆角为 12px，同时底层有 4px 大小的网格线。根据参考形状与图标几何形状对比做出选择，并在规定区域内进行图标的设计。如下图所示：

![gridsAndKeylinebc1ef484b8cf7901.png](https://www.z4a.net/images/2019/03/22/gridsAndKeylinebc1ef484b8cf7901.png)

一般地，我们选择圆角正方形及圆形输出一套标准的传统图标，圆角正方形在应用里为正常图标 (icon)，圆形在应用里为圆形图标 (round icon)，在应用中进行相应的设置即可。

而根据规范，图标结构分以下 5 层：

![style-logos-product-anatomy-components-perspective.png](https://www.z4a.net/images/2019/03/23/style-logos-product-anatomy-components-perspective.png)

> 1: Finish (一个最后的遮罩层，与自适应图标里的遮罩不是同一个概念)
>
> 2: Material background (背景层)
>
> 3: Material foreground (前景层)
>
> 4: Color (色块层，可以理解为 logo 上某一元素)
>
> 5: Shadow (阴影层)

**下面我们依据自适应图标设计风格来进行设计**，以在 AI 上为例，首先选择一个参考形状圆角正方形，进行一个背景色填充，再给其加上一个 Drop Shadow (或者可能称其为 Contact Shadow)，参数如下：

![backgroundDesign.png](https://www.z4a.net/images/2019/03/23/backgroundDesign.png)

这个阴影源自 Material Design 中一个正上方左上角的光源投影，阴影比较柔和。下面是图标正体设计，也是图标的前景，在此之后，Material Design 为了体现出材质的厚度感 (把你的图标想象成是一张纸)，为其增加了一个顶部的着色边缘和一个底部的阴影边缘，如下图 Hangouts 图标效果所示：

![Hangouts.png](https://www.z4a.net/images/2019/03/23/Hangouts.png)

**注意这里环聊使用的还是传统图标设计的风格**，虽然它也提供了自适应图标，但这两种风格有种不协调感，可能也是因为 Android 8.0 以下本身就不支持自适应图标这个原因，Google 觉得对于低版本，还要啥自行车。下图左边为魅族 MX4 Android 5.0 的手机，右边为小米 6 Android 8.0， 左边图标采用一致性的自适应设计风格，右边环聊图标在 8.0 以下采用的是传统设计风格 (自行感受一下，个人还是推荐采用一致的自适应设计风格)：  

![HangoutsDiff.png](https://www.z4a.net/images/2019/03/23/HangoutsDiff.png)

图标前景设计好后，还可以为其加一个经典的长投影，以其图标中心为起点，右下方 45° 角径向渐变投影，如下图所示：

![longShadow.png](https://www.z4a.net/images/2019/03/23/longShadow.png)

这个投影可以用钢笔工具勾勒出来，然后设置渐变填充，用以模仿投影效果，其渐变各点处参数如下：

| AI 渐变工具 | 位置 | 透明度 |
| -- | -- | -- |
| 滑块 1 | 32% | 15% |
| 滑块 2 | 62% | 2% |
| 滑块 3 | 100% | 0% |

但是我发现加完后效果根本不行，所以直接用填充加透明度 10% 来替代 (我也只能安慰自己，规范也仅仅是规范，不是死的，我们要灵活变通)：

![IconLongShadow.png](https://www.z4a.net/images/2019/03/23/IconLongShadow.png)

最后是一个 finish 图层，它是一个白色透明渐变图层作为遮罩至于前景上方，也被称为精加工层 🤨。与长投影类似它也是一个 45° 角的径向渐变，其中间点在 33% 的位置：

![finishLayer.png](https://www.z4a.net/images/2019/03/23/finishLayer.png)

其渐变各点处参数如下：

| AI 渐变工具 | 位置 | 透明度 |
| -- | -- | -- |
| 滑块 1 | 0% | 10% |
| 滑块 2 | 100% | 0% |

这个精加工层可能只有隐藏和查看之前切换你才能看出那细微的差别，毕竟是精加工嘛 :p

最后就是导出操作了，如下图所示先点击 Android 按钮用以输出一套 Android 下的图标文件 (SVG 格式可以取消勾选)，修改缩放为指定宽度，文件名后缀和前缀都去除，输出完后，修改背景图层为圆形，同样地再次输出一套图标图片，**这里记得改默认文件名，后面加上 round**，这样我们就得到了一套完整地传统图标资源。

![exportSetting.png](https://www.z4a.net/images/2019/03/23/exportSetting.png)

而其中不同尺寸对应的 mipmap 资源文件夹如下表所示：

| 尺寸 | 文件夹 |
| -- | -- |
| 48px | mipmap-mdpi |
| 72px | mipmap-hdpi |
| 96px | mipmap-xhdpi |
| 144px | mipmap-xxhdpi |
| 192px | mipmap-xxxhdpi |

> 或者其实这里的导出可以省略，后面我们利用导出的自适应图标 SVG 文件，生成矢量文件后，再利用 Android Studio 的 Asset Studio 自动生成两套图标 (推荐做法)，具体请参考下文一键生成部分。

### 自适应图标

因为秉承着自适应图标设计风格，有了之前的传统图标设计，自适应图标就比较简单了，直接拿过来修改一下。自适应图标画板长宽为 108px，其中一个直径为 72px 的最大边界圆为内容限制区域，也是遮罩剪裁区域，图标一定不能超出这个范围。四周保留的 18px 空白是为了给图标的一些动效所留出的空间，比如拉扯的回弹。也正是因为其动效特征，故其又有了一个图标的安全区域，为 66px 直径的圆，图标大小不超过安全区域能最大程度的保证你的图标在动效过程中不会脱离视野边界。

具体的操作就是把背景层复制粘贴过来，去除阴影效果，填充满整个画板，再将前景内容 (包括长投影) 复制粘贴过来，调整位置和大小 (主要是要在安全区域内)，如下图所示：

![adaptiveIcon.png](https://www.z4a.net/images/2019/03/23/adaptiveIcon.png)

做完这些工作后，我们可以利用 [Marius Claret] (也是 Google Design) 制作的在线预览工具检查体验一下图标是否合理，并作出相应修改，地址为 [Adapticon]，使用方法如下 (你需要先导出一份前、背景图片资源)：

![adapticonTool.gif](https://www.z4a.net/images/2019/03/23/adapticonTool.gif)

最后，觉得图标已经没有问题后，**只要将背景层和前景层分别导出为 SVG 格式文件就可以了**。之后我们可以利用 Android Studio 的 Asset Studio 工具将 SVG 文件转化为矢量图标资源文件，这种方式不仅比直接输出图片资源高效，而且文件更小，矢量图也具有不失真的特点。我们右键 drawable -> new -> Vector Asset 之后，如下图所示导入之前输出的 SVG 文件进行转换就可以了：

![adaptiveExport.png](https://www.z4a.net/images/2019/03/23/adaptiveExport.png)

将前、背景分别转换完成后，设计工作就结束啦！

## 图标使用

### 传统图标

现在我们已经得到了一套完整的传统图标资源，如下图所示 (暂时忽略自适应图标的两个 xml 文件，那个是下面将要创建的)：

![mipmap.png](https://www.z4a.net/images/2019/03/23/mipmap.png)

在 AndroidManifest 文件中设置传统图标，即 icon 及 roundIcon 项：

``` xml
<application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">

        <activity
            android:name=".MainActivity"
            android:theme="@style/AppTheme.SplashScreen">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
</application>
```

### 自适应图标

**自适应图标需要编译版本在 26 以上**，而自适应图标的资源类型是新的独有的一种资源类型，被称之为 AdaptiveIconDrawable ，whatever，前面我们已经得到了由 SVG 转成矢量图的前景和背景资源，位于 drawable 目录下，例如：

``` text
- drawable
  - xxxxxx
  - ic_launcher_background.xml
  - ic_launcher_foreground.xml
```

接着在 mipmap-anydpi-v26 文件夹下创建两个图标资源文件，以同样的命名方式，只不过由图片格式变成了 xml 格式，即之前提到的这两个文件：

![mipmap.png](https://www.z4a.net/images/2019/03/23/mipmap.png)

两个文件相同，内容如下：

``` xml
<?xml version="1.0" encoding="utf-8"?>
<adaptive-icon xmlns:android="http://schemas.android.com/apk/res/android">
    <background android:drawable="@drawable/ic_launcher_background"/>
    <foreground android:drawable="@drawable/ic_launcher_foreground"/>
</adaptive-icon>
```

### Asset Studio 一键生成

我们可以利用 Android Studio 的 Asset Studio 工具一键生成两套图标并直接使用，非常方便，利用之前转换来的矢量文件资源：

``` text
- drawable
  - xxxxxx
  - ic_launcher_background.xml
  - ic_launcher_foreground.xml
```

具体操作如下，右键 mipmap 文件 new -> Image Asset：

![generateQuick.png](https://www.z4a.net/images/2019/03/23/generateQuick.png)

图标类型选择传统和自适应两套，然后再将下面的前景层和后景层替换成上面我们转换来的矢量文件，这里有个小技巧，因为打开时默认位置是 AS 内置默认图标资源的位置，这里点击项目路径按钮能快速切换到项目路径：

![generateQuickTrick.png](https://www.z4a.net/images/2019/03/23/generateQuickTrick.png)

之后直接生成替换，你会发现，这样就完成了两套图标资源的生成，与上面单独操作生成的效果是一样的，方面又快捷 :)

## 参考文档

[Material Design - icons (archive)](https://material.io/archive/guidelines/style/icons.html#)

[Material Design - Product icons](https://material.io/design/iconography/product-icons.html#)

[Material Design - Keyline shapes](https://material.io/design/platform-guidance/android-icons.html#keyline-shapes)

[Developer guides - Adaptive icons](https://developer.android.com/guide/practices/ui_guidelines/icon_design_adaptive)

[Understanding Android Adaptive Icons](https://medium.com/google-design/understanding-android-adaptive-icons-cee8a9de93e2)

[Designing Adaptive Icons](https://medium.com/google-design/designing-adaptive-icons-515af294c783)

[Implementing Adaptive Icons](https://medium.com/androiddevelopers/implementing-adaptive-icons-1e4d1795470e)

[What Google missed in their guidelines for Material Design iconography](https://stories.uplabs.com/what-google-missed-in-their-guidelines-for-material-design-iconography-daf9f88000ec)

[(YouTube) Design Adaptive Icons using Google Material Design Guidelines - Illustrator CC 2018 Tutorial]

[Roman]: https://medium.com/@romannurik
[Plaid]: https://github.com/nickbutcher/plaid/
[Nick Butcher]: https://medium.com/@crafty
[图源]: https://www.cnet.com/how-to/how-to-change-to-rounded-icons-on-the-galaxy-s7-s7-edge/
[老的设计规范]: https://material.io/archive/guidelines/style/icons.html#
[新的设计规范]: https://material.io/design/iconography/product-icons.html#
[(YouTube) Design Adaptive Icons using Google Material Design Guidelines - Illustrator CC 2018 Tutorial]: https://www.youtube.com/watch?v=tidxhJNUPSE&t=187s
[AI Legacy]: https://github.com/zoeywoohoo/Android-icon-keyline-resources/raw/master/Legacy%20Icon%20keyline/LegacyIconKeyline.ai
[AI Adaptive]: https://github.com/zoeywoohoo/Android-icon-keyline-resources/raw/master/Adaptive%20Icon%20keyline/AdaptiveIconKeyline(AI).ai
[Sketch Adaptive]: https://github.com/zoeywoohoo/Android-icon-keyline-resources/raw/master/Adaptive%20Icon%20keyline/AdaptiveIconKeyline(Sketch).sketch
[Figma Adaptive]: https://github.com/zoeywoohoo/Android-icon-keyline-resources/raw/master/Adaptive%20Icon%20keyline/AdaptiveIconKeyline(Figma).fig
[Affinity Designer Adaptive]: https://cyrilmottier.com/2017/07/06/adaptive-icon-template/
[Adobe XD Adaptive]: https://github.com/faizmalkani/adaptive-icon-template-xd/raw/master/adaptive_icon_template_xd.xd
[Marius Claret]: https://twitter.com/mariusclaret
[Adapticon]: https://adapticon.tooo.io/
