[TOC]

# zsh使用文档



### 简介: 
Zsh(Z-shell)是一款用于交互式使用的shell，也可以作为脚本解释器来使用。其包含了bash、ksh、tcsh等其他shell中许多优秀功能，也拥有诸多自身特色。

### 读取配置文件顺序：
$ZDOTDIR/.zshenv
$ZDOTDIR/.zprofile
$ZDOTDIR/.zshrc
$ZDOTDIR/.zlogin
$ZDOTDIR/.zlogout
${TMPPREFIX}* (default is /tmp/zsh*)
/etc/zshenv
/etc/zprofile
/etc/zshrc
/etc/zlogin
/etc/zlogout (installation-specific - /etc is the default)

