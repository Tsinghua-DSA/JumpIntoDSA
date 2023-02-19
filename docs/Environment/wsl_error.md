# WSL常见问题

根据助教答疑的情况，持续更新本页面。主要记录微软官方文档未覆盖的问题。

微软官方的文档:

https://learn.microsoft.com/zh-cn/windows/wsl/faq 

https://learn.microsoft.com/zh-cn/windows/wsl/troubleshooting 

如果你看到的报错信息是英文的，那么查询微软的英文文档可能会有帮助:

https://learn.microsoft.com/en-us/windows/wsl/troubleshooting

## 1 WSL安装过程中的问题

*待补充*

## 2 WSL 安装成功，在启动WSL后遇到的问题

### 2.1 DNS解析错误

**表现**: 

在尝试`apt-get update`的时候，报错`Temporary failure resolving 'archive.ubuntu.com'`

**原因**: 

WSL的已知问题，有些情况下DNS配置错误，导致无法从网站的域名解析到对应的IP

**解决方法**: 

https://www.cnblogs.com/liujiaxin2018/p/16200315.html

https://github.com/microsoft/WSL/issues/5256 

### 2.2 代理配置错误

**表现**

报错信息 `参考的对象类型不支持尝试的操作`  `Error code: Wsl/Service/0x8007273d`

**原因**

某些网络代理软件与WSL发生冲突

**解决方法**

https://github.com/microsoft/WSL/issues/4194

https://github.com/microsoft/WSL/issues/4177

如果其他方法无效，可以考虑每次启动WSL之前都运行一下`netsh winsock reset`。可以把它放到一个脚本里，保存到桌面上，每次需要用的时候直接点击运行。
