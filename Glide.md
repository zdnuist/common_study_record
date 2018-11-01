###### 获取bitmap大小

```
@TargetApi(Build.VERSION_CODES.KITKAT)
    public static int getBitmapByteSize(Bitmap bitmap) {
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.KITKAT) {
            // Workaround for KitKat initial release NPE in Bitmap, fixed in MR1. See issue #148.
            try {
                return bitmap.getAllocationByteCount();
            } catch (NullPointerException e) {
                // Do nothing.
            }
        }
        return bitmap.getHeight() * bitmap.getRowBytes();
    }
```

你可以使用 adb shell setprop log.tag.<tag_name> <VERBOSE|DEBUG> 操作为任何下面提到的标签(tag))开启日志。VERBOSE 级别的日志会显得更加冗余但包含更多有用的信息

什么是硬件位图（Hardware Bitmaps）？
Bitmap.Config.HARDWARE 是一种 Android O 添加的新的位图格式。硬件位图仅在显存 (graphic memory) 里存储像素数据，并对图片仅在屏幕上绘制的场景做了优化。
我们为什么应该使用硬件位图?
因为硬件位图仅储存像素数据的一份副本。一般情况下，应用内存中有一份像素数据（即像素字节数组），而在显存中还有一份副本（在像素被上传到 GPU之后）。而硬件位图仅持有 GPU 中的副本，因此：

硬件位图仅需要一半于其他位图配置的内存；
硬件位图可避免绘制时上传纹理导致的内存抖动。

```
@NonNull
  private Context getContextForPackage(Uri source, String packageName) {
    try {
      return context.createPackageContext(packageName, /*flags=*/ 0);
    } catch (NameNotFoundException e) {
      throw new IllegalArgumentException(
          "Failed to obtain context or unrecognized Uri format for: " + source, e);
    }
  }


Drawable.class.isAssignableFrom(clazz)


```
