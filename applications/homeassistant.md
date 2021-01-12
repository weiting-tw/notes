# Home Assistant

## [Install](https://www.home-assistant.io/getting-started/)

## Connect to Wifi

確認 wifi 已啟用

> nmcli radio

```bash
WIFI-HW  WIFI     WWAN-HW  WWAN
enabled  enabled  enabled  enabled
```

掃描

> nmcli device wifi rescan

取得清單

> nmcli device wifi list

```bash
IN-USE  SSID           MODE   CHAN  RATE        SIGNAL  BARS  SECURITY  
        weiting-guest  Infra  8     270 Mbit/s  100     ▂▄▆█  WPA2      
*       weiting        Infra  8     270 Mbit/s  97      ▂▄▆█  WPA2      
        Vantist-2.4G   Infra  1     195 Mbit/s  92      ▂▄▆█  WPA2      
        AndRiver       Infra  11    260 Mbit/s  82      ▂▄▆█  WPA2      
        AndRiver       Infra  11    260 Mbit/s  74      ▂▄▆_  WPA2      
        hochung5f      Infra  4     405 Mbit/s  50      ▂▄__  WPA1 WPA2 
        鄭月櫻         Infra  11    130 Mbit/s  50      ▂▄__  WPA1 WPA2 
        hochung        Infra  4     270 Mbit/s  37      ▂▄__  WPA1 WPA2 
        --             Infra  4     405 Mbit/s  34      ▂▄__  WPA2      
        FLOWER         Infra  1     270 Mbit/s  12      ▂___  WPA1 WPA2
```

連線

> nmcli device wifi connect 'SSID' password 'PASSWORD'

確認連線

> nmcli con show

刪除連線

> nmcli connection delete CONNECTION\_NAME

連線資訊

> ip addr show

