# NanUI
### 前言
NanUI是一个基于ChromiumFX开源项目的.Net Winform界面库，ChromiumFX是Chromium Embedded Framework的.Net实现。众所周知，Chromium Embedded Framework (CEF)是由 Marshall Greenblatt 在2008年创办的开源项目，致力于基于Google Chromium项目开发一个Web控件。可以将Chrome浏览器的功能（页面渲染，JS 执行）嵌入到其他应用程序的框架。CEF 作为嵌入式浏览器框架最适合的应用场景应该是Html页面渲染，所以很多程序都基于CEF来为应用程序提供 HTML 页面渲染的功能，如有道笔记，微信Windows客户端，网易云音乐，Evernote，GitHub Window Client，Q+，Adobe Brackets 等。

在此之前CEF应用大多使用C++来进行开发，对于.Net项目和.Net程序原来说只能是望梅止渴。基于ChromiumFX项目的诞生，.Net项目终于能够与CEF来一次亲密接触，但ChromiumFX项目主要注重于浏览器核心的实现，对Winform界面开发并无太大作用。在此背景下，NanUI孕育而生。

NanUI打破了传统的Winform界面设计方式，通过NanUI你能够使用Html5、CSS3和javascript来构建你的Winform界面。如果你熟悉诸如bootstrap、jQuery、WinJS等各类CSS或JS库的话，你能够根据喜好或客户要求设计出各种漂亮的Winform界面。所以，使用NanUI，你的Winform软件界面将有无限可能。

