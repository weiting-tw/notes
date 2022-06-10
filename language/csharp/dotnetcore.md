# .Net Core

## [HTTPS 認證](https://docs.microsoft.com/zh-tw/aspnet/core/security/enforcing-ssl?view=aspnetcore-3.0&tabs=visual-studio#trust-https-certificate-from-windows-subsystem-for-linux)

```bash
dotnet dev-certs https --clean
dotnet dev-certs https --trust
```

## [Windows Defender Exclusions](https://gist.github.com/nerzhulart/89c6a376b521a6e7eb69a04277a9489a)

```ps1
$userPath = $env:USERPROFILE
$pathExclusions = New-Object System.Collections.ArrayList
$processExclusions = New-Object System.Collections.ArrayList

$pathExclusions.Add('C:\Windows\Microsoft.NET') > $null
$pathExclusions.Add('C:\Windows\assembly') > $null
$pathExclusions.Add($userPath + '\AppData\Local\Microsoft\VisualStudio') > $null
$pathExclusions.Add('C:\ProgramData\Microsoft\VisualStudio\Packages') > $null
$pathExclusions.Add('C:\Program Files (x86)\MSBuild') > $null
$pathExclusions.Add('C:\Program Files (x86)\Microsoft Visual Studio 14.0') > $null
$pathExclusions.Add('C:\Program Files (x86)\Microsoft Visual Studio 10.0') > $null
$pathExclusions.Add('C:\Program Files (x86)\Microsoft Visual Studio') > $null
$pathExclusions.Add('C:\Program Files (x86)\Microsoft SDKs\NuGetPackages') > $null
$pathExclusions.Add('C:\Program Files (x86)\Microsoft SDKs') > $null

# VS
$processExclusions.Add('vshost-clr2.exe') > $null
$processExclusions.Add('VSInitializer.exe') > $null
$processExclusions.Add('VSIXInstaller.exe') > $null
$processExclusions.Add('VSLaunchBrowser.exe') > $null
$processExclusions.Add('vsn.exe') > $null
$processExclusions.Add('VsRegEdit.exe') > $null
$processExclusions.Add('VSWebHandler.exe') > $null
$processExclusions.Add('VSWebLauncher.exe') > $null
$processExclusions.Add('XDesProc.exe') > $null
$processExclusions.Add('Blend.exe') > $null
$processExclusions.Add('DDConfigCA.exe') > $null
$processExclusions.Add('devenv.exe') > $null
$processExclusions.Add('FeedbackCollector.exe') > $null
$processExclusions.Add('Microsoft.VisualStudio.Web.Host.exe') > $null
$processExclusions.Add('mspdbsrv.exe') > $null
$processExclusions.Add('MSTest.exe') > $null
$processExclusions.Add('PerfWatson2.exe') > $null
$processExclusions.Add('Publicize.exe') > $null
$processExclusions.Add('QTAgent.exe') > $null
$processExclusions.Add('QTAgent_35.exe') > $null
$processExclusions.Add('QTAgent_40.exe') > $null
$processExclusions.Add('QTAgent32.exe') > $null
$processExclusions.Add('QTAgent32_35.exe') > $null
$processExclusions.Add('QTAgent32_40.exe') > $null
$processExclusions.Add('QTDCAgent.exe') > $null
$processExclusions.Add('QTDCAgent32.exe') > $null
$processExclusions.Add('StorePID.exe') > $null
$processExclusions.Add('T4VSHostProcess.exe') > $null
$processExclusions.Add('TailoredDeploy.exe') > $null
$processExclusions.Add('TCM.exe') > $null
$processExclusions.Add('TextTransform.exe') > $null
$processExclusions.Add('TfsLabConfig.exe') > $null
$processExclusions.Add('UserControlTestContainer.exe') > $null
$processExclusions.Add('vb7to8.exe') > $null
$processExclusions.Add('VcxprojReader.exe') > $null
$processExclusions.Add('VsDebugWERHelper.exe') > $null
$processExclusions.Add('VSFinalizer.exe') > $null
$processExclusions.Add('VsGa.exe') > $null
$processExclusions.Add('VSHiveStub.exe') > $null
$processExclusions.Add('vshost.exe') > $null
$processExclusions.Add('vshost32.exe') > $null
$processExclusions.Add('vshost32-clr2.exe') > $null

# VS Code
$processExclusions.Add('Code - Insiders.exe') > $null

# Runtimes, build tools
$processExclusions.Add('dotnet.exe') > $null
$processExclusions.Add('mono.exe') > $null
$processExclusions.Add('mono-sgen.exe') > $null
$processExclusions.Add('java.exe') > $null
$processExclusions.Add('java64.exe') > $null
$processExclusions.Add('msbuild.exe') > $null
$processExclusions.Add('node.exe') > $null
$processExclusions.Add('node.js') > $null
$processExclusions.Add('perfwatson2.exe') > $null
$processExclusions.Add('ServiceHub.Host.Node.x86.exe') > $null
$processExclusions.Add('vbcscompiler.exe') > $null
$processExclusions.Add('nuget.exe') > $null

# VCS
$processExclusions.Add('git.exe') > $null

# Shells
$processExclusions.Add('git-bash.exe') > $null
$processExclusions.Add('bash.exe') > $null
$processExclusions.Add('powershell.exe') > $null

# All of JetBrains stuff
$processExclusions.Add('JetBrains.EntityFramework.Runner620.exe') > $null
$processExclusions.Add('JetBrains.MsBuild.TaskEntryPoint.exe') > $null
$processExclusions.Add('JetBrains.Platform.Satellite.exe') > $null
$processExclusions.Add('JetBrains.ReSharper.Features.XamlPreview.External.exe') > $null
$processExclusions.Add('JetBrains.ReSharper.Host.exe') > $null
$processExclusions.Add('JetBrains.ReSharper.Host64.exe') > $null
$processExclusions.Add('JetBrains.ReSharper.Roslyn.Worker.exe') > $null
$processExclusions.Add('JetLauncher32.exe') > $null
$processExclusions.Add('JetLauncher32c.exe') > $null
$processExclusions.Add('JetLauncher64.exe') > $null
$processExclusions.Add('JetLauncher64c.exe') > $null
$processExclusions.Add('JetLauncherIL.exe') > $null
$processExclusions.Add('JetLauncherILc.exe') > $null
$processExclusions.Add('OperatorsResolveCacheGenerator.exe') > $null
$processExclusions.Add('PsiGen.exe') > $null
$processExclusions.Add('ReSharperTestRunner32.exe') > $null
$processExclusions.Add('ReSharperTestRunner64.exe') > $null
$processExclusions.Add('ReSharperTestRunnerIL.exe') > $null
$processExclusions.Add('RiderClrProcessEnumerator32.exe') > $null
$processExclusions.Add('RiderClrProcessEnumeratorIL.exe') > $null
$processExclusions.Add('TokenGenerator.exe') > $null
$processExclusions.Add('xamarin-component.exe') > $null
$processExclusions.Add('ClrStack.x64.exe') > $null
$processExclusions.Add('ClrStack.x86.exe') > $null
$processExclusions.Add('CsLex.exe') > $null
$processExclusions.Add('ErrorsGen.exe') > $null
$processExclusions.Add('JetBrains.Debugger.Worker.exe') > $null
$processExclusions.Add('JetBrains.Debugger.Worker32c.exe') > $null
$processExclusions.Add('JetBrains.Debugger.Worker64c.exe') > $null
$processExclusions.Add('dotPeek32.exe') > $null
$processExclusions.Add('dotPeek64.exe') > $null
$processExclusions.Add('DotTabWellScattered32.exe') > $null
$processExclusions.Add('DotTabWellScattered64.exe') > $null
$processExclusions.Add('DotTabWellScatteredIL.exe') > $null
$processExclusions.Add('JetBrains.Platform.Installer.Bootstrap.exe') > $null
$processExclusions.Add('JetBrains.Platform.Installer.Cleanup.exe') > $null
$processExclusions.Add('JetBrains.Platform.Installer.exe') > $null
$processExclusions.Add('CleanUpProfiler.x64.exe') > $null
$processExclusions.Add('CleanUpProfiler.x86.exe') > $null
$processExclusions.Add('Configuration2Xml32.exe') > $null
$processExclusions.Add('Configuration2Xml64.exe') > $null
$processExclusions.Add('ConsoleProfiler.exe') > $null
$processExclusions.Add('dotTrace32.exe') > $null
$processExclusions.Add('dotTrace64.exe') > $null
$processExclusions.Add('DotTraceLauncher.exe') > $null
$processExclusions.Add('dotTraceView32.exe') > $null
$processExclusions.Add('dotTraceView64.exe') > $null
$processExclusions.Add('JetBrains.Common.ElevationAgent.exe') > $null
$processExclusions.Add('JetBrains.Common.ExternalStorage.exe') > $null
$processExclusions.Add('JetBrains.Common.ExternalStorage.x86.exe') > $null
$processExclusions.Add('JetBrains.dotTrace.IntegrationDemo.exe') > $null
$processExclusions.Add('Reporter.exe') > $null
$processExclusions.Add('SnapshotStat.exe') > $null
$processExclusions.Add('Timeline32.exe') > $null
$processExclusions.Add('Timeline64.exe') > $null
$processExclusions.Add('dotMemory.UI.32.exe') > $null
$processExclusions.Add('dotMemory.UI.64.exe') > $null
$processExclusions.Add('dotMemoryUnit.exe') > $null
$processExclusions.Add('JetBrains.dotMemory.Console.SingleExe.exe') > $null
$processExclusions.Add('JetBrains.dotMemoryUnit.Server.exe') > $null
$processExclusions.Add('restarter.exe') > $null
$processExclusions.Add('rider64.exe') > $null
$processExclusions.Add('runnerw.exe') > $null
$processExclusions.Add('runnerw64.exe') > $null
$processExclusions.Add('WinProcessListHelper.exe') > $null
$processExclusions.Add('elevator.exe') > $null
$processExclusions.Add('fsnotifier.exe') > $null
$processExclusions.Add('fsnotifier64.exe') > $null
$processExclusions.Add('launcher.exe') > $null
$processExclusions.Add('NGen Rider Assemblies.exe') > $null
$processExclusions.Add('idea.exe') > $null
$processExclusions.Add('idea64.exe') > $null
$processExclusions.Add('JetBrains.Etw.Collector.Host.exe') > $null

# JB Toolbox
$processExclusions.Add('jetbrains-toolbox.exe') > $null
$processExclusions.Add('jetbrains-toolbox-cef.exe') > $null
$processExclusions.Add('jetbrains-toolbox-cef-helper.exe') > $null

# Unity
$processExclusions.Add('UnityHelper.exe') > $null
$processExclusions.Add('Unity.exe') > $null
$processExclusions.Add('UnityShaderCompiler.exe') > $null
$processExclusions.Add('UnityYAMLMerge.exe') > $null
$processExclusions.Add('UnityCrashHandler64.exe') > $null

Write-Host "This script will create Windows Defender exclusions for common Visual Studio 2017 folders and processes."
Write-Host ""
$projectsFolder = Read-Host 'What is the path to your Projects folder? (example: c:\projects)'

Write-Host ""
Write-Host "Adding Path Exclusion: " $projectsFolder
Add-MpPreference -ExclusionPath $projectsFolder

foreach ($exclusion in $pathExclusions) 
{
    Write-Host "Adding Path Exclusion: " $exclusion
    Add-MpPreference -ExclusionPath $exclusion
}

foreach ($exclusion in $processExclusions)
{
    Write-Host "Adding Process Exclusion: " $exclusion
    Add-MpPreference -ExclusionProcess $exclusion
}

Write-Host ""
Write-Host "Your Exclusions:"

$prefs = Get-MpPreference
$prefs.ExclusionPath
$prefs.ExclusionProcess

Write-Host ""
Write-Host "Enjoy faster build times and coding!"
Write-Host ""
```

