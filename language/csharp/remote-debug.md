# [遠端偵錯](https://docs.microsoft.com/zh-tw/visualstudio/debugger/remote-debugging-azure?view=vs-2017)

## with IIS

1. 下載 Visual Studio 2017 遠端工具

    [x64][vs2017-x64-remote-tool-download]
    [x86][vs2017-x86-remote-tool-download]
    [other][other-download]

2. 在 windows 上安裝/設定遠端偵錯
    **需要系統管理權限**
    ![image][remote-debugger-conf-wizard-page]

    設定完後
    ![image][remote-debugger-window]
    **預設使用port: 4022**

3. 從vs2017連到IIS進行偵錯

    pending

## with Azure

[vs2017-x64-remote-tool-download]: https://aka.ms/vs/15/release/RemoteTools.amd64ret.cht.exe
[vs2017-x86-remote-tool-download]:https://aka.ms/vs/15/release/RemoteTools.x86ret.cht.exe
[other-download]: https://visualstudio.microsoft.com/zh-hant/downloads/
[remote-debugger-conf-wizard-page]: ../../images/remotedebuggerconfwizardpage.png
[remote-debugger-window]: ../../images/remotedebuggerwindow.png
