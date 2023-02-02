# 前言
本文只介绍ArchLinux安装后的一些配置和安装桌面与美化shell,arch安装请另外寻找办法。
## 1、连接网络
如果你是有线连接，请输入这行命令:
```bash
systemctl start dhcpcd
```
### ping测试
```bash
ping baidu.com
```
(本文以baidu.com为例，你也可以使用其他网址进行测试)<br>
## 2、打开32位支持库
```bash
vim /etc/pacman.conf
```
去掉[multilib]一行的注释，:wq保存过后使用命令刷新pacman:
```bash
pacman -Syyu
```
## 3、设置swap文件
如果你在分区的时候分了swap,那么你可以不进行设置。本文只介绍简单设置
```bash
dd if=/dev/zero of=/swapfile bs=1M count=4096 status=progress #创建4G的交换文件
chmod 600 /swapfile #设置权限
mkswap /swapfile #格式化
swapon /swapfile #启用swap
```
然后向fstab写入这些内容：
```bash
/swapfile none swap defaults 0 0
```
## 4、用户增加
使用useradd命令来管理用户
```bash
useradd -m -G wheel -s /bin/bash 你的用户名
```
注意，在这行命令中，用户名不能有大写和中文()，Shell不能指定一个没有正确配置的Shell<br>
设置新用户的密码:
```bash
passwd 你的用户名
```
使用vim编辑sudoers
```bash
EDITOR=vim visudo
```
找到``#%wheel ALL=(ALL) ALL``这一行，把注释符号（即最前面的#）去掉，:wq保存即可。
## 5、安装Plasma
你也可以装其他的桌面，当然我这里拿kde举例子
```bash
pacman -S plasma-meta konsole dolphin
```
如果你有使用Wayland的需求，那么安装好这几个包
```bash
pacman -S plasma-wayland-session plasma-wayland-protocols qt5-wayland qt6-wayland xorg
```
### 设置sddm开机自启
```bash
systemctl enable sddm
```
### 安装中文字体
```bash
pacman -S adobe-source-code-pro-fonts adobe-source-han-sans-cn-fonts adobe-source-han-serif-cn-fonts # adobe的字体
pacman -S wqy-zenhei # 文泉驿
pacman -S noto-fonts-cjk noto-fonts-emoji noto-fonts-extra # 谷歌开源字体
```
以上这些即可满足日常使用<br>
注：不要安装太多字体。<br>
至此，你应该可以可以重启了。
## 6、安装软件
安装软件前开启网络
```bash
sudo systemctl enable --now NetworkManager
```
安装一些基础的功能包。
```bash
sudo pacman -S sof-firmware alsa-firmware alsa-ucm-conf
sudo pacman -S ntfs-3g
sudo pacman -S ark
sudo pacman -S p7zip unrar unarchiver lzop lrzip
sudo pacman -S packagekit-qt5 packagekit appstream-qt appstream
sudo pacman -S git wget kate bind
```
重启后继续安装软件:<br>
网络浏览器 ``Firefox Chromium``
看图 ``gwenview``
视频软件 `vlc`
## 7、设置DNS
vim编辑/etc/resolv.conf,删除已有条目,填入以下内容
```bash
nameserver 223.5.5.5
nameserver 223.6.6.6
```
设置只读标志
```bash
sudo chattr +i /etc/resolv.conf
```
## 8、设置中文
打开Settings > Regional Settings -> Add languages中选择中文加入，拖到第一位Apply应用。
然后把格式全设置为`简体中文（中国）`
注销掉重新登陆
## 9、安装yay
没有aur的arch是没有灵魂的，先设置archlinuxcn源，vim打开/etc/pacman.conf,在最后面添加如下内容：
```bash
[archlinuxcn]
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
```
然后执行`sudo pacman -Syyu`更新软件源
安装archlinuxcn-keyring
```bash
sudo pacman -S archlinuxcn-keyring
```
安装yay
```bash
sudo pacman -S yay
```
## 10、安装输入法
```bash
sudo pacman -S fcitx5-im fcitx5-chinese-addons fcitx5-pinyin-zhwiki
yay -S fcitx5-pinyin-moegirl
```
设置环境变量，使用vim编辑文件`/etc/environment`加入以下内容
```bash
GTK_IM_MODULE=fcitx
QT_IM_MODULE=fcitx
XMODIFIERS=@im=fcitx
SDL_IM_MODULE=fcitx
```
打开fcitx的设置，点击添加输入法，添加`Pinyin`，点选Pinyin右边的小齿轮，勾选上**启用云拼音**和**在程序中显示预编辑文本**，应用。<br>
注销，登陆，就可以使用中文输入了。
## 11、配置默认编辑器
EDITOR=vim sudoedit /etc/profile添加以下内容：
```bash
export EDITOR='vim'
```
这样运行命令就不用每次都执行一遍`EDITOR='vim'`了。
## 12、显卡驱动
自带的驱动通常无法满足我们日常使用，而archlinux安装驱动非常方便，我的是AMD显卡，只需要安装这些包就可以了:
```bash
sudo pacman -S mesa lib32-mesa xf86-video-amdgpu vulkan-radeon lib32-vulkan-radeon libva-mesa-driver lib32-libva-mesa-driver mesa-vdpau lib32-mesa-vdpau
```
一些老显卡需要将`xf86-video-amdgpu`这个包更换为ATI开源驱动，其他显卡具体参考ArchWiki
## 13、配置zsh
安装zsh
```bash
sudo pacman -S zsh
```
安装oh my zsh
```bash
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
重命名配置文件
```bash
mv ~/.zshrc ~/.zshrc.oh-my-zsh
```
`vim ~/.zshrc`增加下面一行。
```bash
source ~/.zshrc.oh-my-zsh
```
### 安装高亮插件
```bash
cd $HOME/.oh-my-zsh/plugins
#下载代码
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git
#自动配置
echo "source ${(q-)PWD}/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh" >> ${ZDOTDIR:-$HOME}/.zshrc
```
### 安装自动补全
```bash
cd $HOME/.oh-my-zsh/plugins
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```
`vim ~/.zshrc.oh-my-zsh`找到plugins一行，在括号内换行加入`zsh-autosuggestions`
#### 应用设定
```bash
source ~/.zshrc
```
### 配置主题
```bash
wget https://github.com/ryanoasis/nerd-fonts/releases/download/v2.3.0-RC/Hack.zip
```
解压Hack.zip,安装里面的字体，在设置中将固定宽度字体设置为`Hack Nerd Font 10pt`
安装主题
```bash
git clone --depth=1 https://gitee.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```
更改配置文件，`vim ~/.zshrc.oh-my-zsh`设置`ZSH_THEME="powerlevel10k/powerlevel10k"`
`source ~/.zshrc`应用设定。
后期修改主题配置使用`p10k configure`即可。<br>
至此，ArchLinux的桌面安装与基本配置就完毕了。

