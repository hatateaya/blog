---
title: "Fedora安装初配置"
date: 2024-11-01T12:00:00+08:00
draft: false
---
1. 删除无用软件：
 - `sudo dnf remove gnome-tour`
 - `sudo dnf remove gnome-boxes`
 - `sudo dnf remove gnome-software`
 - `sudo dnf remove flatpak`
2. 换源更新装vim：
 - `sudo sed -e 's|^metalink=|#metalink=|g'          -e 's|^#baseurl=http://download.example/pub/fedora/linux|baseurl=https://mirrors.ustc.edu.cn/fedora|g'          -i.bak          /etc/yum.repos.d/fedora.repo          /etc/yum.repos.d/fedora-updates.repo`
 - `sudo dnf update`
 - `sudo dnf autoremove`
 - `sudo dnf install vim`
3. 关闭GRUB启动等待：
 - `sudo vim /etc/default/grub`
 - 将TIMEOUT一项调为0
 - `sudo grub2-mkconfig -o /boot/grub2/grub.cfg`
4. 美化：
 - `sudo dnf install papirus-icon-theme`
 - `sudo dnf install gnome-tweaks gnome-extensions-app`
 - 打开 Gnome Tweak 启用 icon 主题并配置窗口右上角按键
 - 打开 <https://extensions.gnome.org>
 - 安装 `blur-my-shell` `dash-to-panel` `appindicator`
 - 打开 Gnome Extension 启用上述插件并配置 dash-to-panel
5. 配置电源管理：
 - `sudo dnf install tlp tlp-rdw`
 - `sudo systemctl enable tlp.service --now`
 - `sudo systemctl mask systemd-rfkill.service`
 - `sudo systemctl mask systemd-rfkill.socket`
6. 启用RPM Fusion源：
 - `sudo dnf install https://mirrors.ustc.edu.cn/rpmfusion/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.ustc.edu.cn/rpmfusion/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm`
 - `sudo sed -e 's|^metalink=|#metalink=|g'          -e 's|^#baseurl=http://download1.rpmfusion.org|baseurl=https://mirrors.ustc.edu.cn/rpmfusion|g'          -i.bak          /etc/yum.repos.d/rpmfusion*.repo`
 - `sudo dnf update`
7. 安装软件配置代理：
 - `sudo dnf install telegram-desktop`
 - 打开 <https://im.qq.com> 下载 QQ rpm 并用 dnf 安装
 - 打开 QQ 并登陆，将从手机下载的 clash rpm config.yaml country.mmdb 传到电脑
 - 用 dnf 安装 clash rpm
 - `sudo systemctl enable mihomo --now`
 - 复制 config.yaml country.mmdb 到 /etc/mihomo/
 - `sudo systemctl restart mihomo`
 - 在 Settings 里配置系统代理
 - 到 <https://chrome.google.com> 下载 chrome rpm 并用 dnf 安装
8. 配置中文输入法：
 - `sudo dnf install fcitx5 fcitx5-autostart fcitx5-chinese-addons fcitx5-configtool`
 - 重启并在 fcitx5-config-tool 中配置使用中文输入法
 - 在 <https://extensions.gnome.org> 中下载 kimpanel 并安装
9. 若安装 yarn rustup 需另行换源
 - `sudo dnf install yarn rustup`
 - 配置 yarn 源 与 electron 下载使用代理
 - 配置 cargo 源
