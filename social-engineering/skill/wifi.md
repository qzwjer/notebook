
windows

# 图形
控制面板-》网络和共享中心-》WLAN(wifiname)-》无线属性-》安全


# 命令行
## netsh wlan show profiles

```
Profiles on interface WLAN:

Group policy profiles (read only)
---------------------------------
    <None>

User profiles
-------------
    All User Profile     : ChinaNet-8207
    All User Profile     : ChinaNet-Kdwq
    All User Profile     : QHWP8604
    All User Profile     : iPhone
    All User Profile     : HUAWEI Mate 20 Pro
    All User Profile     : CPTC_AX6000_2.4G
    All User Profile     : CC-VnHE
    All User Profile     : qzwjer2
    All User Profile     : ChinaNet-Kdwq-5G
    All User Profile     : qzwjer
    All User Profile     : 8410
    All User Profile     : xxckkz
    All User Profile     : ChinaNet-cZtp-5G
    All User Profile     : ChinaNet-cZtp
```


## netsh wlan show profile name=xxckkz key=clear

key content

```

Profile xxckkz on interface WLAN:
=======================================================================

Applied: All User Profile

Profile information
-------------------
    Version                : 1
    Type                   : Wireless LAN
    Name                   : xxckkz
    Control options        :
        Connection mode    : Connect automatically
        Network broadcast  : Connect only if this network is broadcasting
        AutoSwitch         : Do not switch to other networks
        MAC Randomization  : Disabled

Connectivity settings
---------------------
    Number of SSIDs        : 1
    SSID name              : "xxckkz"
    Network type           : Infrastructure
    Radio type             : [ Any Radio Type ]
    Vendor extension          : Not present

Security settings
-----------------
    Authentication         : WPA2-Personal
    Cipher                 : CCMP
    Authentication         : WPA2-Personal
    Cipher                 : GCMP
    Security key           : Present
    Key Content            : 15202917727

Cost settings
-------------
    Cost                   : Unrestricted
    Congested              : No
    Approaching Data Limit : No
    Over Data Limit        : No
    Roaming                : No
    Cost Source            : Default
```