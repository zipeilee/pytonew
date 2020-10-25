```
title:写给嘉贺：How to use Qutip in Windows with WSL
data:2020.10.24
tags:
	-Qutip
	-Python
	-WSL
```

*打开qutip官方文档的[installation页面](http://qutip.org/docs/latest/installation.html#)可以看到安装Qutip之前需要安装的依赖环境，其中大部分都是anaconda自带的python库，但还需要一个`gcc 4.7+`或`MS VS 2015`的`C++ 编译器`用来编译`Cython 文件`。在Windows下你可以选择去微软官网安装相应的`VS C++`编译器（为图省事也可以直接安装一个最新版的[Visual Studio Community](https://visualstudio.microsoft.com/zh-hans/vs/)）或者安装一个[MinGW](http://mingw.org/wiki/InstallationHOWTOforMinGW)。但对于qutip以及一些其他量子物理相关的库来说，Linux是一个更友好的操作系统，相应的依赖也不需要自己另行安装，因此我们还有一个更方便的选择：就是在WSL(Windows Subsystem for Linux)下使用Qutip*

## Step1-安装WSL2
参考[官网](https://docs.microsoft.com/zh-cn/windows/wsl/install-win10)
>建议使用Ubuntu20.04发行版，建议使用WSL2

## Step2-在Ubuntu下安装anaconda和qutip
打开你在上一步下载好的的Windows Terminal，在终端选项中选择Linux，然后你就进入到一个完完全全原汁原味的Ubuntu系统啦，剩下的就是在Ubuntu下如何安装anaconda，一样，参考[官网](https://docs.anaconda.com/anaconda/install/linux/)

安装好之后在终端里输入`python`，如果出现
```shell
Python 3.8.5 (default, Sep  4 2020, 07:30:14) 
[GCC 7.3.0] :: Anaconda, Inc. on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 
```
表示安装成功。

然后你就可以在终端里使用`pip install qutip`或者`conda install qutip`安装qutip啦。
安装好之后可以使用`pip list`命令查看是否安装成功。

## Step3-在Windows浏览器下使用jupyter lab
一切装好了剩下的就是写代码了。
之前我在[b站的视频](https://www.bilibili.com/video/BV1Yt4y1X7Rv)上介绍了如何使用vscode写python代码，不过那是完全的windows环境。好消息是vscode有一个插件叫做 remote-wsl， 通过这个插件你可以很方便的在win10的vscode上通过linux子系统环境写代码作调试，就像在windows里写代码一样。但我后来发现用vscode写jupter notebook文件还是有些缺陷，jupyter lab比它好用多了。因此今天先不详细介绍vscode的用法，你可以自行阅读[remote-wsl插件使用方法](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)并配置好。

正常来说使用Jupyter lab很简单，anaconda里已经自带了这个包，你安装好anaconda没问题好只需要在终端里输入命令`jupyter lab`就能使用。
但是wsl的问题在于，你只有一个终端，没有图形界面，jupter lab是通过浏览器访问的，你没有图形界面就没有浏览器，因此你输入上述命令之后，jupyter lab是被你打开了并且跑了起来，但是你却看不到它。不过wsl毕竟wsl，w开头的，因此wsl也是可以访问windows的exe程序的，也就是说，你可以配置相应的环境变量，把windows下的浏览器设置城wsl的默认浏览器！你只需要在终端里输入`code ~/.bashrc`

`.bashrc`是ubuntu的bash的配置文件。通过上面的命令打开配置文件之后，在最末尾加一行
```bash
export BROWSER='/mnt/c/Program Files/Mozilla Firefox/firefox.exe'
```
（当然路径改称你自己的安装路径，而且不一定是firefox，chrome也可以，edge也可以，这个只是参考）
ctrl + s 保存文件之后，再在终端里输入`source ~/.bashrc`
然后你就能通过`jupyter lab`命令正常打开jupyter lab啦。关于jupyter lab的配置和使用可以参考[我的b站视频](https://www.bilibili.com/video/BV1Yt4y1X7Rv)中使用jupyter notebook的部分以及[他们的官网](https://jupyter.org/)。

## Step4-在WSL上使用latex
latex在linux下的编译速度是比在windows下快的！而且快很多！所以建议在wsl下使用latex。并且在ubuntu下安装texlive比在windows下简单许多，你只需输入命令：
```bash
sudo apt install texlive-full
```
等着它装好就行了。
texlive当然还是配合vscode才好用了。
在wsl终端，cd到你的某个latex工作文件文件夹里，使用命令`code .`，你就在这个文件夹下打开了vscode, 按照我之前[推文](https://blog.exavalon.tk/2020/08/13/yong-hui-vscode-xie-latex-liao/)的vscode插件配置部分配置到latex-workship插件，就能愉快的使用啦。
