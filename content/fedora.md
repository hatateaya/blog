---
title: "Fedora 41 GNOME 安装配置笔记"
date: 2024-11-01T12:00:00+08:00
draft: false
---
- 换源更新：
   - `sudo sed -e 's|^metalink=|#metalink=|g'          -e 's|^#baseurl=http://download.example/pub/fedora/linux|baseurl=https://mirrors.ustc.edu.cn/fedora|g'          -i.bak          /etc/yum.repos.d/fedora.repo          /etc/yum.repos.d/fedora-updates.repo`
   - `sudo dnf update`
   - `sudo dnf autoremove`
- 删除无用软件，安装必要软件：
   - `sudo dnf remove gnome-tour gnome-boxes gnome-software flatpak gnome-connections gnome-contacts gnome-calendar mediawriter`
   - `sudo dnf install vim p7zip file-roller`
- 启用RPM Fusion源：
   - `sudo dnf install https://mirrors.ustc.edu.cn/rpmfusion/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.ustc.edu.cn/rpmfusion/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm`
   - `sudo sed -e 's|^metalink=|#metalink=|g'          -e 's|^#baseurl=http://download1.rpmfusion.org|baseurl=https://mirrors.ustc.edu.cn/rpmfusion|g'          -i.bak          /etc/yum.repos.d/rpmfusion*.repo`
   - `sudo dnf update`
- 配置电源管理：
   - `sudo dnf install tlp tlp-rdw`
   - `sudo systemctl enable tlp.service --now`
   - `sudo systemctl mask systemd-rfkill.service`
   - `sudo systemctl mask systemd-rfkill.socket`
- 配置 GNOME 并进行基础美化：
   - `sudo dnf install papirus-icon-theme gnome-tweaks gnome-extensions-app`
   - 打开 Gnome Tweak 启用 icon 主题并配置窗口右上角按键
   - 打开 <https://extensions.gnome.org>
   - 安装 `blur-my-shell` `dash-to-panel` `appindicator`
   - 打开 Gnome Extension 启用上述插件并配置 dash-to-panel
- 配置中文输入法：
   - `sudo dnf install fcitx5 fcitx5-autostart fcitx5-chinese-addons fcitx5-configtool`
   - 重启并在 fcitx5-config-tool 中配置使用中文输入法
   - 在 <https://extensions.gnome.org> 中下载 kimpanel 并安装
- 配置 ZIP 包自动按 GB2312 编码解压：
   - 在 ~/.bashrc 中加入：
```
UNZIP="-O CP936"
ZIPINFO="-O CP936"
```
- 配置代理：
   - 到 <https://github.com/MetaCubeX/mihomo/releases> 下载 mihomo rpm 并用 dnf 安装
   - 到 <https://github.com/MetaCubeX/meta-rules-dat/releases/tag/latest> 下载 country.mmdb
   - 到你的机场网站下载你的 config.yaml
   - `sudo systemctl enable mihomo --now`
   - 以 root 权限复制 config.yaml country.mmdb 到 /etc/mihomo/ 目录下
   - `sudo systemctl restart mihomo`
   - 在 Settings 里配置系统代理
- 可选：安装软件：
   - 到 <https://im.qq.com> 下载 QQ rpm 并用 dnf 安装
   - 到 <https://chrome.google.com> 下载 chrome rpm 并用 dnf 安装
   - 到 <https://code.visualstudio.com> 下载 vscode rpm 并用 dnf 安装
   - `sudo dnf install telegram-desktop`
- 可选：关闭GRUB启动等待：
   - `sudo vim /etc/default/grub`
   - 将TIMEOUT一项调为0
   - `sudo grub2-mkconfig -o /boot/grub2/grub.cfg`
- 可选：配置 pixelbook 驱动
   - 参考： `https://github.com/jmontleon/pixelbook-fedora`
   - pixelbook 2017 若出现耳机左耳杂音则打开 alsamixer 选择声卡 选择 avs\_rt5663 选中第一个 ADDA\_MIX 按 m 静音
- 可选：配置除 pixelbook 外的 chromebook 音频驱动
   - 参考： `https://github.com/WeirdTreeThing/chromebook-linux-audio`
- 可选：配置 java 开发环境：
   - `sudo dnf install java-21-openjdk-devel`
- 可选：配置 nodejs 环境：
   - `sudo dnf install yarn`
   - `yarn config set registry https://registry.npmmirror.com`
   - `yarn config set electron_mirror https://registry.npmmirror.com/electron/`
- 可选：配置 rust 开发环境：
   - 在 ～/.bashrc 中加入 
```
export RUSTUP_DIST_SERVER=https://mirrors.ustc.edu.cn/rust-static
export RUSTUP_UPDATE_ROOT=https://mirrors.ustc.edu.cn/rust-static/rustup
```
   - `source ~/.bashrc`
   - `curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh`
   - 创建 ~/.cargo/config.toml 文件 写入
```
[source.crates-io]
replace-with = 'ustc'
[source.ustc]
registry = "sparse+https://mirrors.ustc.edu.cn/crates.io-index"
```
- 可选：安装 electron ncm：
   - 参考：<https://github.com/Rocket1184/electron-netease-cloud-music>
   - 若自己编译 可用带 UNM 支持的 fork： <https://github.com/hatateaya/electron-netease-cloud-music>
- 可选：安装 zen browser：
   - `sudo dnf copr enable sneexy/zen-browser`
   - `sudo dnf install zen-browser`
- 可选：配置 git 以向 GitHub push：
   - `sudo dnf install gh`
   - `gh auto login`
- 其他：一些 dnf 命令参考：
   - `dnf search SOFTWARENAME`
   - `sudo dnf install SOFTWARENAME`
   - `sudo dnf remove SOFTWARENAME`
   - `dnf history list`
   - `dnf list --installed | grep SOFTWARENAME`
   - `dnf repoquery --userinstalled`
