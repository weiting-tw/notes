# IIS

## 讓 IIS 網站保持運作狀態

1. 安裝「應用程式初始化(Application Initialization)」
    ![image](../images/iis/setting-with-install.png)
2. 網站的進階設定「預先載入已啟用」設為 True
    ![image](../images/iis/website-setting.png)
3. 應用程式集區的進階設定「啟動模式」設為 True
    ![image](../images/iis/application-setting-alwaysrunning.png)