[TOC]

# Iterm2常用操作



## 一、Iterm2配置rz sz



### 1.在mac上面安装lrzsz

```zsh
brew install lrzsz
```


### 2.在`/usr/local/bin`目录下创建以下两个文件



> /usr/local/bin/iterm2-recv-zmodem.sh
```sh
#!/bin/bash
# Author: Matt Mastracci (matthew@mastracci.com)
# AppleScript from http://stackoverflow.com/questions/4309087/cancel-button-on-osascript-in-a-bash-script
# licensed under cc-wiki with attribution required
# Remainder of script public domain

osascript -e 'tell application "iTerm2" to version' > /dev/null 2>&1 && NAME=iTerm2 || NAME=iTerm
if [[ $NAME = "iTerm" ]]; then
	FILE=$(osascript -e 'tell application "iTerm" to activate' -e 'tell application "iTerm" to set thefile to choose folder with prompt "Choose a folder to place received files in"' -e "do shell script (\"echo \"&(quoted form of POSIX path of thefile as Unicode text)&\"\")")
else
	FILE=$(osascript -e 'tell application "iTerm2" to activate' -e 'tell application "iTerm2" to set thefile to choose folder with prompt "Choose a folder to place received files in"' -e "do shell script (\"echo \"&(quoted form of POSIX path of thefile as Unicode text)&\"\")")
fi

if [[ $FILE = "" ]]; then
	echo Cancelled.
	# Send ZModem cancel
	echo -e \\x18\\x18\\x18\\x18\\x18
	sleep 1
	echo
	echo \# Cancelled transfer
else
	cd "$FILE"
	/opt/homebrew/bin/rz -E -e -b --bufsize 4096  # 根据实际情况修改命令位置
	sleep 1
	echo
	echo
	echo \# Sent \-\> $FILE
fi
```

 

> /usr/local/bin/iterm2-recv-zmodem.sh
```sh
#!/bin/bash
# Author: Matt Mastracci (matthew@mastracci.com)
# AppleScript from http://stackoverflow.com/questions/4309087/cancel-button-on-osascript-in-a-bash-script
# licensed under cc-wiki with attribution required
# Remainder of script public domain

osascript -e 'tell application "iTerm2" to version' > /dev/null 2>&1 && NAME=iTerm2 || NAME=iTerm
if [[ $NAME = "iTerm" ]]; then
	FILE=$(osascript -e 'tell application "iTerm" to activate' -e 'tell application "iTerm" to set thefile to choose folder with prompt "Choose a folder to place received files in"' -e "do shell script (\"echo \"&(quoted form of POSIX path of thefile as Unicode text)&\"\")")
else
	FILE=$(osascript -e 'tell application "iTerm2" to activate' -e 'tell application "iTerm2" to set thefile to choose folder with prompt "Choose a folder to place received files in"' -e "do shell script (\"echo \"&(quoted form of POSIX path of thefile as Unicode text)&\"\")")
fi

if [[ $FILE = "" ]]; then
	echo Cancelled.
	# Send ZModem cancel
	echo -e \\x18\\x18\\x18\\x18\\x18
	sleep 1
	echo
	echo \# Cancelled transfer
else
	cd "$FILE"
	/opt/homebrew/bin/rz -E -e -b --bufsize 4096  # 根据实际情况修改命令位置
	sleep 1
	echo
	echo
	echo \# Sent \-\> $FILE
fi
```



需要给上面两个脚本777权限

```zsh
sudo chmod 777 /usr/local/bin/iterm2-*
```





**需要注意的是:**

不同版本brew安装的rz\sz命令位置可能不同，可以用which命令查看rz\sz的实际位置，修改脚本中rz\sz命令位置。



### 3、配置iterm2
> Preferences -> Profiles -> Advanced -> Triggers -> Edit  改为一下配置

![tools_mac_iterm2_trigger](../../images/tools_mac_iterm2_trigger.png)



**解释:**

Regular Experssion:正则表达式，写入屏幕的文本将被发送到regex 进行匹配。    

Action: 被正则匹配成功后执行的操作，`Run Silent Coprocess`的意思是，运行静默协进程，这与协进程的不同之处在于，输出只传递给协进程，在运行时不会显示。  

Parameters: 各种参数(运行命令、协进程、发送文本之类的都需要参数信息)，在这个字段指定。  

Instant:如果打开了Instant，一旦发生与正则匹配成功，触发器会立即触发，不会等待换行，如果没有打开，需要等换行才会触发。  



**需要注意的是:**

Regular Experssion 匹配的值可能是不确定的，需要看执行rz\sz命令后看具体弹出的信息。  



rz命令和sz命令

常用参数

```txt
-a, --ascii   # 以ascii码方式传输
-b, --binary  # 以二进制方式传输
-e, --escape  # 对所有控制字符转义
-y, --overwrite # 源文件如果有，则删除旧文件
```

为了防止传输过程中出现意外，最好加上`-be`参数

```zsh
rz -be 
sz -be file1 file2 ...
```

















