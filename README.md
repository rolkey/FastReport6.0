# FastReport本地版

## 修改为.net6 可编译的版本

```bash
dotnet build FastReport.OpenSource.sln -f net6.0 -c Debug
```

## 查找最近编译的DLL

```bash
find . -type f -name "*.dll" -newermt "1 day ago"
```

