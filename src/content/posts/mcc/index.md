---
title: 记使用MCC部署Minecraft进行挂机和遇到的一些问题
published: 2024-09-10
description: 部署并使用mcc进行在游戏Minecraft中的挂机和一些操作，使得游戏进行区块加载和随机刻计算
tags: [MCC, Minecraft,linux,ssh,vim,ini,gt,tick,vps]
category: Linux
draft: false
---

# 引言

随着9月份的到来，我变得基本无法在我所在的服务器上线了，为了让机器保持加载和部分作物的生长以及刷怪，也为了在将来能够给自己的(众所周知在Minecraft中刷怪和随机刻需要玩家，使用加载器无效)，遂在Github进行查询，准备在我的上海服务器上部署一个mcc
但是有些难受的是，我在部署时遇到了一些困难却无法查询到中文教程，于是在克服这些困难后写下本篇

本文为在Linux环境下部署mcc的教程，即中国大陆上海Linux服务器，实例系统为debian-11

---

# Main

### 资源链接与参考文档

::github{repo="<MCCTeam/Minecraft-Console-Client>"}

[MCC官方文档](https://mccteam.github.io/ "Minecraft Console Client MCC is a lightweight open-source Minecraft Java client implemented in C#")

[MCC-Github地址](https://github.com/MCCTeam/Minecraft-Console-Client "Minecraft Console Client (MCC) is a lightweight cross-platform open-source Minecraft TUI client for Java edition that allows you to connect to any Minecraft Java server, send commands and receive text messages in a fast and easy way without having to open the main Minecraft game.")

[WindTerm-SSH工具](https://github.com/kingToolbox/WindTerm "A Quicker and better SSH/Telnet/Serial/Shell/Sftp client for DevOps.")  [如何使用Windterm](https://wiki.deepin.org/zh/04_%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98FAQ/%E5%BC%80%E6%BA%90%E5%85%8D%E8%B4%B9%E7%9A%84%E7%BB%88%E7%AB%AF%E5%B7%A5%E5%85%B7WindTerm%E7%9A%84%E4%BD%BF%E7%94%A8%E4%BB%8B%E7%BB%8D "windterm") (如果您熟练使用其他ssh工具，那还请用熟悉的)

[MCS管理器](https://mcsmanager.com/ "Free, Secure, Distributed, Modern Control Panel for Minecraft and Steam Game Servers.")(可选，用于在移动端时进行便捷管理，不安装同样可用)

### 准备工作

#### 使用ssh工具连接至远程服务器并切换至root用户
具体教程链接已经在上文[资源链接与参考文档](###资源链接与参考文档)中给出
~~(顺带说一下，如果这个都不会的话，这样的项目建议还是洗洗睡吧)~~


#### 安装 `.NET Core 7`
:::note
 提示:如果您的 VPS 使用 ARM 处理器，请按照 [这个](https://mccteam.github.io/l10n/zh-Hans/guide/installation.html#installing-net-on-arm)部分文档进行操作，然后返回此文档之后的部分。
:::

:::note
在`Ubuntu 22.04` 上使用新版本的 .NET Core 7 可能会遇到以下错误：`A fatal error occurred, the folder [/usr/share/dotnet/host/fxr] does not contain any version-numbered child folders` 如果你遇到了这个错误，请使用这个[解决方案](https://github.com/dotnet/sdk/issues/27082#issuecomment-1211143446)
:::

以上两条来自于mccteam官方文档

更新系统软件包和软件包管理库:
```sh
sudo apt update -y && sudo apt upgrade -y
```
(在使用大部分大厂服务器时，您无需更改**源**，如果出现错误,请参照[LinuxMirrors](https://linuxmirrors.cn/ "GNU/Linux 更换系统软件源脚本及 Docker 安装脚本"))

安装 `wget`
```sh
sudo apt install wget -y #安装 wget
cd ~                     #转到~目录
```

下载 Microsoft 存储库文件：
```sh
wget https://packages.microsoft.com/config/ubuntu/22.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb #下载 Microsoft 存储库文件
```

将 Microsoft 仓库添加到包管理器：
```sh
sudo dpkg -i packages-microsoft-prod.deb
#将 Microsoft 仓库添加到包管理器
rm packages-microsoft-prod.deb
#将文件删除
```

最后，安装 .NET Core 7：
```sh
sudo apt-get update -y && sudo apt-get install -y dotnet-sdk-7.0
```

运行下面的命令来检查一切是否正确安装
```sh
dotnet
```

您应该看到：

Usage: 
```text
Usage: dotnet [options]
Usage: dotnet [path-to-application]

Options:
  -h|--help         Display help.
  --info            Display .NET information.
  --list-sdks       Display the installed SDKs.
  --list-runtimes   Display the installed runtimes.

path-to-application:
  The path to an application .dll file to execute.
```

如果您没有获得这个输出且安装失败，请尝试[其它方法](https://docs.microsoft.com/zh-cn/dotnet/core/install/linux-ubuntu#2204)

### 安装mcc
1. 现在你已经安装了 `.NET Core 7`，你应该安装`screen`工具， 您需要这么做，以便在您关闭了 SSH 会话后保持MCC运行(如果您没有它的话， MCC 将在您断开连接后停止工作)。 您可以看到`screen`就像一个窗口， 除了在终端中，它允许您同时打开多个“窗口”。

要安装 `screen` 执行以下命令：
```sh
sudo apt install screen -y
```

#### 使用`Git`克隆
打开终端并导航到您将存储 MCC 的文件夹，然后执行以下命令(如果没有，可以使用`mkdir`命令用于新建目录，然后`cd`用于进入目录)
```sh
git clone https://github.com/MCCTeam/Minecraft-Console-Client.git --recursive
```
1.1 此过程中报错的解决方法
如果在此过程中报错`Timed out`或`Refused to connect`说明服务器当前无法连接至[Github](https://github.com/)
解决方法如下:

前往[ITDOG-WebTest](https://www.itdog.cn/http/)输入github.com点击测试并查看测试结果

1.2 测试结果筛选：
请选择您所在的地域和运营商 测试状态码为200的ip并记录下来，我所选择的ip不一定在以后依旧有效

~~请不要使用某个gitee的镜像，那个太老了，我没注意直接clone了，也是踩坑了~~

![示例图片](https://s2.loli.net/2024/09/08/jaUnI2NwrpsDoid.jpg)

下面进行修改`hosts`操作:
1.3 进入vim编辑器编辑hosts文件(请sudo，否则会报file read-only错误)
```sh
sudo vim /etc/hosts
```
1.4 在结尾添加以下两行
```text
20.200.245.247 github.com 
20.200.245.247 raw.githubusercontent.com
```
按键盘上的`i`进入编辑模式，添加后按`Esc`进入命令行模式，输入`:wq`并回车以进行保存并退出
(请将其中的ip替换为其他ip)

重新返回到原先存放项目的目录 进行git clone操作
请参考[使用Git克隆](####使用`Git`克隆)

2. 转到您克隆过的文件夹 (应该是 `Minecraft-Console-Client`)
```sh
cd Minecraft-Console-Client #进入目录
```
3. 如果您想下载翻译资源，请查看[下载翻译资源](https://mccteam.github.io/l10n/zh-Hans/guide/%E2%80%9C#download-translation-resources-optional%E2%80%9D)
4. 运行以下命令以生成项目：
```sh
dotnet publish MinecraftClient -f net7.0 -r linux-x64 --no-self-contained -c Release -p:UseAppHost=true -p:
```
>如果您使用的是 ARM、32 位、基于 Rhel、Using Musl 或 Tirzen 的 Linux，请为您的平台[找到合适的 RID]（https://docs.microsoft.com/zh-cn/dotnet/core/rid-catalog#linux-rids），并将 '-r linux-64' 替换为适当的 '-r RID_NAME'（arm 示例：“-r linux-arm64”）

5. 运行完成后的文件将被输入以下目录:
```text
MinecraftClient/bin/Release/net7.0/linux-x64/publish/
```
###启动&编辑配置文件
1. 启动`screen`
```sh
screen -S mcc
```
>`mcc`是这里屏幕上的名字，你可以使用你喜欢的任何东西，但如果你使用了不同的名字的话。请确保你在以下命令中使用该命令而不是`mcc`

要重新连接/返回到屏幕，请执行以下命令：
```sh
screen -d -r mcc
```
此处也是我踩坑的一个点，由于不会使用screen，导致这里出了问题，上次连接时没有断开导致下次无法连接，而解决方法是加上 `-d`来分离上次的会话

要列出屏幕，您可以使用：
```sh
screen -ls
```

2. 启动mcc
```sh
./MinecraftClient
```
3. 登录部分
现在，程序将要求您输入账户与密码
```text
账户部分
对于正版用户:输入微软邮箱
对于离线用户:输入游戏id
密码部分
对于正版用户:输入微软账户密码
对于离线用户:输入`-`
！！！注意:密码输入将不会被显示
```

4. 在登陆中遇到的问题

账户需要二次验证/无法连接到Microsoft

**这个也是我踩坑的一个大点，微软账户的验证堪称玄学，有时需要验证有时又不需要了，所以为了避免在登录过程中的此类报错，我们可以换用另一种方式登录，即将配置文件中的`mcc`改为`browser`后重新启动mcc
这时你就会发现：无法正常打开浏览器但是返回了一个URL链接，打开URL链接，登录微软账户，这是你就会发现返回了一个类似于密钥的text，将其复制出来，粘贴回mcc，即可登录成功**

5. 修改配置文件
首先如果你处于游戏中，请先`/quit`
```sh
vim MinecraftClient.ini
```
6. 配置账户`Account`
正如之前所说，正版登录连接错误可以参考上文在登陆中遇到的问题
```text
Account = { Login = "<email>", Password = "<password>" }
#以下为注释
email:正版填写邮箱，离线填写游戏内ID
password:正版填写微软账户密码，离线填写`-`
```

7. 配置服务器`Server`
配置此项后即可直接进入填写的服务器
```text
服务器 = { 主机 = "<ip>", 端口 = <port> }
Server = { Host = "192.168.1.27", Port = 12345 } #示例
```

8. 配置登录方式`Method`
```text
Method = "mcc"
```
此设置用于定义使用Microsoft帐户登录的方式，可用选项包括mcc 和browser
正如我之前所说的，如果出现错误，可以尝试将此处改为`browser`

9. 配置bot主人`BotOwners`
```text
BotOwners = [ "milutinke", "bradbyte", "BruceChen", ]
```

10. 配置mc版本`MinecraftVersion`
```text
MinecraftVersion = "1.20.4"
```

11. 配置自动重生`AutoRespawn`
将`false`改为`true`

**以上是我所做的基本配置**除此之外，我还配置了自动接tp的配置，这需要修改`remote control`和`chat format`

```text
[ChatFormat]
Builtins = false #是否启用MCC内置的聊天检测规则。设置为false 以避免与自定义格式冲突。UserDefined = true# 是否启用下方的自定义正则表达式进行聊天检测。
Public = "<([a-zA-Z0-9_]+)>: (.+)"
Private = '^\[([a-zA-Z0-9_]+) -> 你\] (.+)$'
TeleportRequest = "^([a-zA-Z0-9_]+) 请求传送到你的位置$"
```
注意，中文服务器基本无法使用默认的聊天检测规则，具体请查看[正则表达式](https://www.google.com.hk/url?sa=t&source=web&rct=j&opi=89978449&url=https://zh.wikipedia.org/zh-hans/%25E6%25AD%25A3%25E5%2588%2599%25E8%25A1%25A8%25E8%25BE%25BE%25E5%25BC%258F&ved=2ahUKEwiwmIuq3rKIAxWsr1YBHSweJ8cQFnoECBsQAQ&usg=AOvVaw36u8i0rwVEGuG-klosrE7J "Wikipedia")

```text
[ChatBot.RemoteControl]
Enabled = true
AutoTpaccept = true
AutoTpaccept_Everyone = false
```
`Enabled`:是否开启`remotecontrol`

`AutoTpaccept`:是否自动接owner的tp

`AutoTpaccept_Everyone`:是否自动接所有人的tp

除此之外，mcc对于离线用户同样也有自动登录的解决方案
那就是使用脚本，以下是脚本示例:
```text
[ChatBot.ScriptScheduler]
Enabled = true
[[ChatBot.ScriptScheduler.TaskList]]
Task_Name = "登录RIA"
Trigger_On_First_Login = false
Trigger_On_Login = true
Trigger_On_Times = { Enable = false, Times = [ ] }
Trigger_On_Interval = { Enable = false, MinTime = 5.0, MaxTime = 5.0 }
Action = "send /login 密码1145141919810"
```
您还可以添加命令进行在有导航服务器时自动加入主服务器
 例如`/joinq zeroth`具体命令取决于所处服务器

此段脚本内容感谢Fle的支持


mcc有着丰富的集成自动功能，详情查看[这里](https://mccteam.github.io/l10n/zh-Hans/guide/configuration.html#%E7%AD%BE%E5%90%8D%E9%83%A8%E5%88%86)

### 配置mcs(可选)
配置mcs只是为了让我能够通过web管理，非必要

项目地址已经在开头给出
![mcs](https://s2.loli.net/2024/09/08/R5nOZWSrLIw2KQs.jpg)

`快速建立应用实例``部署任意控制台应用程序``localhost``无需额外文件`

启动命令`screen -d -r mcc`

# 好啦，大功告成！

![Screenshot_2024-09-08-15-02-55-179_com.android.chrome-edit.jpg](https://s2.loli.net/2024/09/08/1AUevj4SYQJZhl5.jpg)

呜呜第一次写markdown，十分坐牢