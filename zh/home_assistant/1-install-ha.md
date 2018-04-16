# 安装

## 在树莓派中安装Home Assistant

在进行以下操作前，请先确保树莓派已安装系统，且接入电源和路由器。本文以 [Raspbian Lite](https://www.raspberrypi.org/downloads/raspbian/)，且在虚拟环境安装为例。

1. 通过putty的SSH连接树莓派。

2. 更新系统。

   ```
   $ sudo apt-get update
   $ sudo apt-get upgrade -y
   ```

3. Raspbian系统自带有Python，可跳过此步骤。若没有，可按照以下命令安装Python。

   `$ sudo apt-get install python<版本号>`

   安装后，可通过以下命令验证Python版本。

   `$ python --version`

4. 安装依赖包。

   `$ sudo apt-get install python3 python3-venv python3-pip`

5. 创建名为“homeassistant”的账户。

   ```
   $ sudo useradd -rm homeassistant
   ```

6. 创建Home Assistant的安装目录，并切换到“homeassistant”的账户下。

   ```
   $ cd /srv
   $ sudo mkdir homeassistant
   $ sudo chown homeassistant:homeassistant homeassistant
   ```

7. 在此账户下创建一个虚拟环境。

   ```
   $ sudo su -s /bin/bash homeassistant
   $ cd /srv/homeassistant
   $ python3 -m venv .
   $ source bin/activate
   ```

8. 虚拟环境激活后，安装需要的Python包。

   ```
   $ python3 -m pip install wheel
   ```

9. 安装Home Assistant。

   `$ pip3 install homeassistant`

10. 启动Home Assistant。

  `$ hass --open-ui`，启动成功后，浏览器将自动打开Home Assistant的Web页面。

  地址为：http://youripaddress:8123

  > 说明：如果是第一次运行`hass`命令，会自动创建一个配置文件（其中包含通过IP推测的经纬度、时区信息，http访问，自动发现设备、sun实体、天气预报等配置内容），另外会自动下载和安装需要的库和依赖，可能会耗费一些时间，请耐心等待。



![](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/doc-images/zh/home-assistant/install-0.png)



## 更新

通过以下命令可更新Home Assistant到最新版本。

```
$ sudo su -s /bin/bash homeassistant
$ source /srv/homeassistant/bin/activate
$ pip3 install --upgrade homeassistant
```

命令执行后，重启Home Assistant服务即可。



## 自启动

自启动服务可以在局域网环境下任意客户端访问Home Assistant的Web页面。树莓派的发行版（除手动安装外）已经自带自启动服务。针对树莓派手动安装和其他系统安装的用户需要自行配置自启动服务。

**步骤一：**创建自启动服务配置文件

- **Home Assistant 运行在 Python 虚拟环境中。**

  按照以下命令创建文件：

  ```
  sudo nano -w /etc/systemd/system/homeassistant.service
  ```

  输入以下内容，可复制粘贴：

  ```
  [Unit]
  Description=Home Assistant
  After=network-online.target

  [Service]
  Type=simple
  User=homeassistant
  ExecStart=/srv/homeassistant/bin/hass -c "/home/homeassistant/.homeassistant"

  [Install]
  WantedBy=multi-user.target
  ```

  同时按“Ctrl+X”，提示是否保存，输入“Y”，再按回车，文件创建并保存。


- **未使用Python虚拟环境安装Home Assistant。**

  按照以下命令创建文件：

  ```
  sudo nano -w /etc/systemd/system/homeassistant.service
  ```

  输入以下内容，可复制粘贴：

  ```
  [Unit]
  Description=Home Assistant
  After=network-online.target

  [Service]
  Type=simple
  User=%i
  ExecStart=/usr/bin/hass

  [Install]
  WantedBy=multi-user.target
  ```

  同时按“Ctrl+X”，提示是否保存，输入“Y”，再按回车，文件创建并保存。

> 注意：若用户名和home assistant安装路径不同，请修改User和ExecStart。

**步骤二：**启动配置

1. 重新加载systemd进程来发现新配置文件：

   ```
   $ sudo systemctl --system daemon-reload
   ```

2. 输入以下命令启动服务来使Home Assistant自动启动：

   ```
   $ sudo systemctl enable homeassistant.service
   ```

   如果想禁用自启动服务，可使用以下命令：

   ```
   $ sudo systemctl disable homeassistant.service
   ```

3. 输入以下命令启动Home Assistant：

   ```
   $ sudo systemctl start homeassistant.service
   ```

