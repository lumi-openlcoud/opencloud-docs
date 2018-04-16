# 配置

当第一次启动Home Assistant时，会自动创建一个配置文件。在不同的操作系统中，配置文件的目录也是不同的。

| OS      | Path                        |
| ------- | --------------------------- |
| macOS   | ` ~/.homeassistant`         |
| Linux   | `~/.homeassistant`          |
| Windows | ` %APPDATA%/.homeassistant` |

如果想更改配置文件的目录，可用此命令来修改：`hass --config path/to/config`。

在配置文件夹中包含Home Assistant的主配置文件：configuration.yaml。该配置文件各部分内容说明在下文一一讲述。如果修改了该文件，需要重启Home Assistant使其生效。



## 基础信息配置

默认情况下，Home Assistant会自动获取您所在的位置、IP地址、时区等信息。您也可以在configuration.yaml中修改这些信息：

```
homeassistant:
  name: Home
  latitude: 22.5333
  longitude: 114.1333
  elevation: 0
  unit_system: metric
  time_zone: Asia/Shanghai
  customize: !include customize.yaml
```

参数说明如下：

|        参数        | 是否必须 |                    说明                    |
| :--------------: | :--: | :--------------------------------------: |
|       name       |  可选  |         Home Assistant所在的空间位置名称          |
|     latitude     |  可选  |                    纬度                    |
|    longitude     |  可选  |                    经度                    |
|    elevation     |  可选  |                 海拔高度（米）                  |
|   unit_system    |  可选  |       度量衡单位，metric：公制，imperial：英制        |
|    time_zone     |  可选  | 时区，可参考[时区表](http://en.wikipedia.org/wiki/List_of_tz_database_time_zones) |
|    customize     |  可选  |                  自定义实体                   |
| customize_domain |  可选  |               自定义一个域内的所有实体               |
|  customize_glob  |  可选  |               自定义匹配某种模式的实体               |



## HTTP配置

默认情况下，在configuration.yaml文件中`http`模块配置信息为空。安装Home Assistant后，请先设置访问密码，否则任何人都可以访问你的Home Assistant。其他参数可在需要时再设置。

```
http:
  api_password: YOUR_PASSWORD
```



各参数说明如下：

|            参数            | 是否必须 |                    说明                    |
| :----------------------: | :--: | :--------------------------------------: |
|       api_password       |  可选  |          设置访问Home Assistant的密码。          |
|       server_host        |  可选  | 仅监听发送到指定IP/host的网络请求。缺省默认为0.0.0.0，指接收所有IPv4的连接请求。若设置为“::0”，指仅接收IPv4的所有请求。 |
|       server_port        |  可选  |              服务器端口，默认为8123。              |
|         base_url         |  可选  |      访问Home Assistant的地址。默认为本地IP地址。      |
|     ssl_certificate      |  可选  |           安全连接时使用的TLS/SSL证书地址。           |
|         ssl_key          |  可选  |           安全连接时使用的TLS/SSL密钥地址。           |
|   cors_allowed_origins   |  可选  |            允许来自CORS请求的原始域名列表。            |
|   use_x_forwarded_for    |  可选  | 接受header的“X-Forwarded-For”信息。建议在安全的网络环境中进行设置。默认是False。 |
|     trusted_networks     |  可选  | 可信任的IP和网络列表，允许列表中的客户可以不需要密码访问Home Assistant。如果使用了反向代理，所有Home Assistant的请求都会来自反向代理的地址，所以在这种情况下，设置要格外小心。 |
|      ip_ban_enabled      |  可选  |            是否开启IP过滤。默认为False。            |
| login_attempts_threshold |  可选  | 在ip_ban_enabled为true的情况下，单个IP尝试登录的失败次数。默认为-1，表示允许次数无穷大。 |

配置示例如下：

```
http:
  api_password: YOUR_PASSWORD
  server_port: 12345
  ssl_certificate: /etc/letsencrypt/live/hass.example.com/fullchain.pem
  ssl_key: /etc/letsencrypt/live/hass.example.com/privkey.pem
  cors_allowed_origins:
    - https://google.com
    - https://www.home-assistant.io
  use_x_forwarded_for: True
  trusted_networks:
    - 127.0.0.1
    - ::1
    - 192.168.0.0/24
    - 2001:DB8:ABCD::/48
  ip_ban_enabled: True
  login_attempts_threshold: 5
```



## 验证配置文件

配置完成后，可使用如下方法验证配置文件的有效性：

- 输入命令：`hass --script check_config`。

- 访问Home Assistant的Web页面，在“配置”-“通用”中，单击“检查配置”。