![NanUI](http://images2015.cnblogs.com/blog/352785/201605/352785-20160518180435701-1461536015.png)

QQ群：
241088256

### 更新日志
> 0.4.3

- 解决了最大化出现白色边的问题。
- 解决了Win7下面不开启DWM错误的渲染了系统主题边框的问题（感谢 Mr.铲屎 提供帮助）。

**注意：**请使用了0.4.2版本的朋友及时更新0.4.3版本

> 0.4.2

- 添加了离屏渲染的支持，并添加了一个离屏渲染+LayeredForm的小火箭渲染例子。

- 增加了本地资源的支持，现在可以通过 local:///C:/xxx/a.jpg这种方式来调用本地目录里的文件了。使用local:///x.jpg则调用当前目录下文件，这里稍作修改了下，和原本的file:///这种scheme的方式不同。

- 部分使用第三方theme.dll的Win7和XP系统在NonclientMode模式下面会突发边框和标题栏重绘时不正确的绘制了系统原生皮肤的图像，这个问题还在找解决办法，请有条件的朋友协助处理此BUG

> 版本0.4.1

**0.4.1更新后无法下载CEF库的问题紧急修复了，这是我没有仔细核对版本号造成的问题，现在已修正。请下载了0.4.1 release的朋友重新下载Release**

 - 新增了NonclientMode下窗口投影的开关NonclientModeDropShadow，现在在无边框模式下面窗口有阴影了（WinXP暂时没测试）。
 - 应大家的要求，从这个版本开始支持本地打包CEF运行框架了。将[all.exe](http://www.ohtrip.cn/NanUI/NanUIPackages/3.2526.1373/all.exe)自解压包中的问价解压到软件目录的fx文件夹，就可以达到CEF随软件分发的目的。


> 版本0.4

- 现在CEF运行库安装路径从%AppData%下转到了%ProgramData%目录下，有群友报告此问题，在多用户环境中若使用%AppData%作为CEF共享目录，如果切换用户后会导致NanUI重新下载CEF运行库。

- 重新优化了CEF运行库自动下载的逻辑，现在CEF运行库带有版本号了，因此采用不同CEF版本的NanUI软件理论上不会因为版本不同造成无法启动的问题。

- 现在CEF运行库的版本从3.2623.1401.gb90a3be（Chromium 49）降回到3.2526.1373.gb660893（Chromium 47），因为CEF 3.2623.1401在使用EvalJavascript方法时有概率无法接收到回调参数。

- 现版本把内部脚本移到了Resources\InitalScripts.js中，不再作为const string，方便修改。

- 另外，各位要求的原生窗口回来了。通过修改窗口的Borderless属性来控制是否开启原生窗口。默认情况Borderless属性为True，若要使用原生窗口请设置为False


### 版本状态
NanUI版本
> 0.4.1 alpha预览

支持的操作系统
> Windows XP及已上版本

最小支持 .NET 版本
> .NET Framework 4.0 Client Profile

当前CEF版本
> CEF 3.2526.1373.gb660893 Chromium 47.0.2526.80

### 如何使用
**引用NanUI库**

NanUI使用非常简单。在项目中只需引用“NetDimension.NanUI.dll”一个库即可。如果本机没有检测到CEF运行库，NanUI会提供下载地址或者自动下载相应的支持文件。

**初始化CEF环境**

在Main函数中
```C#
static class Program
{
   /// <summary>
   /// 应用程序的主入口点。
   /// </summary>
  [STAThread]
  static void Main()
  {
	Application.EnableVisualStyles();
	Application.SetCompatibleTextRenderingDefault(false);

	HtmlUILauncher.EnableFlashSupport = true;	//开启Flash支持

	if (HtmlUILauncher.InitializeChromium())	//初始化CEF环境
	{
	  //启动主窗体
	  Application.Run(new MainFrame());
	}
  }
}
```

**加载Html**

初始化完成后在窗口中继承HtmlUIForm类，并传入网页地址就完成了页面的加载。除了使用本地资源和远程网址，NanUI还提供了对嵌入式资源的支持，具体请参看Wiki中的相关示例。
```C#
public class MainFrame : HtmlUIForm
{
	public MainFrame()
		: base("<页面地址>")
	{
		InitializeComponent();
		//... 各种初始化代码
	}
}

```

只需如上简单的三步，就完成了对NanUI的加载。其他界面设计的工作就交给美工吧！

代码示例中，将详细展示NanUI的使用方法。当然你也可以通过WIKI来了解更多信息。

**注意**

如果加载窗口后一片空白，请按照下述方法解决：

项目属性->调试->取消勾选“启用 Visual Studio 承载进程”

另外，请勾选“启用本机代码调试”选项来解决中文输入法状态启动调试时软件崩溃的问题。

### 演示项目

**MarkdownDotNet**

NanUI.Demo.MarkdownDotNet

一个功能简单的Markdown编辑器，主要展示NonclientMode。

**CodeEditor**

NanUI.Demo.CodeEditor
NanUI.Demo.CodeEditor.Resources

该项目使用强大的JS项目CodeMirror，配合Bootstrap库进行界面布局，实现了一个简单的代码编辑器功能。

**Welcome**

NanUI.Demo.Welcome

该项目使用jQuery及Bootstrap构建界面，主要演示了NanUI对HTML5、CSS3、Flash、WebGL等技术的支持程度。

### CEF运行库下载
| 说明           | 大小  | 说明  | 下载                                                           |
| -------------- |------:|:-----:|:-------------------------------------------------------------:|
| 完整安装包      | 73.0M | 推荐  | [下载](http://www.ohtrip.cn/NanUI/NanUIPackages/3.2526.1373/all.exe)             |
| 资源文件        | 3.53M | 必要  | [下载](http://www.ohtrip.cn/NanUI/NanUIPackages/3.2526.1373/resources.exe)       |
| 32位CEF运行库   | 24.4M |      | [下载](http://www.ohtrip.cn/NanUI/NanUIPackages/3.2526.1373/x86/cef_x86.exe.exe)  |
| 32位Flash支持库 | 7.46M |      | [下载](http://www.ohtrip.cn/NanUI/NanUIPackages/3.2526.1373/x86/flash_x86.exe)    |
| 64位CEF运行库   | 29.2M |      | [下载](http://www.ohtrip.cn/NanUI/NanUIPackages/3.2526.1373/x64/cef_x64.exe.exe)  |
| 64位Flash支持库 | 10.2M |      | [下载](http://www.ohtrip.cn/NanUI/NanUIPackages/3.2526.1373/x64/flash_x64.exe)    |

**当前CEF运行库版本：3.2526.1373.gb660893**

### 参考
暂无，等我慢慢写。要想写得快，记得打赏我几杯咖啡：）
