---
title: "Windows 10 快速配置指南"
date: 2019-01-14 23:11:19 +0800
category: Windows
excerpt: 这是一篇关于 Windows 10 配置的快速指南，因为 Windows 算是重装过最多次的系统了，也算是给自己做个备份，节省一些时间。
---

## 告别

在此先向用了一年多的 Arch 道个别，谢谢你让我体验了 Linux 世界的美好。

![Screenshot-from-2018-12-08-21-01-54.png](https://www.z4a.net/images/2019/01/15/Screenshot-from-2018-12-08-21-01-54.png)

## 系统篇

Windows 10 镜像下载，当然是 [I tell you](https://msdn.itellyou.cn/) 网站了，首选英文版。安装完成后，更新系统及驱动至最新，之后登录微软账号同步一些个性化配置，这里你可能会想修改系统默认的用户名，参见[在知乎上的回答](https://www.zhihu.com/question/51241293/answer/550179994)。

![Snipaste_2019-01-15_18-43-03.png](https://www.z4a.net/images/2019/01/15/Snipaste_2019-01-15_18-43-03.png)

对于桌面的个性化，个人不喜欢桌面留任何快捷方式图标，回收站放在开始页面的磁贴里，任务栏自动隐藏，时钟的话，使用 Rainmeter 写个插件就行了。Windows 上字体可能是吐槽的比较多的一点(MacType 试了一下，没有理想中的效果就作罢了)，因为很喜欢 Monaco 字体，这个字体也当然是必装的啦，在 Github 上有几个仓库可以参考一下 (推荐安装 [Monaco](https://github.com/cstrap/monaco-font) 和 [HeyMona](https://github.com/0xHJK/HeyMo))。其实想想，换个高清屏电脑，Windows 的体验也不至于太差，谁让 MacBook 贵还不好说它坏话呢 🙂 。

清除桌面上的回收站快捷方式如下图所示：

![Snipaste_2019-01-15_19-50-42.png](https://www.z4a.net/images/2019/01/15/Snipaste_2019-01-15_19-50-42.png)

安装 Mac 风格的鼠标光标主题，这个网上还是挺多的，综合试用后，推荐 [Capitaine](https://github.com/keeferrourke/capitaine-cursors) 这款，因为其他的文本光标是全黑没有白边，一碰到黑色背景，你就不知道你在玩啥了 🙃 。下载后右键安装就完事了。

![Snipaste_2019-01-16_13-44-15.png](https://www.z4a.net/images/2019/01/16/Snipaste_2019-01-16_13-44-15.png)

之后在主题设置中选择这款鼠标指针主题，我好了，你呢？

![Snipaste_2019-01-16_13-14-35.png](https://www.z4a.net/images/2019/01/16/Snipaste_2019-01-16_13-14-35.png)

## 软件篇

软件清单：[7-Zip]、[Snipaste]、[Listary]、[Rainmeter]、[Chocolatey]、[ConEmu]、[Atom] 或 [SublimeText]、[Chrome]、[WPS 专业版]、Adobe 系列、[JetBrains 系列]、[TIM]、[WeChat]、[Tg]、[网易云音乐]等。

其实软件这东西，除了常用的外，也是想到啥装啥呗 😑 。下面挑几个需要稍微做点工作的软件工具。

### Rainmeter

[Rainmeter] 是一个 Windows 下的桌面插件工具，给予用户如 Android 桌面插件的体验，为了满足自己的需求，在自带插件例子下，修改编写了一个极简时钟插件。

直接新建一个主题，然后将文件复制粘贴过去即可，主题文件夹位于：

```
C:\Users\[username]\Documents\Rainmeter\Skins\
```

主题代码贴到 [Gist](https://gist.github.com/zoeywoohoo/618974a4a7a9d3ff7deb5c40cd3da587) 上了，位置及其它设置可自行调整。

![Snipaste_2019-01-16_12-25-46.png](https://www.z4a.net/images/2019/01/16/Snipaste_2019-01-16_12-25-46.png)

### Chocolatey

使用 [Chocolatey] 进行软件包管理，纵享丝滑，有着如 Linux/macOS 下软件包管理的体验，尤其是安装一些工具时，**安装完成后自动配置环境变量**。一些基本的你想得到的软件包，都已经支持了，详情可参见 [包列表](https://chocolatey.org/packages)。

例如安装 Git、Vim、JDK 等，只需一行命令，且无需手动设置环境变量，也算是解决了一些 Windows 被诟病的问题。

```
$ choco install git
$ choco install vim
$ choco install jdk8
```

补一张安装 ffmpeg 的截图：

![Snipaste_2019-01-19_14-08-43.png](https://www.z4a.net/images/2019/01/19/Snipaste_2019-01-19_14-08-43.png)

### ConEmu

使用 [ConEmu]，还原最优的终端体验，说到终端，在 Windows 下使用 PowerShell 而不是 Cmd (因为 Cmd 是旧时代的遗留物)。提到 ConEmu 和 PowerShell，我又想起了 [oh-my-posh](https://github.com/JanDeDobbeleer/oh-my-posh)，这个在 ConEmu 里为 PowerShell 提供的主题引擎。

README 文档里展示的主题还是挺漂亮的，就是我下载了那个字体后，出来的并不是它展示的那个效果，也不知道是我搞错字体了还是作者搞错了。那就换我最心爱的 Monaco 字体吧 (ConEmu 设置里一把梭)，当然，还不能不考虑到中文字体，所以就选择 HeyMona 吧。但是，这时候发现，主题里炫酷的效果没了，很多字符这个字体根本不支持。正所谓「两权相害取其轻」，干脆自己写个主题算了，稍微研究了一下官方给的主题，摸清了这个语法套路，写了个极简主题 [Minimalism](https://gist.github.com/zoeywoohoo/c2d7265fc4114bb33b0a83b222dc17a9)。

这个主题大概长这样：

![Snipaste_2019-01-16_17-35-42.png](https://www.z4a.net/images/2019/01/16/Snipaste_2019-01-16_17-35-42.png)

管理员权限时：

![Snipaste_2019-01-16_17-31-57.png](https://www.z4a.net/images/2019/01/16/Snipaste_2019-01-16_17-31-57.png)

如果你也想配置成这个样子 😐 ，可以按下面步骤配置。

调整了配色，修改后的 ConEmu 配置文件在 [Gist](https://gist.github.com/zoeywoohoo/cd4f8e3f4d0225fb41ba556d70d4f2e5) 上贴出，其文件默认位于：

```
C:\Users\[username]\AppData\Roaming\ConEmu.xml
```

将其内容复制粘贴覆盖即可，你也可以根据自己需求改动，而 oh-my-posh 的主题目录默认在：

```
C:\Users\[username]\Documents\WindowsPowerShell\PoshThemes\
```

下载主题文件，放置在这个目录下，一行命令即可设置成功 (但是只是当前窗口生效，想要启动时自动切换到这个主题，需要写在 PowerShell 的配置文件中，详细见下文)：

```
$ Set-Theme Minimalism
```

主题可以根据自己的需求修改，其中发现了一个网站，一并注释在主题文件中，Enjoy yourself！

那么现在插播一下 PowerShell 的配置文件，相当于 Bash 下的 .bashrc，默认位置位于：

```
C:\Users\[username]\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1
```

自己添加了两个命令，方便修改 PowerShell 和 Vim 的配置：

```powershell
Import-Module posh-git
Import-Module oh-my-posh
Set-Theme Minimalism

Set-Alias vi vim

# for editing PowerShell profile
Function Edit-Profile
{
    vim $profile
}

# for editing Vim settings
Function Edit-Vimrc
{
    vim $home\_vimrc
}
```

> 新增了一个类似 zsh 默认样式效果的主题 [zshStyle](https://gist.github.com/zoeywoohoo/3a1df1251c26f5a733eb1f49afecaa11)，效果如下图所示，我已经用上了，你呢？

![Snipaste_2019-01-17_18-10-09.png](https://www.z4a.net/images/2019/01/17/Snipaste_2019-01-17_18-10-09.png)

### WPS 专业版

[WPS 专业版] 相比普通的 WPS 版本，好在哪里呢？谁用谁知道 🙄 ，至于那啥 🤐 ，百度一下就有了。

> 以上提及的一些资源一并上传到 [GitHub 仓库](https://github.com/zoeywoohoo/Windows10-configuration-resources) 中方便下载使用。

[7-Zip]: https://www.7-zip.org/
[Snipaste]: https://www.snipaste.com/
[Listary]: https://www.listary.com/
[Rainmeter]: https://www.rainmeter.net/
[ConEmu]: https://conemu.github.io/
[Chocolatey]: https://chocolatey.org/
[Chrome]: https://www.google.com/chrome/
[Atom]: https://atom.io/
[SublimeText]: https://www.sublimetext.com/
[WPS 专业版]: http://ep.wps.cn/product/wps-office-download.html
[JetBrains 系列]: https://www.jetbrains.com/
[TIM]: https://tim.qq.com/
[WeChat]: https://pc.weixin.qq.com/
[Tg]: https://telegram.org/
[网易云音乐]: https://music.163.com/#/download
