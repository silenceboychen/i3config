i3桌面管理器配置说明
=====

> 基于i3默认的配置做了一些添加,以下内容为添加内容,可直接复制使用.

### 锁屏和休眠快捷键

需要安装 ``i3lock``，需要处理图片则还需安装 ``imagemagick``，自动锁屏需要安装 ``xautolock``。

```
$ sudo apt-get install i3lock
$ sudo apt-get install imagemagick
$ sudo apt-get install xautolock
```
i3配置

```
# 只是锁屏
bindsym $mod+l exec "i3lock -c 01544A -n"
# 锁屏 + 休眠
# bindsym $mod+Shift+s exec "i3lock && systemctl suspend"
# 锁屏带模糊的截图
bindsym $mod+Shift+s exec "scrot /tmp/lockscreen.png && mogrify -scale 50% -gaussian-blur 0x4 -gamma 0.8 -scale 200% /tmp/lockscreen.png && i3lock -i /tmp/lockscreen.png"
# 10 分钟不活动自动锁屏,这里用了 notify-send 弹出提示
# -notify在锁定之前警告用户的时间长度,单位秒;-notifier指定要使用的通知程序。此选项仅与-notify结合使用
# 这里配置完成后无法执行,需要重启才能生效
exec --no-startup-id xautolock -time 10 -locker 'scrot /tmp/lockscreen.png && mogrify -scale 50% -gaussian-blur 0x4 -gamma 0.8 -scale 200% /tmp/lockscreen.png && i3lock -i /tmp/lockscreen.png' -notify 10 -notifier 'notify-send "锁屏提示:" "还有10秒钟我就要自动锁屏了!" -i /tmp/lockscreen.png'
# 上述内容也可写在shell脚本里,执行shell文件,shell文件要设置为可执行文件: sudo chmod +x lock.sh
# exec ./lock.sh
```

### 截屏并复制到剪贴板

需要安装 ``scrot`` 和 ``xclip``。

```
$ sudo apt-get install scrot
$ sudo apt-get install xclip
```

i3配置

```
# 全屏截图
bindsym --release Print exec "scrot -b -m /tmp/screenshot.png && xclip -selection clipoard -t 'image/png' /tmp/screenshot.png"
# 截取当前窗口
bindsym --release $mod+Print exec "scrot -u -b -m /tmp/screenshot.png && xclip -selection clipoard -t 'image/png' /tmp/screenshot.png"
# 鼠标选择区域
bindsym --release $mod+Shift+Print exec "scrot -s -b -m /tmp/screenshot.png && xclip -selection clipoard -t 'image/png' /tmp/screenshot.png"
# QQ 风格的截屏快捷键
bindsym --release Control+Mod1+A exec "scrot -s -b -m /tmp/screenshot.png && xclip -selection clipoard -t 'image/png' /tmp/screenshot.png"
```
