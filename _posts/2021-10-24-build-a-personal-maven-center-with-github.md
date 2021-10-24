---
title: 利用 GitHub 搭建个人私有 Maven 仓库
date: 2021-10-24 14:21:47 +0800
category: Android
tags: [Maven, GitHub]
excerpt: 基于 GitHub 仓库文件可直接引用 raw 链接下载的特性，打造一个私人的 Maven 仓库
reward: true
---

## 起因

之前 Google Cloud 白嫖的服务器到期了，导致自己部署的 Maven 仓库也用不了了，想搭一个私有的 Maven 仓库，又不想买服务器，怎么办呢？想到 GitHub 能不能办成这件事，搜索之后发现 GitHub 已经提供了一个 GitHub Packages 的功能，同时[支持 Maven 仓库](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-apache-maven-registry)，不过这项服务只有对公开的仓库是全免费的，私人的仓库则是有额度限免，具体的收费标准可查看[官方链接](https://docs.github.com/en/billing/managing-billing-for-github-packages/about-billing-for-github-packages)。不过对于个人使用，好像已经绰绰有余了？但是担心后续付费规则的修订，这里选择了另一种方式曲线救国。

## 原理

当我们项目中依赖一个三方库时，需要从 Maven 仓库拉取下载，而最关键的一步则是 POM 文件，依赖于 POM 文件的读取，解析并下载对应结构路径下的包，从而集成进我们的项目中。而如 Nexus 提供的服务，只不过是帮你在服务器上跑通这一流程，提供依赖包的发布存储及拉取下载。

既然是这样就好办了，我们都知道，GitHub 是可以引用一个 raw 链接直接下载文件的:

```text
https://raw.githubusercontent.com/<Username>/<RepoName>/<BranchName>/<PathToFile>
```

具体的规则是: `用户名/仓库名/分支名/文件路径`，而且只能引用文件，不能引用文件夹 (否则 404)。那岂不是可以直接把 Maven 发布生成的包文件直接放到仓库中，然后用 raw 链接作为 Maven 仓库地址，这样就可以了。经过一番试验，最终也证明确实可行，下面则是搭建的流程记录。

## 流程

### 新建一个私人仓库

在 GitHub 上新建一个私人仓库，用于统一存放我们生成发布的三方库包，不同于一个仓库放一个三方库包，统一存放到一个仓库里，在项目中只需引入一个 Maven 仓库地址即可。下面将以我的仓库 Barn 为例。

### 本地发布 release 包

这里我假设你已经有了一个待发布的三方库，并懂得怎么利用 maven 插件进行发布，如果还不了解相关知识的话，可以参考 Google 的[官方文档](https://developer.android.com/studio/build/maven-publish-plugin)及其他资料。这里唯一特殊的是，我们配置 Maven 仓库地址时，引用的是本地的路径，对应于上面新建的私有仓库本地文件地址。

```gradle
publishing {
    repositories {
        maven {
            // 这里更改为你自己的本地仓库路径，对应于 GitHub 上的仓库
            url "/Users/ZoeyWoohoo/NeoStudio/Barn/maven"
        }
    }
}
```

下面就是生成发布包了，我们在 Android Studio 的 Gradle 任务下找到 publish 任务运行即可，或者直接命令行运行：

```shell
./gradlew publish
```

运行结束后，可以在文件夹下看到生成的包，结构大致如下：

```text
➜ pwd
/Users/ZoeyWoohoo/NeoStudio/Barn

➜ tree ./
./
├── maven
    └── com
        └── neostudio
            └── neotoast
                └── neotoast
                    ├── 1.0
                    │   ├── neotoast-1.0-sources.jar
                    │   ├── neotoast-1.0-sources.jar.md5
                    │   ├── neotoast-1.0-sources.jar.sha1
                    │   ├── neotoast-1.0-sources.jar.sha256
                    │   ├── neotoast-1.0-sources.jar.sha512
                    │   ├── neotoast-1.0.aar
                    │   ├── neotoast-1.0.aar.md5
                    │   ├── neotoast-1.0.aar.sha1
                    │   ├── neotoast-1.0.aar.sha256
                    │   ├── neotoast-1.0.aar.sha512
                    │   ├── neotoast-1.0.module
                    │   ├── neotoast-1.0.module.md5
                    │   ├── neotoast-1.0.module.sha1
                    │   ├── neotoast-1.0.module.sha256
                    │   ├── neotoast-1.0.module.sha512
                    │   ├── neotoast-1.0.pom
                    │   ├── neotoast-1.0.pom.md5
                    │   ├── neotoast-1.0.pom.sha1
                    │   ├── neotoast-1.0.pom.sha256
                    │   └── neotoast-1.0.pom.sha512
                    ├── maven-metadata.xml
                    ├── maven-metadata.xml.md5
                    ├── maven-metadata.xml.sha1
                    ├── maven-metadata.xml.sha256
                    └── maven-metadata.xml.sha512

5 directories, 25 files
```

### 将发布的包推送到仓库中

万事俱备只欠推送，进入仓库文件夹地址，初始化 git，配置远程仓库地址，创建提交。当然，为了政治正确，改一下默认的分支名 master 为 main 吧 (:dog:)，然后将其推到 GitHub 远程仓库中就行了。

```text
➜ cd /Users/ZoeyWoohoo/NeoStudio/Barn
➜ git init
➜ git add *
➜ git commit -m "init maven"

// 这里修改为你自己的 GitHub 仓库地址
➜ git remote add origin git@github.com:ZoeyWoohoo/Barn.git

// 需要注意本地生成提交后 master 分支才会生效，此前修改分支名会报错
➜ git branch -m master main

➜ git push origin main -u
```

### 项目中引用自己的三方库

现在我们就可以在项目中引用自己的三方库包了，不过在此之前，还需做一件事。公开的仓库是可以直接拉取的，但是私人仓库就需要身份认证了，直接用账号密码肯定是不推荐的，这里我们需要生成一个 GitHub 的 Personal Access Token，[点击这里](https://github.com/settings/tokens)可以直接跳转设置，其设置位置在于：

```text
Settings -> Developer settings -> Personal access tokens 
```

新建一个 Token，有效期按自己需要设置一个时长，GitHub 官方不建议设为永久，这里笔者选了个自定义一年时间，权限只需勾选 repo 项就可以了，然后点击生成，并将其复制下来备份一下，因为为了安全，之后 GitHub 将不允许在这个页面上查看 Token 了。

然后在项目的 build.gradle 文件中配置 maven 仓库地址：

```gradle
buildscript {
    // ...
    repositories {
        google()
        // ...

        maven {
            // 填写你自己的仓库地址
            url "https://raw.githubusercontent.com/ZoeyWoohoo/Barn/main/maven/"
            credentials(HttpHeaderCredentials) {
                name "Authorization"
                // 填写你获取到的 Token
                value "Bearer <Your Personal Access Token>"
            }
            authentication {
                header(HttpHeaderAuthentication)
            }
        }
    }
}

allprojects {
    // ...
    repositories {
        google()
        // ...

        maven {
            // 填写你自己的仓库地址
            url "https://raw.githubusercontent.com/ZoeyWoohoo/Barn/main/maven/"
            credentials(HttpHeaderCredentials) {
                name "Authorization"
                // 填写你获取到的 Token
                value "Bearer <Your Personal Access Token>"
            }
            authentication {
                header(HttpHeaderAuthentication)
            }
        }
    }
}
```

不过安全起见，万一哪天你不小心把代码推到公开仓库，这个 token 就泄漏了，所以我们还是将其配置在本地吧：

```text
// 修改 gradle.properties 文件
➜ vim ~/.gradle/gradle.properties

// 在其中加入下面这行配置，填入自己的 token
github.personal.access.token=<Your Personal Access Token>
```

而引入仓库地址时，则对应的修改为：

```gradle
// ...
maven {
    // ...
    credentials(HttpHeaderCredentials) {
        name "Authorization"
        value "Bearer " + project.findProperty("github.personal.access.token")
    }
    // ...
}
```

在 module 下 build.gradle 文件中正常地引入包依赖即可：

```gradle
dependencies {
    // ...
    implementation 'com.neostudio.neotoast:neotoast:1.0'
}
```

最后的最后 (Last but not least)，项目 gradle sync 的时候记得需要代理，因为库已经托管在 GitHub 上了，因为这个问题浪费了我一晚上的时间 :)