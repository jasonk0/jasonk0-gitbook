# Flutter

## 环境配置

### Windows下

电脑配置要求

- **操作系统**：Windows 10 或更高的版本（基于 x86-64 的 64 位操作系统）。
- **磁盘空间**：除安装 IDE 和一些工具之外还应有至少 1.64 GB 的空间。
- **设置**: 必须在 Windows 10/11 上启用开发者模式。

必选项：

- Git

- Power Shell

可选项：（flutter的stable版本自带web）

- Android studio （安卓的开发环境）（且最少是8GB运行内存）

  这其中的配置项、需要下载的其他环境、所需内存、所需配置极其庞杂。不推荐

#### 安装flutter



#### 安装Android Studio

jdk8-（8gb的运行内存）

### Linux

- **操作系统**: Linux (64 位)
- **磁盘空间**: 600MB (不包含安装 IDE 和其他工具的空间)
- **命令工具**: Flutter 需要你的开发环境中已经配置了以下命令行工具。
  - `bash`
  - `curl`
  - `file`
  - `git` 2.x
  - `mkdir`
  - `rm`
  - `unzip`
  - `which`
  - `xz-utils`
  - `zip`
- **公用库**: Flutter 的 `test` 命令需要你的系统安装或存在如下的公用库。
  - `libGLU.so.1` - 由 mesa 套件 (packages) 提供，比如 Ubuntu/Debian 系统下的 `libglu1-mesa`，以及 Fedora 系统下的 `mesa-libGLU`。

可选项：（flutter的stable版本自带web）

- Android studio （安卓的开发环境）（且最少是8GB运行内存）

  这其中的配置项、需要下载的其他环境、所需内存、所需配置极其庞杂。不推荐

- Google Chrome 也是用来测试的

#### 安装`flutter`

找到git库，clone下来；配置环境变量，`vim ~/.zshrc` ，在末尾添加`export PATH="$PATH:/home/jasonk0/.flutter/bin"`，这里可以看图。

这里的第一行便是环境变量，后边两行是配置的`flutter`的国内镜像（上海交大的），下载比较快一些。

![1745dfd85ccc1adbd3da8f220736f8c1.png](https://i.jpg.dog/file/jpg-dog/1745dfd85ccc1adbd3da8f220736f8c1.png)

之后运行`flutter doctor`，你会看到类似如下图所示（因为我已经装了几个了），你可以不用管其他的，如果你不需要Android的测试环境，Chrome也没必要，那就完全可以继续往下走。

> 这里说明一下`Android`环境的前提，你需要具有可视化的`Linux`桌面，而我的`Linux`是`WSL2`下的，需要额外安装`X-Server`、`gonme`；
>
> 且如果你不幸是64位的，你还需要安装一些32位的依赖，[看这里](https://developer.android.google.cn/studio/install?hl=zh-cn#64bit-libs)

![36188a5e48244f985297ec811ff52612.png](https://i.jpg.dog/file/jpg-dog/36188a5e48244f985297ec811ff52612.png)


咱们接着走，假设你已经有了`VScode`，且可以通过它远程连接到`Linux`，那你就已经完成了最最最最最丐版的`flutter`安装。

接下来`flutter create <project-name>`创建一个具备web的项目，

> 或者你可以通过`flutter new application <project-name>`创建一个具备所有测试环境的项目(前提是你有那些环境)

然后`flutter run `跑起来！或者右下角选取一个`devices`（不出意外就一个web）然后`F5`

![4857b61ff45a8e7fe1eaa2f99156f50b.png](https://i.jpg.dog/file/jpg-dog/4857b61ff45a8e7fe1eaa2f99156f50b.png)

反正我没跑起来。。。

---

2022-7-11

1. 尝试配置安装Flutter的运行环境，其中遇到很多问题，有些待解决
2. 尝试了WSL+Linux下的安装后，放弃转windows安装

---

1. 安装配置好windows下的flutter和全测试环境
2. 熟悉项目

## 正经学习




