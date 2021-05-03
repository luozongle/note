[TOC]



## 一、从github上clone项目报错



### 1.错误状态:fatal: Could not read from remote repository.



将`~/.ssh/id_rsa.pub`公钥添加到github上以后，clone代码报以下错误

```shell
MacintoshdeMacBook-Pro-4:~/study/git v_luozongle$ git clone git@github.com:luozongle/note.git
Cloning into 'note'...
ssh_exchange_identification: read: Connection reset by peer
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```



将`~/.ssh/known_hosts`中github地址删除以后还是不行



最后将`~/.ssh/config`中添加以下配置以后 问题解决


```config
Host github.com
    Hostname ssh.github.com
    Port 443
```

