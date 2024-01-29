---
{"dg-publish":true,"permalink":"/07-Windows系统/Windows11取消右键“显示更多选项”/","dgPassFrontmatter":true,"created":"2023-10-27T08:59:53.262+08:00","updated":"2024-01-29T10:58:16.238+08:00"}
---


管理员运行命令提示符

``` shell
# 取消“显示更多选项”
reg.exe add "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32" /f /ve

# 重启资源管理器
taskkill /f /im explorer.exe & start explorer.exe

# 复原
reg.exe delete "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32" /va /f

# 重启资源管理器
taskkill /f /im explorer.exe & start explorer.exe
```