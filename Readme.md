NeoBusmaster
---

这是从 https://github.com/rbei-etas/busmaster.git Fork 出来的项目，由于官方的 busmaster 仓库似乎已经不再维护，在这里，我想延续它。

不得不承认，这是一个极其复杂的工程，由于其原本采用的技术和开发工具都已经很古老，这里将尝试使用这些开发工具中较新的版本，不可避免的会遇到一些问题，然后尝试去解决它。

从 Visual Studio Community 2013 到 Visual Studio Community 2019
---------------------------------------------------------------

使用 Visual Studio Installer （Visual Studio 安装程序），工作负载选择"使用C++的桌面开发"，一般的 MFC 组件不会被默认选中，你需要在右侧安装详细信息列表中将其选中。你不必再单独下载适用于 Visual Studio 2019 的 MFC 多字节支持，其已经包含在 MFC 组件中。

Installation of Additional Libraries
------------------------------------

Description
This section describes about the installation of additional libraries required for compiling BUSMASTER and its
dependency projects.

Busmaster 原文：

> Support to IXXAT hardware
The following steps should be followed to compile CAN_IXXAT_VCI project in BUSMASTER.
• Download and install IXXAT VCI drivers from the following link http://www.ixxat.com/download_vci_v3_en.html
• Set Windows environment variable 'IXXAT_VCI_SDK' with the path to the VCI header files (e.g.
IXXAT_VCI_SDK = "C:\Program Files\IXXAT\VCI 3.5\sdk\Microsoft_VisualC").
• Restart the computer after performing the above two steps

你可以在下面的链接中找到最新的支持 VCI V4 的安装文件。
https://www.ixxat.com/technical-support/pages/can-interfaces?ordercode=1.01.0086.10200

最直接的下载链接为:
https://www.ixxat.com/docs/librariesprovider8/ixxat-english-new/pc-can-interfaces/windows-drivers/vci-v4-windows-10-8-7-xp-sp2.zip?sfvrsn=9ceb48d7_10

值得注意的是, 安装时必需要选中 SDK -> SDK VCI (C)，构建 CAN_IXXAT_VCI 项目时要用到其中的头文件。

安装完成后，系统环境变量会自动增加 VciSDKDir，用来替代 IXXAT_VCI_SDK。

Building BUSMASTER
------------------

对于所有的工程，默认的平台工具集为"Visual Studio 2013 - Windows XP (v120_xp)"，配合 Visual Studio 2019，将其更新为 "Visual Studio 2019 (v142)"。

BUSMASTER Build Order:
To Build BUSMASTER its dependency solution files must be compiled first.The following section describes the
order of compilation.
Order:

1. Kernel\BusmasterKernel.sln

    Debug - 解决方案生成成功
    
    Release - 解决方案生成成功

2. DBManager\DBManager.sln

    我并没有找到这个项目，所以跳过。

3. BUSMASTER\BUSMASTER.sln

    - CAN_i-VIEW （加载失败），
    > busmaster\Sources\BUSMASTER\CAN_iVIEW\CAN_i-VIEW.vcxproj : error  : 无法加载具有重复项目项的项目: CAN_i-VIEW_Resource.h 作为 ClInclude 且作为 None 项类型包括在其中。
    从 CAN_i-VIEW.vcxproj 中删除 None 的项可修复。
    - BUSMASTER
    > 严重性	代码	说明	项目	文件	行	禁止显示状态
错误	C2062	意外的类型“void”	BUSMASTER	A:\xiaojian\Source\Siogian\busmaster\Sources\BUSMASTER\UDS_Protocol\UDSMainWnd.h	141	

    > 严重性	代码	说明	项目	文件	行	禁止显示状态
错误	C2238	意外的标记位于“;”之前	BUSMASTER	A:\xiaojian\Source\Siogian\busmaster\Sources\BUSMASTER\UDS_Protocol\UDSMainWnd.h	141	
    
    修复此类语法错误。
    - CAN_IXXAT_VCI

    > 严重性	代码	说明	项目	文件	行	禁止显示状态
错误	C1083	无法打开包括文件: “vcinpl.h”: No such file or directory	CAN_IXXAT_VCI	A:\xiaojian\Source\Siogian\busmaster\Sources\BUSMASTER\CAN_IXXAT_VCI\IxxatCanChannel.h	31	

    添加 $VciSDKDir 到头文件包含目录。

其他更多的问题已提交解决，参见首次提交记录。

4. BUSMASTER\Language Dlls\Language Dlls.sln
5. BUSMASTER\LDFEditor\LDFEditor.sln
6. BUSMASTER\LDFViewer\LDFViewer.sln
7. BUSMASTER\Format Converter\FormatConverter.sln