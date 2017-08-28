## Mac系统Python升级

Mac是默认安装了 Python 2.7 版本在／System目录下。


### 第一步 修改权限

因为Mac有个Rootless机制，所以不允许修改／System，所以要关闭Rootless机制。

1. 重启电脑, 重启过程中按住command+R, 进入恢复模式 
1. 打开terminal，键入: `csrutil disable` 
1. 重启电脑


>事后若是开启个Rootless机制，重复上面操作，命令改为`csrutil enable` 

### 第二步 安装python3.6

[Python-3.6-下载地址](https://www.python.org/downloads/)

下载并安装

默认安装目录：

    /Library/Frameworks/python.framework/Versions/
    

### 第三步 删除旧版本

在终端执行

    sudo rm -R /System/Library/Frameworks/Python.framework/Versions/2.7


### 第四步 移动新版本

将刚才安装好的python 3.6 移动到`/System/Library/Frameworks/Python.framework/Versions/`目录下，执行：

    sudo mv /Library/Frameworks/Python.framework/Versions/3.6 /System/Library/Frameworks/Python.framework/Versions

### 第五步 修改新版本用户组

将移动过来的3.6设置为和旧版本一样的用户组:

    sudo chown -R root:wheel /System/Library/Frameworks/Python.framework/Versions/3.6

### 第六步 修改软链接

在Versions的目录里有一个Current的link，是指向当前的Python版本，原始是指向系统自带的Python-2.7，我们把它删除后，link就失效了，所以需要重新链一下


    sudo rm /System/Library/Frameworks/Python.framework/Versions/Current  
    sudo ln -s /System/Library/Frameworks/Python.framework/Versions/3.6 /System/Library/Frameworks/Python.framework/Versions/Current


### 第七步 修改执行链接

- 修改原执行路径


    sudo rm /usr/bin/pydoc
    sudo rm /usr/bin/python
    sudo rm /usr/bin/pythonw
    sudo rm /usr/bin/python-config
    
- 建立新链接



    sudo ln -s /System/Library/Frameworks/Python.framework/Versions/3.6/bin/pydoc3.6 /usr/bin/pydoc  
    sudo ln -s /System/Library/Frameworks/Python.framework/Versions/3.6/bin/python3.6 /usr/bin/python  
    sudo ln -s /System/Library/Frameworks/Python.framework/Versions/3.6/bin/pythonw3.6 /usr/bin/pythonw  
    sudo ln -s /System/Library/Frameworks/Python.framework/Versions/3.6/bin/python3.6m-config /usr/bin/python-config  
    
    
### 第八步 配置全局环境

修改`bash_profile`文件的*Python*路径。

打开`bash_profile`

    vim ~/.bash_profile
将目录

    /Library/Frameworks/Python.framework/Versions/3.6/bin:${PATH}
    
改为目录

    /System/Library/Frameworks/Python.framework/Versions/3.6/bin:${PATH}
    
然后使`bash_profile`生效

    source .bash_profile
    
    
### 第九步 卸载Python 3.6安装的两个软件

### 第十步 检测

    $python
    Python 3.6.2 (v3.6.2:5fd33b5926, Jul 16 2017, 20:11:06)
    [GCC 4.2.1 (Apple Inc. build 5666) (dot 3)] on darwin
    Type "help", "copyright", "credits" or "license" for more information.
    >>>
