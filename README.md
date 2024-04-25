# 甲骨文实例抢购教程

## 获取程序
- 如果需要在本地电脑运行该程序，根据自己电脑的CPU架构类型，打开下面的 `下载地址` 下载对应的文件即可。如果需要在服务器上运行该程序，那么根据服务器的CPU架构类型选择对应的文件下载到服务器即可。

- 下载完成后解压压缩包，可以得到可执行程序 (Windows系统: `oci-help.exe` , 其他操作系统: `oci-help` ) 和 配置文件 `oci-help.ini` 。

> 下载地址: https://github.com/lemoex/oci-help/releases/latest


## 获取配置信息
![image](https://github.com/lemoex/oci-help/raw/main/doc/1.png)
![image](https://github.com/lemoex/oci-help/raw/main/doc/2.png)
![image](https://github.com/lemoex/oci-help/raw/main/doc/3.png)
![image](https://github.com/lemoex/oci-help/raw/main/doc/4.png)
![image](https://github.com/lemoex/oci-help/raw/main/doc/5.png)
![image](https://github.com/lemoex/oci-help/raw/main/doc/6.png)


## 编辑配置文件
用文本编辑器打开在第一步获取到的 `oci-help.ini` 文件，进行如下配置:

![image](https://github.com/lemoex/oci-help/raw/main/doc/7.png)
![image](https://github.com/lemoex/oci-help/raw/main/doc/8.png)

## Telegram 消息提醒配置
![image](https://github.com/lemoex/oci-help/raw/main/doc/9.png)

> BotFather: https://t.me/BotFather    
> IDBot: https://t.me/myidbot


## 在R2S中运行程序
```
# 创建目标目录
sudo mkdir -p /root/oci_help

# 进入目标目录
cd /root/oci_help

# 使用 wget 下载文件
wget "https://github.com/lemoex/oci-help/releases/download/v2.3.1/oci-help-linux-arm64-v2.3.1.zip" -O oci-help.zip

# 解压文件, 上传pem文件, 并编辑配置文件
unzip oci-help.zip

# 安装screen, 编辑screen脚本
vi /root/oci_help/run-oci-help.sh
------------------------------------
#!/bin/sh
# 这个脚本将在一个新的 screen 会话中启动 oci-help 交互式程序，并通过命令行参数指定配置文件路径

# 在 screen 会话中启动 oci-help 程序，直接指定配置文件路径
screen -dmS oci-help-session /root/oci_help/oci-help -config /root/oci_help/oci-help.ini

# 显示启动消息
echo "启动了 screen 会话 'oci-help-session'。使用 'screen -r oci-help-session' 来访问会话。"
------------------------------------
# 赋予screen脚本权限
chmod +x run-oci-help.sh

# 新建脚本, 设置开机自启与自运行screen脚本
vi /etc/init.d/oci-help
------------------------------------
#!/bin/sh /etc/rc.common

START=99  # The start priority
STOP=10   # The stop priority

start() {
    echo "Starting OCI-Help..."
    /root/oci_help/run-oci-help.sh
}

stop() {
    echo "Stopping OCI-Help..."
    killall -9 oci-help  # Adjust the process name if needed
}
------------------------------------
# 赋予自启脚本权限
chmod +x /etc/init.d/oci-help
# 设置自启
/etc/init.d/oci-help enable
# 启动脚本
/etc/init.d/oci-help start
# 停止脚本
/etc/init.d/oci-help stop
```

## 如果想离开 screen 而不终止运行的程序：
使用 Ctrl+A，然后按 D。这个操作会分离 screen 会话，将其留在后台运行，而你则返回到你的原始 shell。这样可以让 oci-help 或任何其他程序继续在 screen 会话中运行，而不会被中断。