快速執行指令

```powershell
iex ((New-Object System.Net.WebClient).DownloadString('https://gist.githubusercontent.com/nerzhulart/89c6a376b521a6e7eb69a04277a9489a/raw/Windows%2520Defender%2520Exclusions%2520for%2520Developer.ps1'))
```

## 使用 HttpClient 發送 multipart/form-data

出現參數錯誤問題

Postman

```body
Content-Type: multipart/form-data; boundary=--------------------------639275760242036520206377
Accept-Encoding: gzip, deflate
Content-Length: 566
Connection: keep-alive

----------------------------639275760242036520206377
Content-Disposition: form-data; name="mch_id"

1565111111
----------------------------639275760242036520206377
Content-Disposition: form-data; name="media_hash"

7215E92A8F3F3D0256484EFFF53A25F6
----------------------------639275760242036520206377
Content-Disposition: form-data; name="sign_type"

HMAC-SHA256
----------------------------639275760242036520206377
Content-Disposition: form-data; name="sign"

A1D8B094FA24BE5531D1AC198DE25550
----------------------------639275760242036520206377--
```

c#

```body
Content-Type: multipart/form-data; boundary="e9d5712f-7923-4ec5-8bf3-c8d5d3cd3217"
Content-Length: 502

--e9d5712f-7923-4ec5-8bf3-c8d5d3cd3217
Content-Type: text/plain; charset=utf-8
Content-Disposition: form-data; name=mch_id


--e9d5712f-7923-4ec5-8bf3-c8d5d3cd3217
Content-Type: text/plain; charset=utf-8
Content-Disposition: form-data; name=media_hash

33F15BC2D17D6FFBC18FA566EF65722E
--e9d5712f-7923-4ec5-8bf3-c8d5d3cd3217
Content-Type: text/plain; charset=utf-8
Content-Disposition: form-data; name=sign

1E377684F9BD583D2ED26FB367916C0C
--e9d5712f-7923-4ec5-8bf3-c8d5d3cd3217--
```

