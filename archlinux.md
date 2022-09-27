# ArchLinux

## pacman
```shell
pacman -S package_name        # 安装软件  
pacman -S extra/package_name  # 安装不同仓库中的版本
pacman -Syu                   # 升级整个系统，y是更新数据库，yy是强制更新，u是升级软件
pacman -Ss string             # 在包数据库中查询软件
pacman -Si package_name       # 显示软件的详细信息
pacman -Sc                    # 清除软件缓存，即/var/cache/pacman/pkg目录下的文件
pacman -R package_name        # 删除单个软件
pacman -Rs package_name       # 删除指定软件及其没有被其他已安装软件使用的依赖关系
pacman -Qs string             # 查询已安装的软件包
pacman -Qi package_name       # 查询本地安装包的详细信息
pacman -Ql package_name       # 获取已安装软件所包含的文件的列表
pacman -Qtdq                  # 获取未被其他软件使用的软件包列表
pacman -U package.tar.zx      # 从本地文件安装
pactree package_name          # 显示软件的依赖树
```

## Yay
>  Yet Another Yogurt: 一个用于 Arch Linux 的工具，用于从 Arch User Repository 中构建和安装软件包。 另见 pacman。
```shell
sudo pacman -S yay #安装yay
sudo yay --aururl "https://aur.tuna.tsinghua.edu.cn" --save #设置国内源
sudo yay -P -g #检查源是不是国内的
sudo yay {{软件包|搜索词}} #从仓库和 AUR 中交互式搜索和安装软件包：
sudo yay #同步并更新所有来自仓库和 AUR 的软件包：
sudo yay -Sua #只同步和更新 AUR 软件包：
sudo yay -S {{软件包}} #从仓库和 AUR 中安装一个新的软件包。
sudo yay -Ss {{关键词}} #从仓库和 AUR 中搜索软件包数据库中的关键词：
sudo yay -Ps #显示已安装软件包和系统健康状况的统计数据：

```

```shell
#code 
sudo yay -S android-studio
sudo yay -S intellij-idea-ultimate-edition 
sudo yay -S vscode
sudo yay -S mousepad
sudo yay -S geidt

#ui 
sudo yay -S latte-dock

#explore
sudo yay -S google-chrome
sudo yay -S microsoft-edge-stable
sudo yay -S microsoft-edge-dev-bin

#office
sudo yay -S deepin-wine-wechat
sudo yay -S wps-office
sudo yay -S ttf-wps-fonts
sudo yay -S flameshot

#type
sudo pacman -S fcitx-sogoupinyin
sudo pacman -S fcitx-im
sudo pacman -S fcitx-configtool

#font
sudo yay -S nerd-fonts-jetbrains-mono
sudo pacman -S ttc-iosevka

#environment
sudo yay -S nodejs 
sudo yay -S npm 
sudo yay -S jdk8-openjdk 
sudo yay -S jdk11-openjdk 

#command tools
sudo yay -S brightnessctl
sudo yay -S trash-cli
sudo yay -S lf 
sudo yay -S lazygit 
sudo yay -S ripgrep 
sudo yay -S fzf 
sudo yay -S zsh 
sudo yay -S htop 
sudo yay -S starship 
sudo yay -S bat 
sudo yay -S tig 
sudo yay -S exa 
sudo yay -S fd 

sudo yay -S teamviewer
sudo yay -S kchmviewer
sudo yay -S obs-studio 

sudo pacman -Sy jdk 
sudo archlinux-java set java-11-openjdk #自由切换jdk版本 
sudo pacman -Sy jdk8-openjdk #安装openjdk8
sudo archlinux-java status #查看当前java版本

```

```shell
sudo pacman -Syy
sudo pacman -S vscode nnn ranger lf lazygit ripgrep fzf zsh htop btop starship bat tig exa fd neovim nodejs npm
jdk8-openjdk kchmviewer xmind-zen deepin-picker vlc unarchiver typora-free geidt nitrogen brightnessctl
flameshot microsoft-edge-dev-bin obs-studio  clash clash-for-windows-bin typora-free-cn
yay -S ttf-wps-fonts wps-office wps-office-mui-zh-cn wps-office-mime-cn ttf-ms-fonts cups
yay -S dragon-drop trash-cli xmysql sxiv zoxide lynx mousepad
```
