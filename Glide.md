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

