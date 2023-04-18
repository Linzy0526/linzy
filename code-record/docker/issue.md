# Docker Issue

> 记录使用docker中遇到问题和报错时的解决方案

### 1.安装完成docker，打开docker报错(An unexpected error was encountered while executing a WSL command)

#### 报错信息
 `An unexpected error was encountered while executing a WSL command. Common causes include access rights issues, which occur after waking the computer or not being connected to your domain/active directory.`

#### 安装环境
- Windows Version: Windows 11
- Docker Desktop Version: 4.18.0 (10412)

#### 解决

#### 重置一下Windows网络设置
可能系统用了Vpn导致网络设置代理异常，重置一下Windows网络设置，并重启系统即可

##### 检查 WSL 的状态
开 PowerShell 并输入以下命令来检查 WSL 的状态：  
```
wsl -l -v
```  
如果 WSL 的状态不是“运行中”，可以尝试启动它。在 PowerShell 中输入以下命令：
```
wsl --shutdown
wsl
```

##### 重置 WSL
如果上述方法都没有解决问题，可以尝试重置 WSL。在 PowerShell 中输入以下命令：
```
dism.exe /online /cleanup-image /restorehealth
```
接着再输入以下命令：
```
sfc /scannow
```
然后重启电脑。