因 `"` 而造成發送失敗，[參考](https://developers.de/blogs/damir_dobric/archive/2013/09/10/problems-with-webapi-multipart-content-upload-and-boundary-quot-quotes.aspx)

```txt
RFC 2612 原文：

Although RFC 2046 [40] permits the boundary string to be
quoted, some existing implementations handle a quoted boundary
string incorrectly.
```

* Boundary 的雙引號

    ```csharp
    // 兩個問題都是由於雙引號導致的，所以只需要在真正發起調用之前將內部的雙引號替換為空，或者將缺失的雙引號添加上即可。
    var boundaryValue = form.Headers.ContentType.Parameters.Single(p => p.Name == "boundary");
    boundaryValue.Value = boundaryValue.Value.Replace("\"", String.Empty);
    ```

* 表單內鍵值對，值的雙引號

    ```csharp
    // 在構造內部 Content 的時候，其 Name 手動賦予雙引號。
    var form = new MultipartFormDataContent
    {
        {new StringContent(mchId), "\"mch_id\""},
        {new ByteArrayContent(bytes), "media", $"\"{HttpUtility.UrlEncode(Path.GetFileName(imagePath))}\""},
        {new StringContent(mediaHash), "\"media_hash\""},
        {new StringContent(sign), "sign"}
    };
    ```

## [NuGet 已被取代且易受攻擊的相依性](https://docs.microsoft.com/zh-tw/nuget/concepts/security-best-practices#nuget-deprecated-and-vulnerable-dependencies)

> dotnet list package --deprecated
> dotnet list package --vulnerable

## nuget 套件

### [路徑更換](https://docs.microsoft.com/zh-tw/nuget/consume-packages/managing-the-global-packages-and-cache-folders)

```powershell
# 檢視資料夾位置
nuget locals all -list 

# 設定環境變數
[Environment]::SetEnvironmentVariable('NUGET_PACKAGES', $PATH, 'Machine')
[Environment]::SetEnvironmentVariable('NUGET_HTTP_CACHE_PATH', $PATH, 'Machine')
```

### 清除快取

```powershell
nuget locals all -clear
dotnet nuget locals all --clear
```

