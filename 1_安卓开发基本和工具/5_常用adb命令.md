> `/data/local/tmp`  拷贝到 安卓手机的文件, 放到这个文件内 , 才可以给予执行权限

```shell
$adb  devices   # 查看有哪些硬件存在

$adb  logcat               #查看日志
$adb  shell                #adb linux命令行, 默认根目录在 安卓手机那里
$adb  install  *.apk       #安装 apk
$adb  push  电脑源文件  手机目标目录    #adb push a.out /storage/self/primary/Download/   推送
$adb  pull  手机文件    电脑目录       #adb pull  /storage/self/primary/Download/a.out ~/Downloads 拉取
```

