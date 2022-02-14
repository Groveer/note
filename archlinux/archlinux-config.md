# Arch Linux安装后配置

[TOC]

## 配置源

### 添加`archlinuxcn`仓库
1. 添加文件：`/etc/pacman.d/archlinuxcn-mirrorlist`</br>
添加内容：`Server = https://repo.archlinuxcn.org/$arch`</br>
更多内容：参考[国内仓库](https://github.com/archlinuxcn/mirrorlist-repo/blob/master/archlinuxcn-mirrorlist)</br>
2. 修改文件：`/etc/pacman.conf`</br>
取消注释 `#Color`</br>
添加内容：
```
[archlinux]
Include = /etc/pacman.d/archlinux-mirrorlist
```
3. 安装 GPG key：
```
sudo pacman -Syu
sudo pacman -S archlinuxcn-keyring
```
### 配置[aur](https://wiki.archlinux.org/title/Arch_User_Repository_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))
```
sudo pacman -S yay
```
以后就可以使用`yay`代替`pacman`了，语法完全一致，无需提前`sudo`，并且可访问`aur`仓库。

## 配置控制台登录图形桌面

1. 安装[xorg](https://wiki.archlinux.org/title/Xorg_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))：
```
sudo pacman -S xorg-server xorg-xinit
```
2. 修改`～/.xinitrc`文件，并添加内容，挑选自己的桌面环境添加：
```
exec startplasma-x11       (kde-x11)
exec startplasma-wayland   (kde-wayland)
exec startdde              (dde)
exec gnome-session         (gnome)
exec openbox-session       (openbox)
exec startxfce4            (xfce)
exec mate-session          (mate)
```
3. 修改`~/.bash_profile`(bash)或者`～/.zprofile`（zsh）文件，并添加内容：
```
if [[ -z $DISPLAY && $(tty) == /dev/tty1 ]]; then
  exec startx
fi
```

## 安装[oh-my-zsh](https://github.com/ohmyzsh/ohmyzsh)及插件

1. 安装必备工具：
```
sudo pacman -S git wget
```
2. 安装`oh-my-zsh`：
```
sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
3. 安装历史记录插件和语法检查插件
```
cd ~/.oh-my-zsh/plugins
git clone git://github.com/zsh-users/zsh-autosuggestions.git
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git
```
4. 修改`～/.zshrc`文件，找到`plugins=(git)`，改为：
```
plugins=(
	git
	sudo
	zsh-syntax-highlighting
	zsh-autosuggestions
)
```
5. 使插件生效：
```
source ~/.zshrc
```

## 安装输入法

1. 安装[fcitx5](https://wiki.archlinux.org/title/Fcitx5_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))：
```
sudo pacman -S fcitx5 fcitx5-chinese-addons
```
2. 配置环境变量`~/.pam_environment`：
```
GTK_IM_MODULE DEFAULT=fcitx
QT_IM_MODULE  DEFAULT=fcitx
XMODIFIERS    DEFAULT=\@im=fcitx
INPUT_METHOD  DEFAULT=fcitx
SDL_IM_MODULE DEFAULT=fcitx
GLFW_IM_MODULE DEFAULT=ibus
```

## 配置开发环境
### python
1. 下载pip
```
sudo pacman -S python-pip
```
2. 设置pip源
```
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```
### golang
1. 安装[go](https://wiki.archlinux.org/title/Go_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))：
```
sudo pacman -S go
```
2. 选择一个go工作目录，以`～/Develop/go`为例，新建三个目录：
```
mkdir -p ~/Develop/{src,bin.pkg}
```
3. 配置环境变量`~/bash_profile`：
```
export GOROOT=/usr/lib/go
export GOPATH=~/Develop/go		   # 这两行你需要
export GOBIN=~/Develop/go/bin		# 修改为自己的
export PATH=$PATH:$GOROOT/bin:$GOBIN
```
```
source ~/bash_profile
```
4. 配置GOPROXY
```
go env -w GOPROXY=https://goproxy.io,direct
```