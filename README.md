简介：

抖音/Tiktok/头条 加密、签名算法研究

# 加密算法
## 设备注册：/service/2/device_register/
设备注册请求解密后  
#``` 
{
    "magic_tag": "ss_app_log",
    "header": {
        "display_name": "抖音短视频",
        "update_version_code": 11009900,
        "manifest_version_code": 110001,
        "app_version_minor": "",
        "aid": 1128,
        "channel": "xiaomi",
        "appkey": "57bfa27c67e58e7d923328d3",
        "package": "com.ss.android.ugc.aweme",
        "app_version": "11.0.0",
        "version_code": 110000,
        "sdk_version": "2.10.1-rc.18-setIntervalThrowable",
        "sdk_target_version": 29,
        "git_hash": "de5f3e3f",
        "os": "Android",
        "os_version": "6.0.1",
        "os_api": 23,
        "device_model": "MI 4W",
        "device_brand": "Xiaomi",
        "device_manufacturer": "Xiaomi",
        "cpu_abi": "armeabi-v7a",
        "build_serial": "63672594",
        "release_build": "5f51528_20200507_739734d2-9058-13ea-8899-02420a000052",
        "density_dpi": 480,
        "display_density": "mdpi",
        "resolution": "1920x1080",
        "language": "zh",
        "mc": "0C:1D:AF:DF:4E:58",
        "timezone": 8,
        "access": "wifi",
        "not_request_sender": 0,
        "rom": "MIUI-7.3.23",
        "rom_version": "miui_V8_7.3.23",
        "cdid": "eaeb1c10-8931-405f-88e4-f2f47ad48780",
        "sig_hash": "aea615ab910015038f73c47e45d21466",
        "google_aid": "1d820b1b-3119-497e-b4d0-b39431b86f57",
        "device_id": "4441663973901047",
        "openudid": "3182b142d722ab5f",
        "udid": "865291585084194",
        "clientudid": "59c3962d-202c-4bcd-8150-43db55cc5d15",
        "serial_number": "63672594",
        "sim_serial_number": [],
        "region": "CN",
        "tz_name": "Asia/Shanghai",
        "tz_offset": 28800,
        "oaid_may_support": false,
        "req_id": "479b202f-9815-4f02-8565-687caac402f3",
        "custom": {
            "filter_warn": 0,
            "web_ua": "Mozilla/5.0 (Linux; Android 6.0.1; MI 4W Build/MMB29M; wv) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/55.0.2883.91 Mobile Safari/537.36"
        },
        "apk_first_install_time": 1589461294742,
        "is_system_app": 0,
        "sdk_flavor": "china"
    },
    "_gen_time": 1589461488220
}
#```   
## xlog加密算法
xlog的算法在libcms.so中，在JNI_Onload中动态注册jni函数。算法用vm protect，和混淆，主要是流程平坦化，流程混淆和运算替换，看起来比较困难。


# 签名
## 抖音04/84开头xgorgon
主要是X-Gorgon和X-SS-STUB.之后经过抓包抖音接口，查看Java层,so层代码。  
X-SS-STUB是post请求时body部分的md5值，但是在为空的情况下，有时候不参与加密，有时候参与加密，具体接口需要具体分析  
X-Khronos比较简单就是一个unix时间戳.  
X-Gorgon是对cookie,X-SS-STUB,X-Khronos,Url进行混合加密之后的参数。这里也区分情况，有些接口只有url和X-Khronos参与接口加密，有些是url，X-Khronos，X-SS-STUB参与接口加密，有些则是所有都进行接口加密。
