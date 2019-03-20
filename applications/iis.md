# IIS

## IIS Crypto Tool

IIS Crypto is a free tool that gives administrators the ability to enable or disable protocols, ciphers, hashes and key exchange algorithms on Windows Server 2008, 2012, 2016 and 2019. It also lets you reorder SSL/TLS cipher suites offered by IIS, change advanced settings, implement Best Practices with a single click, create custom templates and test your website.

[Download](https://www.nartac.com/Products/IISCrypto/Download)

## 讓 IIS 網站保持運作狀態

1. 安裝「應用程式初始化(Application Initialization)」
    ![image](../images/iis/setting-with-install.png)
2. 網站的進階設定「預先載入已啟用」設為 True
    ![image](../images/iis/website-setting.png)
3. 應用程式集區的進階設定「啟動模式」設為 True
    ![image](../images/iis/application-setting-alwaysrunning.png)

## 讓 IIS Express 外部存取

1. 使用 netsh 以本機 IP 及 站台 port 加入 URL 保留區( access control list)

    ```sh
    netsh http add urlacl url=http://10.211.55.3:49486/ user=everyone
    ```

    刪除

    ```sh
    netsh http delete urlacl url=http://10.211.55.3:49486
    ```

2. 修改專案底下 `.vs/config`

    ```diff
        <site name="GSS.Duck.WebApi" id="2">
            <application path="/" applicationPool="Clr4IntegratedAppPool">
                <virtualDirectory path="/" physicalPath="Z:\Workspace\BizForm\duck\src\GSS.Duck.WebApi" />
            </application>
            <bindings>
                <binding protocol="http" bindingInformation="*:49486:localhost" />
    +           <binding protocol="http" bindingInformation="*:49486:10.211.55.4" />
            </bindings>
        </site>
    ```

3. 開啟防火牆

    ``` sh
    netsh advfirewall firewall add rule name="Demo IIS Express" protocol=TCP dir=in localport=49486 action=allow
    ```
