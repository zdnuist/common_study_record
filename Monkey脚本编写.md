##### 需求
简单实现apk页面跳转-返回，目的查看日志用来分析页面耗时

##### 编写Monkey脚本
```
count = 1
speed = 1.0
start data >>

# 加载activity
LaunchActivity([pakcageName],[className])
# 事件等待时间
UserWait(1000)

# EVENT_DOWN
DispatchPointer(10000,10000, 0 , x, y, 0, 0, 0, 0, 0, 0, 0)
# EVENT_UP
DispatchPointer(10000,10000, 1 , x, y, 0, 0, 0, 0, 0, 0, 0)
UserWait(5000)

# 按压返回键
DispatchPress(KEYCODE_BACK)
UserWait(2000)
```
将脚本保存为xx.mks

##### 执行脚本
```
//导入脚本
adb push xx.mks /sdcard

// run
monkey -f xx.mks --ignore-security-exceptions  -v 20

```

##### 参考
[1] https://www.jianshu.com/p/85454be8424f  
[2] https://www.jianshu.com/p/9ef55c9b19aa
