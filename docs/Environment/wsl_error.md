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