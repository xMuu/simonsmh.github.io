---
title: termux 配置 oh-my-zsh 脚本
date: 2016-07-27 14:33:00
tags: [chat]
---
## termux配置oh-my-zsh脚本  
推荐ys主题，shell要是没有路径显示很难受
```
#!/data/data/com.termux/files/usr/bin/bash
termux-setup-storage
apt update
apt install -y git zsh
git clone https://github.com/simonsmh/oh-my-termux.git
if [ -d "$HOME/.termux" ]; then
 mv $HOME/.termux $HOME/.termux.bak
fi
mv oh-my-termux/.termux $HOME/.termux
rm -rf oh-my-termux
git clone git://github.com/robbyrussell/oh-my-zsh.git $HOME/.oh-my-zsh
cp $HOME/.oh-my-zsh/templates/zshrc.zsh-template $HOME/.zshrc
chsh -s zsh
echo Done!
exit
```
