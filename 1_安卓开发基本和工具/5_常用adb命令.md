> `/data/local/tmp`  拷贝到 安卓手机的文件, 放到这个文件内 , 才可以给予执行权限

```shell
$adb  devices   # 查看有哪些硬件存在

$adb  logcat               #查看日志
$adb  shell                #adb linux命令行, 默认根目录在 安卓手机那里
$adb  install  *.apk       #安装 apk
$adb  push  电脑源文件  手机目标目录    #adb push a.out /storage/self/primary/Download/   推送
$adb  pull  手机文件    电脑目录       #adb pull  /storage/self/primary/Download/a.out ~/Downloads 拉取
```

```shell

root uid0 gid0 
system uid 1000 gid1000
shell  uid 2000 gid2000
app  uid >10000 gid >10000



jarsigner -verbose -keystore E:\Epan\huluxia.key -signedjar C:\Users\xuhaiyang\Desktop\admo\sing_4096.apk C:\Users\xuhaiyang\Desktop\admo\un4096.apk huluxia

#adb 指令
$adb root    #获得 root权限shell
$adb devices 
$adb -s serialNumber shell 
$adb shell 进入手机管理
$adb install apkpath  -r -f -s 
$adb uninstall apkpackname -k 
$adb push 电脑端文件路径  手机端文件路径
$adb pull 手机端文件路径   电脑端文件路径
$adb reboot  重启手机
$adb reboot recovery 重启恢复模式
$adb reboot bootloader 重启引导模式  
$adb wait-for-device
$adb shell monkey -v -p com.tencent.mobileqq 500
$adb forward tcp:1100 tcp:1200
$adb shell getprop  获取手机参数

cat /proc/cpuinfo
adb shell dumpsys cpuinfo |notification| meminfo |cpuinfo 查看手机当前的cpu使用 notification使用 meminfo


1、手机截屏  screen sdk_version  filepath 
2、手机字体修改  替换/system/fonts/DroidSansFallback.ttf （中文 ）文件 ，替换/system/fonts/DroidSans.ttf (英文文件) 
3、卸载系统应用  
(1)获取应用的路径 pm path packname
(2)移除apk rm  apkpath
(3)彻底删除残留文件  pm uninstall packnmae ;rm -r /data/data/packname;

4、结束系统进程 
(1)ps  
(2)kill pid
5、静默安装卸载
6、可以禁止开机启动项,冻结应用


7、屏幕解锁
 rm /data/system/gesture.key;rm /data/syste/locksettings.*;
8、应用及应用数据的备份，移动应用到系统应用。
 busybox cp -r -f -p -P source/*  des/
10、修改开机动画
替换 /system/media/bootaniation.zip(注意压缩时用winrar 压缩存储模式)
 三星官方系统 需要替换/system/bin/samsungani 为自己的 然后执行上面的步骤。
11、更换系统刷机

12.查看短信，联系人数据库 
 
cat /data/data/com.android.providers.contacts/databases/contacts2.db > /data/lcoal/tmp/1.db 
adb pull /data/lcoal/tmp/1.db pc_path


 
cat /data/data/com.android.providers.telephony/databases/mmssms.db > /data/lcoal/tmp/1.db 
adb pull /data/lcoal/tmp/1.db pc_path




pm path packname 查看apk安装的路径
pm install -r -f -s apppath 安装apk，r 强制安装，f 安装手机内存 s 安装sdcard
pm uninstall -k packname 卸载应用 -k 保留应用数据 /data/data/packname下的数据 或者 /sdcard/Android/data/packnmae
pm enable packname 设置应用为不可用，或者组件不可用 组件跟类的完整路径  
pm disable packname 设置应用可用 
pm setInstallLocation 0 1 2  设置应用安装的默认目录 0 auto 1 手机内存 2 sdcard
pm getInstallLocation  查看当前设置
pm clear packname 清除应用缓存数据 



linux 常用指令：(权限)
busybox 
rm  移除文件 或 文件夹 rm /data/local/tmp/1.apk
cd  进入目录 cd /data/local/tmp
cat 查看文件内容 cat /proc/cpuinfo  ; 复制文件  cat /data/local/tmp/1.apk > /sdcard/1.apk
cp 复制文件  cp /data/local/tmp/1.apk /sdcard/1.apk
mv 移动文件，重命名文件  mv /data/local/tmp/1.apk /data/local/tmp/2.apk
chmod 为文件或目录赋权限  chmod 777 /data/local/tmp/1.apk
chown 为文件赋所属者 chown 0.0 /data/local/tmp/1.apk
echo 写入文件 如果文件不存在创建并写入 echo '111' > /sdcard/1111.txt
md5sum  获取文件md5码 md5sum /system/app/1.apk
halt 关机 不是所有手机都有此指令
reboot 重启手机
id    获取当前用户信息
touch  创建一个空文件 touch /data/local/tmp/1.txt
sleep  睡眠多少秒 sleep 10
mkdir  创建文件夹 mkdir /sdcard/nihao
ps     查看当前系统所有进程
kill  杀进程 kill 进程id   
ls  列出当前文件夹下的文件



gzip ungzip
mount  挂载分区 mount -o remount rw /system
df  查看磁盘空间 df /system


```

