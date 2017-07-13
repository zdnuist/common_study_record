##### 判断是否是low-RAM device
```
public static boolean isLowRamDeviceStatic() {
      return "true".equals(SystemProperties.get("ro.config.low_ram", "false"));
  }
```
##### 清理应用数据
```
public boolean clearApplicationUserData() {
        return clearApplicationUserData(mContext.getPackageName(), null);
    }
```
