---
title: Apache Cordova
date:  2015-02-04 12:00:53
key:   20150204
tags:  apache cordova
---

제가 회사를 다니면서 혹은 혼자서 취미생활을 하면서 주로 할일 위주로 시간을 관리했습니다.
좀 충격적인건 시간관리를 하는 방법은 TODO list위주가 아니라 시간 위주가 되어야 한다는 것 이었습니다.

  * [The Essential Drucker - 피터 드러커 글 모음 03. 시간 관리 방법](http://www.emh.co.kr/content.pl?the_essential_drucker3)
  * [효율적 시간관리를 위한 14가지 방법](https://www.facebook.com/notes/%ED%97%88%EC%A4%80%ED%98%81/%ED%9A%A8%EC%9C%A8%EC%A0%81-%EC%8B%9C%EA%B0%84%EA%B4%80%EB%A6%AC%EB%A5%BC-%EC%9C%84%ED%95%9C-14%EA%B0%80%EC%A7%80-%EB%B0%A9%EB%B2%95-%ED%97%88%EC%A4%80%ED%98%81%EB%85%B8%ED%8A%B8/214174841939507)

예전에 시간관리를 하겠다는 명목으로 프랭클린 플래너를 사서 쓴적이 있는데 저에겐 오히려 역효과만 발생해서 사용을 중지했습니다.
시간 관리를 잘 하려면 시간을 어떻게 쓰고 있는지 로그를 남겨야 하는데 좀 편하게 해보고자 앱을 만들까 합니다. (음?)

<!--more-->

앱을 만들때 멀티플랫폼을 지원하기 위한 라이브러리가 여러가지 있지만 저는 Cordova를 쓸까합니다.
예전에 테스트로 Xamarin을 설치해본적이 있는데 외부 라이브러리 하나 추가하니 바이너리가 제한 크기를 넘어가 바로 결제를 요구해서 당황하고 중단한 적이 있었습니다.
Cordova는 오픈소스이며 HTML, CSS, 및 JavaScript 만으로 네이티브 모바일 앱을 만들 수 있다고 이야기하고 있습니다.

저같은 경우는 Microsoft 제품의 노예라 Visual Studio와 통합된 툴을 찾아서 설치하였습니다.
[Getting Started with Visual Studio Tools for Apache Cordova](https://msdn.microsoft.com/en-us/library/dn771545.aspx) 로 이동하시면 다운로드 사이트로 링크를 제공합니다.
다운받고 설치하려고 하니 5G정도의 용량을 요구합니다.
상당히 덩치가 큰 녀석입니다 -_-;

![Cordova Project 화면](/assets/images/cordova/visualstudio_cordova_project.png)

설치 후 프로젝트를 생성하려고 보니 위 그림과 같이 JavaScript 와 TypeScript 하위에 Apache Cordova Apps 프로젝트 템플릿이 생긴것을 확인하였습니다.
아무것도 모르지만 일단 JavaScript로 프로젝트를 생성해 보겠습니다.

![Cordova Project 초기 화면](/assets/images/cordova/cordova_overview_1.png)

생성하고 보니 JavaScript와 TypeScript를 모두 지원한다는 문구와 함께 Cordova와 Cordova Plugins에 대해서도 인텔리센스가 작동한다고 뜨고 있습니다.
물론 디버깅과 진단 도구도 있다고 하네요 +_+!
우측 프로젝트 트리에서는 기타 멀티플랫폼 툴도 마찬가지겠지만 OS 종류별로 폴더가 나뉘어 있습니다.
아마 native별로 달라져야 하는 부분에 대한 것이겠죠?


![Cordova Project 초기 화면](/assets/images/cordova/cordova_overview_2.png)

config.xml이 있길래 눌러보니 뭔가 창이 뜹니다.
Common 페이지에서는 기본적인 정보들을 설정하게 했군요.

![Cordova Project 초기 화면](/assets/images/cordova/cordova_overview_3.png)

Plugins 탭을 보니 베터리, 카메라, 캡쳐, 콘솔, 연락처, 등등 상당히 많은 플러그인이 있습니다.
Native별로 어떻게 처리했을지 매우 궁금해지는 부분인데 차근차근 알아나가면 되겠죠?


![Cordova Project 초기 화면](/assets/images/cordova/cordova_overview_4.png)
![Cordova Project 초기 화면](/assets/images/cordova/cordova_overview_5.png)
![Cordova Project 초기 화면](/assets/images/cordova/cordova_overview_6.png)

각 플랫폼별로 세부정보를 입력할 수 있군요!
Windows/Android/iOS 세가지에 대한 설정이 존재하는데 만약 새로운 플랫폼이 등장한다면 Cordova에서 대응을 빠르게 될 수 있는 구조인지 궁금해지는 부분이군요!

빌드를 하니 아래와 같이 메시지가 주르륵 뜹니다.
Android 용으로 컴파일이 되는군요.

{% highlight c# linenos %}

1>------ Build started: Project: TimeManager, Configuration: Debug Android ------
1>C:\Program Files (x86)\MSBuild\Microsoft\VisualStudio\v12.0\TypeScript\Microsoft.TypeScript.targets(95,5): warning : The TypeScript Compiler was given no files for compilation, so it will skip compiling.
1>  GeneratedJavascript=
1>  C:\git\TimeManager\TimeManager>call "C:\Program Files (x86)\nodejs\"\nodevars.bat 
1>  Your environment has been set up for using Node.js 0.10.33 (ia32) and npm.
1>  ------ Ensuring correct global installation of package from source package directory: C:\PROGRAM FILES (X86)\MICROSOFT VISUAL STUDIO 12.0\COMMON7\IDE\EXTENSIONS\UW4VV3MU.RPU\packages\vs-mda
1>  ------ Name from source package.json: vs-mda
1>  ------ Version from source package.json: 0.1.70
1>  ------ Package not currently installed globally.
1>  ------ Installing globally from source package. This could take a few minutes...
1>  C:\Users\Administrator\AppData\Roaming\npm\vs-cli -> C:\Users\Administrator\AppData\Roaming\npm\node_modules\vs-mda\vs-cli.cmd
1>  vs-mda@0.1.70 C:\Users\Administrator\AppData\Roaming\npm\node_modules\vs-mda
1>  ├── rimraf@2.2.6
1>  ├── ncp@0.5.1
1>  ├── mkdirp@0.3.5
1>  ├── q@1.0.1
1>  ├── adm-zip@0.4.4
1>  ├── optimist@0.6.1 (wordwrap@0.0.2, minimist@0.0.10)
1>  ├── fstream@0.1.28 (inherits@2.0.1, graceful-fs@3.0.5)
1>  ├── tar@0.1.20 (inherits@2.0.1, block-stream@0.0.7)
1>  ├── elementtree@0.1.6 (sax@0.3.5)
1>  ├── request@2.36.0 (json-stringify-safe@5.0.0, forever-agent@0.5.2, aws-sign2@0.5.0, qs@0.6.6, oauth-sign@0.3.0, tunnel-agent@0.4.0, mime@1.2.11, node-uuid@1.4.2, tough-cookie@0.12.1, http-signature@0.10.1, form-data@0.1.4, hawk@1.0.0)
1>  ├── ripple-emulator@0.9.24 (connect-xcors@0.5.2, colors@0.6.0-1, open@0.0.3, accounting@0.4.1, request@2.12.0, moment@1.7.2, express@3.1.0)
1>  ├── plugman@0.22.4 (q@0.9.7, nopt@1.0.10, underscore@1.4.4, rc@0.3.0, npm@1.3.4, cordova-lib@0.21.6)
1>  └── cordova@4.0.0 (q@0.9.7, underscore@1.4.4, nopt@2.2.1, cordova-lib@4.0.0)
========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========

{% endhighlight %}

실행하려고 하니 아래와 같은 에러가 발생했습니다.

> Chrome must be installed in order to launch the app in Ripple.

일단은 에러를 회피하고자 Build Target을 Android Emulator로 변경하였습니다.

![Cordova Project Build Target 선택 화면](/assets/images/cordova/cordova_execute_0.png)

그리고 아래의 실행결과를 확인할 수 있었습니다.

![Cordova Project 실행 화면](/assets/images/cordova/cordova_execute_1.png)

이때의 Build Output은 다음과 같았습니다.

{% highlight c# linenos %}

1>------ Build started: Project: TimeManager, Configuration: Debug Android ------
1>C:\Program Files (x86)\MSBuild\Microsoft\VisualStudio\v12.0\TypeScript\Microsoft.TypeScript.targets(95,5): warning : The TypeScript Compiler was given no files for compilation, so it will skip compiling.
1>  GeneratedJavascript=
1>  C:\git\TimeManager\TimeManager>call "C:\Program Files (x86)\nodejs\"\nodevars.bat 
1>  Your environment has been set up for using Node.js 0.10.33 (ia32) and npm.
1>  ------ Ensuring correct global installation of package from source package directory: C:\PROGRAM FILES (X86)\MICROSOFT VISUAL STUDIO 12.0\COMMON7\IDE\EXTENSIONS\UW4VV3MU.RPU\packages\vs-mda
2>------ Deploy started: Project: TimeManager, Configuration: Debug Android ------
2>Starting launch process C:\Program Files (x86)\nodejs\node.exe ""C:\Users\Administrator\AppData\Roaming\npm\node_modules\vs-mda\emulator.js"" --platform android --path "C:\git\TimeManager\TimeManager\bld\Debug" --deployTarget "emulator" --language en-US --configuration debug
2>  Generating config.xml from defaults for platform "android"
2>  Calling plugman.prepare for platform "android"
2>  Preparing android project
2>  Processing configuration changes for plugins.
2>  Iterating over installed plugins: []
2>  Writing out cordova_plugins.js...
2>  Wrote out Android application name to "TimeManager"
2>  This app does not have launcher icons defined
2>  Wrote out Android package name to "io.cordova.TimeManager"
2>  Running command: C:\git\TimeManager\TimeManager\bld\Debug\platforms\android\cordova\run.bat --nobuild --emulator --debug
2>  Skipping build...
2>  Built the following apk(s):
2>      C:\git\TimeManager\TimeManager\bld\Debug\platforms\android\ant-build\CordovaApp-debug.apk
2>  WARNING : no emulator specified, defaulting to AVD_GalaxyNexus_ToolsForApacheCordova
2>  Waiting for emulator...
2>  Creating filesystem with parameters:
2>      Size: 69206016
2>      Block size: 4096
2>      Blocks per group: 32768
2>      Inodes per group: 4224
2>      Inode size: 256
2>      Journal blocks: 1024
2>      Label: 
2>      Blocks: 16896
2>      Block groups: 1
2>      Reserved block group size: 7
2>  Created filesystem with 11/4224 inodes and 1302/16896 blocks
2>  Booting up emulator (this may take a while).................................................BOOT COMPLETE
2>  Installing app on emulator...
2>  Using apk: C:\git\TimeManager\TimeManager\bld\Debug\platforms\android\ant-build\CordovaApp-debug.apk
2>  Launching application...
2>  LAUNCH SUCCESS
2>  Command finished with error code 0: C:\git\TimeManager\TimeManager\bld\Debug\platforms\android\cordova\run.bat --nobuild,--emulator,--debug
========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
========== Deploy: 1 succeeded, 0 failed, 0 skipped ==========

{% endhighlight %}

Android를 띄우고 앱을 설치하는 과정이 너무 느려서 앱개발하기엔 좋지 않은 것 같습니다.
이제 위에서 발생한 에러를 고쳐보도록 하죠.

여러가지 문서를 참조해보니 경로를 못찾는 문제라고 해서 여러가지 방법을 시도해봤는데 모두 무위로 돌아갔습니다.
추천하는 방법들은 대부분 Chrome 재설치, Cordova 툴 재설치 등이 있었는데 모두 다 안되었습니다.
이제 답이 없다는 생각에 Decompile을 해보기로 결심했습니다.

> Decompiler 사용법은 [C# Decompiler]({% post_url 2015-01-28-CSharp-Decompiler %}) 포스팅을 참초바랍니다.

Cordova 관련 Extension의 폴더를 찾아내어 아래의 파일에 대해 Decompile을 해보았습니다.

  * Microsoft.VisualStudio.MultiDeviceHybridApps.Common.dll

브라우져 관련한 요소를 찾다보니 아래의 코드를 찾을 수 있었습니다.

{% highlight c# linenos %}
protected override Action<object> CreateBackgroundAction(ConfiguredProject project, SVsServiceProvider serviceProvider, Action<LauncherErrorData> errorHandler, Action successHandler)
{
	return (Action<object>)(arg =>
	{
		string str = string.Empty;
		string startPage;
		try
		{
			startPage = this.configReaderFactory.GetConfigReader(project).GetStartPage();
		}
		catch (Exception ex)
		{
			if (errorHandler == null)
				return;
			errorHandler(new LauncherErrorData(ex));
			return;
		}
		if (string.IsNullOrEmpty(startPage))
		{
			this.logger.LogErrorTask(StringTable.ConfigStartPageMissing);
			if (errorHandler == null)
				return;
			errorHandler(new LauncherErrorData(new Exception(StringTable.ConfigStartPageMissing)));
		}
		else
		{
			RippleLauncher.RippleCommand = (ICommand)arg;
			this.webBrowserService.OpenInWebBrowser("Google Chrome", RippleLauncher.CreateRippleUriForDevice(this.rippleTarget, startPage, true), string.Format((IFormatProvider)CultureInfo.InvariantCulture, " --remote-debugging-port={0} --disable-first-run-ui --user-data-dir={1} --disable-popup-blocking", new object[2]
				{
					(object) this.portHelper.FindFreePort(9222),
					(object) RippleLauncher.ChromeUserDataDir
				}));
			RippleLauncher.RippleCommand.CancelDataRedirects();
			if (successHandler == null)
				return;
			successHandler();
		}
	});
}
{% endhighlight %}

위 코드 중에서 "Google Chrome" 을 호출하는 __OpenInWebBrowser__ 함수를 확인하실 수 있는데 이 함수는 결국 아래의 함수를 호출합니다.


{% highlight c# linenos %}
namespace Microsoft.VisualStudio.MultiDeviceHybridApps.Common.WebBrowser
{
// ...

public IEnumerable<BrowserDescriptor> FindInstalledBrowsers()
{
	List<BrowserDescriptor> list = new List<BrowserDescriptor>();
	using (IRegistryKey machineRegistryKey = this.registry.LocalMachineRegistryKey)
	{
		if (machineRegistryKey == null)
			throw new InvalidOperationException("Error accessing LOCAL_MACHINE key in registry.");
		using (IRegistryKey key = machineRegistryKey.OpenSubKey("SOFTWARE\\Clients\\StartMenuInternet"))
		{
			if (key != null)
				this.ProcessKey(key, list);
		}
	}
	using (IRegistryKey currentUserRegistryKey = this.registry.CurrentUserRegistryKey)
	{
		if (currentUserRegistryKey == null)
			throw new InvalidOperationException("Error accessing CURRENT_USER key in registry.");
		using (IRegistryKey key = currentUserRegistryKey.OpenSubKey("SOFTWARE\\Clients\\StartMenuInternet"))
		{
			if (key != null)
				this.ProcessKey(key, list);
		}
		if (InstalledBrowsersHelper.IsWindows7())
		{
			using (IRegistryKey registryKey = currentUserRegistryKey.OpenSubKey("Software\\Microsoft\\Windows\\CurrentVersion\\App Paths\\chrome.exe"))
			{
				if (registryKey != null)
				{
					string path = registryKey.GetValue("") as string;
					if (path != null)
						this.ProcessPath(path, "Google Chrome", list);
				}
			}
		}
	}
	return (IEnumerable<BrowserDescriptor>)list;
}

// ...
} // end of namespace

{% endhighlight %}

위 코드를 보시면 아시겠지만 공용 레지스터와 사용자별 레지스터를 모두 참조하기 때문에 개인별로 설치된 크롬도 문제없이 찾아올 수 있음을 확인했습니다.
문제가 되는 부분은 아래의 함수였습니다.

{% highlight c# linenos %}
public ICommand OpenInWebBrowser(string browserName, Uri uri, string commandLineArgs)
{
	return this.CommandService.Execute(new ProcessStartInfo(this.FindBrowserDescriptor(browserName).BrowserPath, uri.AbsoluteUri + commandLineArgs)
	{
		UseShellExecute = true,
		Verb = "Open"
	}, (DataReceivedEventHandler)null, (DataReceivedEventHandler)null, (ILogger)null);
}

private BrowserDescriptor FindBrowserDescriptor(string browserName)
{
    return Enumerable.FirstOrDefault<BrowserDescriptor>(this.helper.FindInstalledBrowsers(), (Func<BrowserDescriptor, bool>) (bd => bd.Name.Equals(browserName)));
}
{% endhighlight %}

__FindBrowserDescriptor__ 함수의 __bd.Name.Equals(browserName)__ 부분을 보시게 되면 browserName은 "Google Chrome"인데 참조하는 Register Key 값을 보니 "Chrome" 이더군요.
해결방법은 다음과 같습니다.

  * 소스코드를 __browserName.Contains(bd.Name)__ 으로 변경
  * __HKEY_LOCAL_MACHINE\SOFTWARE\Clients\StartMenuInternet\Google Chrome__ 에 있는 (Default) 키값을 "Chrome" 에서 "Google Chrome" 으로 변경

물론 둘다 가능하지만 저는 얌전하게 Register Key 값을 "Chrome" 에서 "Google Chrome" 으로 변경하였습니다.
기본적으로 코드가 문제이니 google 님과 stackoverflow 에 물어봐도 모르는게 당연한 것 같습니다.
이전에 [C# Decompiler]({% post_url 2015-01-28-CSharp-Decompiler %}) 포스팅에서도 말씀드렸지만 내부에 문제가 있을 것 같은 예감이 들면 이번 포스팅처럼 직접 열어보고 확인할 수 있습니다.
아래 소스 코드는 Cordova Extension에서 브라우저를 Enumerate하기 위해 사용하는 코드를 완전체로 추려서 테스트 해본 예제입니다.


{% highlight c# linenos %}

using Microsoft.Win32;
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.IO;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace BrowserEnumerator
{
    class Program
    {
        void Main()
        {
            Helper.FindInstalledBrowsers();
        }

        // Define other methods and classes here
        public interface IRegistryKey : IDisposable
        {
            IRegistryKey OpenSubKey(string key);

            string[] GetSubKeyNames();

            object GetValue(string name);
        }

        public class MyRegistryKey : IRegistryKey, IDisposable
        {
            private RegistryKey realKey;

            public MyRegistryKey(RegistryKey key)
            {
                this.realKey = key;
            }

            ~MyRegistryKey()
            {
                RegistryKey registryKey = this.realKey;
                this.Dispose(false);
            }

            public IRegistryKey OpenSubKey(string key)
            {
                if (this.realKey.OpenSubKey(key) == null)
                    return (IRegistryKey)null;
                return (IRegistryKey)new MyRegistryKey(this.realKey.OpenSubKey(key));
            }

            public string[] GetSubKeyNames()
            {
                return this.realKey.GetSubKeyNames();
            }

            public object GetValue(string name)
            {
                return this.realKey.GetValue(name);
            }

            public void Dispose()
            {
                this.Dispose(true);
                GC.SuppressFinalize((object)this);
            }

            private void Dispose(bool disposing)
            {
                if (!disposing || this.realKey == null)
                    return;
                this.realKey.Dispose();
                this.realKey = (RegistryKey)null;
            }
        }

        public interface IRegistry
        {
            IRegistryKey LocalMachineRegistryKey { get; }

            IRegistryKey CurrentUserRegistryKey { get; }
        }

        public class MyRegistry : IRegistry
        {
            public IRegistryKey LocalMachineRegistryKey
            {
                get
                {
                    return (IRegistryKey)new MyRegistryKey(Registry.LocalMachine);
                }
            }

            public IRegistryKey CurrentUserRegistryKey
            {
                get
                {
                    return (IRegistryKey)new MyRegistryKey(Registry.CurrentUser);
                }
            }
        }

        public class BrowserDescriptor
        {
            public string BrowserPath { get; private set; }

            public Version BrowserVersion { get; private set; }

            public string Name { get; private set; }

            public BrowserDescriptor(string path, Version version, string name)
            {
                this.BrowserPath = path;
                this.BrowserVersion = version;
                this.Name = name;
            }

            public override string ToString()
            {
                return "\nName:" + (object)this.Name + "\nPath:" + this.BrowserPath + "\nVersion:" + (string)(object)this.BrowserVersion;
            }
        }

        public static class Helper
        {
            private static MyRegistry registry = new MyRegistry();

            public static IEnumerable<BrowserDescriptor> FindInstalledBrowsers()
            {
                List<BrowserDescriptor> list = new List<BrowserDescriptor>();
                using (IRegistryKey machineRegistryKey = registry.LocalMachineRegistryKey)
                {
                    if (machineRegistryKey == null)
                        throw new InvalidOperationException("Error accessing LOCAL_MACHINE key in registry.");
                    using (IRegistryKey key = machineRegistryKey.OpenSubKey("SOFTWARE\\Clients\\StartMenuInternet"))
                    {
                        if (key != null)
                            ProcessKey(key, list);
                    }
                }
                using (IRegistryKey currentUserRegistryKey = registry.CurrentUserRegistryKey)
                {
                    if (currentUserRegistryKey == null)
                        throw new InvalidOperationException("Error accessing CURRENT_USER key in registry.");
                    using (IRegistryKey key = currentUserRegistryKey.OpenSubKey("SOFTWARE\\Clients\\StartMenuInternet"))
                    {
                        if (key != null)
                            ProcessKey(key, list);
                    }
                    //if (InstalledBrowsersHelper.IsWindows7())
                    //{
                    //    using (IRegistryKey RegistryKey = currentUserRegistryKey.OpenSubKey("Software\\Microsoft\\Windows\\CurrentVersion\\App Paths\\chrome.exe"))
                    //    {
                    //        if (registryKey != null)
                    //        {
                    //            string path = registryKey.GetValue("") as string;
                    //            if (path != null)
                    //                this.ProcessPath(path, "Google Chrome", list);
                    //        }
                    //    }
                    //}
                }
                return (IEnumerable<BrowserDescriptor>)list;
            }

            public static void ProcessKey(IRegistryKey key, List<BrowserDescriptor> list)
            {
                if (key == null)
                    throw new InvalidOperationException("Error accessing SOFTWARE\\Clients\\StartMenuInternet key in registry.");
                string[] subKeyNames = key.GetSubKeyNames();
                if (subKeyNames == null)
                    return;
                foreach (string key1 in subKeyNames)
                {
                    using (IRegistryKey registryKey1 = key.OpenSubKey(key1))
                    {
                        if (registryKey1 != null)
                        {
                            string browserName = registryKey1.GetValue("") as string;
                            if (browserName != null)
                            {
                                using (IRegistryKey registryKey2 = registryKey1.OpenSubKey("shell\\open\\command"))
                                {
                                    if (registryKey2 != null)
                                    {
                                        string path = registryKey2.GetValue("") as string;
                                        if (path != null)
                                            ProcessPath(path, browserName, list);
                                    }
                                }
                            }
                        }
                    }
                }
            }

            public static void ProcessPath(string path, string browserName, List<BrowserDescriptor> browserList)
            {
                if (path.StartsWith("\"", StringComparison.Ordinal) && path.EndsWith("\"", StringComparison.Ordinal))
                    path = path.Substring(1, path.Length - 2);
                if (!FileHelper.FileExists(path))
                    return;
                Version productVersion = FileHelper.GetVersion(path);
                BrowserDescriptor browserDescriptor = new BrowserDescriptor(path, productVersion, browserName);
                int index = browserList.FindIndex((Predicate<BrowserDescriptor>)(bd =>
                {
                    if (bd.Name.Equals(browserName))
                        return bd.BrowserVersion.Equals(productVersion);
                    return false;
                }));
                if (index > -1)
                {
                    browserList.RemoveAt(index);
                    browserList.Insert(index, browserDescriptor);
                }
                else
                    browserList.Add(browserDescriptor);
            }
        }

        public static class FileHelper
        {
            public static bool FileExists(string path)
            {
                return File.Exists(path);
            }

            public static Version GetVersion(string path)
            {
                FileVersionInfo versionInfo = FileVersionInfo.GetVersionInfo(path);
                return new Version(versionInfo.FileMajorPart, versionInfo.ProductMinorPart, versionInfo.ProductBuildPart);
            }
        }
    }
}

{% endhighlight %}

Microsoft Visual Studio에서 Cordova로 코딩할 생각에 설레었는데 오늘 하루는 Extension만 디버그하다가 4시간이 흘러가버렸네요.
툴은 잘 만들어야 할 기본적인것인데 브라우저 실행정도는 내부에서 테스트 후 올릴 수 있지 않을까라는 아쉬움이 남습니다.
결국 툴이란 생산성향상을 위해서 만드는 것인데 세세한 기능을 신경못쓴다 하더라도 브라우저 이름이 조금 차이나서 실행 안되는건 좀 과하단 생각이 드네요.
이것과 비슷한 경험이 처음이 아니라 Roslyn 작업할 때도 겪었는데 그래도 다행인건 Decompiler가 있어서 시간이 좀 걸리더라도 문제가 해결가능하기 때문에 포기하지 않으면 길은 있다는 점인 것 같습니다.
이런저런 삽질을 마치고 Ctrl + F5를 누르니 아래와 같이 이쁜 화면이 나옵니다!
오오 ㅠ_ㅠ 나의 Cordova!

![Cordova Project 실행 화면](/assets/images/cordova/cordova_execute_2.png)

Android Emulator 보다 훨씬 빠르게 뜨네요!
좀 시간이 걸렸지만 앞으로 이 화면을 수백번 볼 생각하면 적절한 시간 투자였다고 생각합니다.
오늘의 포스팅은 여기서 마칩니다!
