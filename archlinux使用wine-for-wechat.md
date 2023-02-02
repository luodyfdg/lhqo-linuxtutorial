# 前言
如果你在使用wine或者deepin-wine6-stable安装微信，那么你可能会遇到这个方框的问题，为了避免这个方框的问题，我们需要使用wine-for-wechat这个包
## 安装前的准备
首先，你要知道，这个包他是和原版的wine冲突的，但是不怎么影响，也能正常用。
在安装之前，我们要配置archlinuxcn源，配置方法就不说了，这里放清华源和中科大源的链接，自己去看吧
[中科大配置帮助][1]
[清华源配置帮助][2]


## 安装和使用
直接安装wine-for-wechat这个包即可
```bash
sudo pacman -S wine-for-wechat
```
安装之后，在微信的官网下载安装包
[微信官网][3]
使用wine运行安装包即可
```bash
WINEARCH=win32 WINEPREFIX=~/.wine/WeChat wine xxx.exe
```
注:WINEPREFIX后的路径改为你希望安装的路径，"xxx.exe"改为微信安装包的文件名.exe。
过一遍安装程序，安装完成后，desktop文件应该会自动生成，这个时候，微信应该就能用了。
## 最后
使用wine-for-wechat这个包，这个包是针对微信和网易云音乐删去阴影，使这些应用的方框问题不复存在，就可以正常使用了。
当然，你如果不用wine,用虚拟机也可以，这里就不说了。


## 注意
这只是粗略的使用方法，可能有一定错误

  [1]: https://mirrors.ustc.edu.cn/help/archlinuxcn.html
  [2]: https://mirror.tuna.tsinghua.edu.cn/help/archlinuxcn/
  [3]: https://weixin.qq.com/
