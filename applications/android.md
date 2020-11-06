# Android

## 通過adb shell命令切換手機的輸入法

```sh
# 啟用
$ ime enabled com.google.android.apps.inputmethod.zhuyin/.ZhuyinInputMethodService

＃
$ ime enabled com.google.android.apps.inputmethod.zhuyin/.ZhuyinInputMethodService

# 設定
ime enabled com.google.android.apps.inputmethod.zhuyin/.ZhuyinInputMethodService
```

## 給予/拒絕權限

```sh
adb shell pm grant com.name.app android.permission.READ_PROFILE

adb shell pm revoke com.name.app android.permission.READ_PROFILE
```

## adb 設定語系/語言

```sh
adb shell am start -a android.settings.LOCALE_SETTINGS
```
