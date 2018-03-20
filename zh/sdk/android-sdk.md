# Android SDK开发文档

本文档旨在说明如何使用LumiSDK进行网关设备快联入网的开发，仅用于Android手机APP应用。

&nbsp;

## 前期准备

1、访问并登录AIOT开放平台网站，创建新应用后，在“应用管理”-“应用概览”页面获取“AppId”和“AppKey”。

2、根据[云端开发手册](http://docs.opencloud.aqara.cn/development/cloud-development/)中OAuth 2.0章节，获取openID。

3、下载[Android SDK](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/sdk/lumi_Android_SDK_v0.3.zip)，并解压，在LHSDKLib文件夹下可找到LumiSDK.aar库；在LHSDKDemo文件夹可查看Demo。

4、下载并安装Andriod Studio或其他集成开发环境。

&nbsp;

## 使用说明

1、配置Andriod项目。（以Andriod Studio为例）

1）创建Android项目，在主菜单中选择File--New--New Module--Import .JAR/.AAR Package，选择LumiSDK.aar所在的路径，单击Finish完成。

2）重新编译工程，LumiSDK模块图标会变成带茶杯的文件夹，且自动生成一个.iml文件。

![模块](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/doc-images/zh/sdk/lumisdk.png)

3）在主菜单中选择File--Project Structure--app--Dependencies，单击右上角的绿色加号，选择Module Dependency，把LumiSDK的module添加进去。

![依赖](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/doc-images/zh/sdk/dependencies.png)

4）配置src/main/AndroidManifest.xml，为项目添加联网权限。如不配置，则无法完成授权操作。

```
<uses-permission android:name="android.permission.INTERNET"/>
```

&nbsp;

2、网关入网操作说明。

1）手机连接外网，调用授权接口（aiotAuth），进行授权，返回授权结果。

授权接口：**LumiSDK.aiotAuth(appID,appKey,openID)**

| 参数     | 说明               |
| ------ | ---------------- |
| appID  | 第三方应用ID，AppID    |
| appKey | 第三方应用秘钥，AppKey   |
| openID | 通过OAuth授权获得的用户ID |

2）授权成功后，长按网关按钮至指示灯为黄色闪烁，且语音提示”等待连接中“，网关会创建一个以”lumi-gateway-xxxx“开头的热点。

3）手机Wi-Fi连接此网关热点，调用入网连接接口（gatewayFastLink），等待网关语音提示”连接成功“。

> 注意：网关成功入网需要一点时间，请耐心等待30s。

入网连接接口：**LumiSDK.gatewayFastLink(ssid, passwd, lang, positionId, positionType, longitude, latitude)**

| 参数           | 是否必须 | 说明               |
| ------------ | ---- | ---------------- |
| ssid         | 是    | 网关需加入的外网网络WIFI名称 |
| passwd       | 是    | 对应的WIFI密码        |
| lang         | 否    | 语言设置，默认中文zh-CN   |
| postionId    | 否    | 位置ID             |
| positionType | 否    | 位置类型             |
| longitude    | 否    | 设备所在位置的经度        |
| latitude     | 否    | 设备所在位置的纬度        |

> 注意：1、系统不识别有特殊字符的Wi-Fi，若Wi-Fi包含特殊字符，请先更换Wi-Fi名称或更换其他Wi-Fi尝试。
>
> 2、由于SDK中的网络请求是同步的，所以调用时必须在非UI线程中调用，具体可参考DEMO。
>
> 3、调用接口时，可能获得正确或错误的返回结果，可参考[云端开发手册](http://docs.opencloud.aqara.cn/development/cloud-development/)中“返回码说明”章节排查错误。

&nbsp;

## Demo示例

下载[Android SDK](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/sdk/lumi_Android_SDK_v0.3.zip)，并解压，在LHSDKDemo文件夹可查看该demo，本示例介绍如何使用SDK调用接口完成网关入网。

```
import android.graphics.Color;
import android.graphics.PorterDuff;
import android.os.Bundle;
import android.os.Handler;
import android.os.Message;
import android.support.v7.app.AppCompatActivity;
import android.util.Log;
import android.view.View;
import android.widget.EditText;
import android.widget.ImageView;
import android.widget.Toast;
import LumiSDK.LumiSDK;   

public class MainActivity extends AppCompatActivity {
    private ImageView mAuthStatues;
    private EditText mEtSSID;
    private EditText mEtPwd;
    
    private final String appID = "";  //第三方应用ID
    private final String appKey = "";  //第三方应用KEY
    private final String openID = "";  //通过OAuth授权获得的用户ID

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        mAuthStatues = (ImageView) findViewById(R.id.iv_auth_statues);
        mEtSSID = (EditText) findViewById(R.id.et_ssid);   //获取用户界面输入的网络wifi名称
        mEtPwd = (EditText) findViewById(R.id.et_pwd);    //获取用户界面输入的网络wifi密码
    }

    private Handler handler = new Handler() {
        @Override
        public void handleMessage(Message msg) {
            super.handleMessage(msg);
            showInfoByToast("授权结果：" + (boolean) msg.obj);
            if ((boolean) msg.obj) {
                mAuthStatues.setColorFilter(Color.GREEN, PorterDuff.Mode.SRC_IN);
            } else {
                mAuthStatues.setColorFilter(Color.GRAY, PorterDuff.Mode.SRC_IN);
            }
        }
    };

    public void auth(View view) {

        new Thread(new Runnable() {
            @Override
            public void run() {
                boolean result = false;
                try {
                    result = LumiSDK.aiotAuth(appID, appKey, openID);   //调用授权接口
                } catch (Exception e) {
                    e.printStackTrace();
                    return;
                }
                Log.d("BIU", "auth result --->>:" + result);
                Message msg = handler.obtainMessage(110, result);
                msg.sendToTarget();

            }
        }).start();
    }

    public void fastLink(View view) {
        new Thread(new Runnable() {
            @Override
            public void run() {
                try {
                    LumiSDK.gatewayFastLink(mEtSSID.getText().toString(), mEtPwd.getText().toString(), "", "", "", "", "");     //调用入网连接接口
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }).start();
    }


    public void showInfoByToast(String info) {
        Toast.makeText(getApplicationContext(), info, Toast.LENGTH_SHORT).show();
    }
}
```