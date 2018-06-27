#### GrowingIO是什么？
GrowingIO 是基于用户行为的新一代数据分析产品，吸取了国内外数据分析的最佳实践，创新了一整套数据采集、清洗整理、展现分析的一站式解决方案，帮助企业用数据驱动业务增长。


#### 初始化
GrowingIO.startWithConfiguration(this,
        new Configuration()
        .useID()
        .trackAllFragments()
        .setChannel("XXX应用商店")
    );
```
public static GrowingIO startWithConfiguration(Application application, Configuration configuration) {
    ...
    ...
    ...
      PermissionUtil.init(configuration.context);
      if (PermissionUtil.hasInternetPermission() && PermissionUtil.hasAccessNetworkStatePermission()) {
        GConfig.initialize(configuration);
        if (!GConfig.isInstrumented()) {
          throw new IllegalStateException("GrowingIO无法正常启动, 请检查:\n1. 首次集成时请先Clean项目再重新编译.\n2. (Gradle环境) 确保已经启用了GrowingIO插件(在build.gradle > buildscript > dependencies 中添加 classpath: 'com.growingio.android:vds-gradle-plugin:2.3.3' 然后在app目录下的build.gradle中添加apply plugin: 'com.growingio.android'.\n3. (Ant环境) 将vds-class-rewriter.jar的路径添加到环境变量ANT_OPTS中.\n有疑问请参考帮助文档 https://docs.growingio.com/SDK/Android.html , 或者联系在线客服 https://www.growingio.com/");
        } else {
          AppState.initialize(configuration);
          setTrackerHost(configuration.trackerHost);
          setReportHost(configuration.reportHost);
          setDataHost(configuration.dataHost);
          setTagsHost(configuration.tagsHost);
          setGtaHost(configuration.gtaHost);
          setWsHost(configuration.wsHost);
          setHybridJSSDKUrlPrefix(configuration.hybridJSSDKUrlPrefix);
          setJavaCirclePluginHost(configuration.javaCirclePluginHost);
          setZone(configuration.zone);
          GConfig config = GConfig.getInstance();
          LogUtil.d("GrowingIO", new Object[]{config});
          if (config.getSampling() > 0.0D) {
            configuration.sampling = config.getSampling();
          }

          if (!Util.isInSampling(getAPPState().deviceFactory().getDeviceId(), configuration.sampling)) {
            sInstance = new GrowingIO.EmptyGrowingIO(null);
            return sInstance;
          } else {
            Object var4 = sInstanceLock;
            synchronized(sInstanceLock) {
              if (null == sInstance) {
                try {
                  sInstance = new GrowingIO(configuration);  //初始化growingIO
                } catch (Throwable var7) {
                  return new GrowingIO.EmptyGrowingIO(null);
                }
              }
            }

            return sInstance;
          }
        }
      } else {
        throw new IllegalStateException("您的App没有网络权限, 请添加 INTERNET 和 ACCESS_NETWORK_STATE 权限");
      }
    }
  }
```

初始化growingIO
```
@TargetApi(14)
 public GrowingIO(final Configuration configuration) {
   this.mGConfig = GConfig.getInstance();
   MessageProcessor.init(configuration.context);
   this.setActivityLifecycleCallbacksRegistrar(configuration.registrar != null ? configuration.registrar : new ActivityLifecycleCallbacksRegistrar() {
     public void registerActivityLifecycleCallbacks(ActivityLifecycleCallbacks callbacks) {
       configuration.context.registerActivityLifecycleCallbacks(callbacks);
     }

     public void unRegisterActivityLifecycleCallbacks(ActivityLifecycleCallbacks callbacks) {
       configuration.context.unregisterActivityLifecycleCallbacks(callbacks);
     }
   });
   getAPPState().addActivityStateChangeListener(MessageProcessor.getInstance());
   this.mRegistrar.registerActivityLifecycleCallbacks(getAPPState());
   GConfig.sCanHook = true;
   if (GConfig.isReplace) {
     GConfig.DEBUG = true;
   }

   tryHookInstrumentation(); //Hook Instrumentation
 }
```

Hook Instrumentation
```
public static void hookInstrumentation() throws Throwable {
  if (!sIsInstrumentationHooked && GConfig.sCanHook && GConfig.supportMultiCircle && !GConfig.isReplace) {
    LogUtil.i("GIO.InstrumentationUtils", "hookInstrumentation");
    Class<?> activityThread = Class.forName("android.app.ActivityThread");
    Method sCurrentActivityThread = activityThread.getDeclaredMethod("currentActivityThread");
    sCurrentActivityThread.setAccessible(true);
    Object activityThreadObject = sCurrentActivityThread.invoke(activityThread);
    Field mInstrumentation = activityThread.getDeclaredField("mInstrumentation");
    mInstrumentation.setAccessible(true);
    Instrumentation odlInstrumentation = (Instrumentation)mInstrumentation.get(activityThreadObject);
    if (!isTestInstrument(odlInstrumentation.getClass())) {
      LogUtil.i("GIO.InstrumentationUtils", "正式环境，hook");
      GrowingIOInstrumentation growingIOInstrumentation = new GrowingIOInstrumentation(odlInstrumentation);
      mInstrumentation.set(activityThreadObject, growingIOInstrumentation);
      sIsInstrumentationHooked = true;
    } else {
      LogUtil.i("GIO.InstrumentationUtils", "自动化测试环境，不hook");
    }

  }
}


//
// Source code recreated from a .class file by IntelliJ IDEA
// (powered by Fernflower decompiler)
//

package com.growingio.android.sdk.utils;

import android.annotation.TargetApi;
import android.app.Activity;
import android.app.Instrumentation;
import android.app.Instrumentation.ActivityResult;
import android.content.Context;
import android.content.Intent;
import android.os.Bundle;
import android.os.IBinder;
import android.os.UserHandle;
import com.growingio.android.sdk.api.LoginAPI;
import com.growingio.android.sdk.circle.CircleManager;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;

public class GrowingIOInstrumentation extends Instrumentation {
  private static final String TAG = "GIO.GrowingIOInstrumentation";
  private Instrumentation mBase;

  public GrowingIOInstrumentation(Instrumentation base) {
    this.mBase = base;
  }

  public Activity startActivitySync(Intent intent) {
    this.enableMultiProcessCircle(intent);
    return (Activity)this.callHideMethod(this.getHideMethod("startActivitySync", Intent.class), intent);
  }

  public ActivityResult execStartActivity(Context who, IBinder contextThread, IBinder token, Activity target, Intent intent, int requestCode, Bundle options) {
    this.enableMultiProcessCircle(intent);
    return (ActivityResult)this.callHideMethod(this.getHideMethod("execStartActivity", Context.class, IBinder.class, IBinder.class, Activity.class, Intent.class, Integer.TYPE, Bundle.class), who, contextThread, token, target, intent, requestCode, options);
  }

  public void execStartActivitiesAsUser(Context who, IBinder contextThread, IBinder token, Activity target, Intent[] intents, Bundle options, int userId) {
    Intent[] var8 = intents;
    int var9 = intents.length;

    for(int var10 = 0; var10 < var9; ++var10) {
      Intent intent = var8[var10];
      this.enableMultiProcessCircle(intent);
    }

    this.callHideMethod(this.getHideMethod("execStartActivitiesAsUser", Context.class, IBinder.class, IBinder.class, Activity.class, Intent[].class, Bundle.class, Integer.TYPE), who, contextThread, token, target, intents, options, userId);
  }

  public ActivityResult execStartActivity(Context who, IBinder contextThread, IBinder token, String target, Intent intent, int requestCode, Bundle options) {
    this.enableMultiProcessCircle(intent);
    return (ActivityResult)this.callHideMethod(this.getHideMethod("execStartActivity", Context.class, IBinder.class, IBinder.class, String.class, Intent.class, Integer.TYPE, Bundle.class), who, contextThread, token, target, intent, requestCode, options);
  }

  @TargetApi(17)
  public ActivityResult execStartActivity(Context who, IBinder contextThread, IBinder token, String resultWho, Intent intent, int requestCode, Bundle options, UserHandle user) {
    this.enableMultiProcessCircle(intent);
    return (ActivityResult)this.callHideMethod(this.getHideMethod("execStartActivity", Context.class, IBinder.class, IBinder.class, String.class, Intent.class, Integer.TYPE, Bundle.class, UserHandle.class), who, contextThread, token, resultWho, intent, requestCode, options, user);
  }

  public ActivityResult execStartActivityAsCaller(Context who, IBinder contextThread, IBinder token, Activity target, Intent intent, int requestCode, Bundle options, boolean ignoreTargetSecurity, int userId) {
    this.enableMultiProcessCircle(intent);
    return (ActivityResult)this.callHideMethod(this.getHideMethod("execStartActivityAsCaller", Context.class, IBinder.class, IBinder.class, Activity.class, Intent.class, Integer.TYPE, Bundle.class, Boolean.TYPE, Integer.TYPE), who, contextThread, token, target, intent, requestCode, options, ignoreTargetSecurity, userId);
  }

  private Method getHideMethod(String methodName, Class... argmentsType) {
    Class cls = this.mBase.getClass();

    try {
      Method method = cls.getMethod(methodName, argmentsType);
      method.setAccessible(true);
      return method;
    } catch (NoSuchMethodException var5) {
      LogUtil.e("GIO.GrowingIOInstrumentation", var5.getMessage());
      throw new IllegalStateException("反射出现异常: " + methodName);
    }
  }

  public void callActivityOnCreate(Activity activity, Bundle icicle) {
    this.mBase.callActivityOnCreate(activity, icicle);
  }

  private Object callHideMethod(Method targetMethod, Object... arguments) {
    try {
      return targetMethod.invoke(this.mBase, arguments);
    } catch (IllegalAccessException var5) {
      LogUtil.e("GIO.GrowingIOInstrumentation", var5.getMessage());
    } catch (InvocationTargetException var6) {
      Throwable throwable = var6.getTargetException();
      if (throwable instanceof RuntimeException) {
        throw (RuntimeException)throwable;
      }
    }

    throw new IllegalStateException("反射出现异常:" + targetMethod);
  }

  public Activity newActivity(ClassLoader cl, String className, Intent intent) throws InstantiationException, IllegalAccessException, ClassNotFoundException {
    return this.mBase.newActivity(cl, className, intent);
  }

  private void enableMultiProcessCircle(Intent intent) {
    LogUtil.i("GIO.GrowingIOInstrumentation", "enableMultiProcessCircle " + intent.toString());
    if (CircleManager.getInstance().isEnable()) {
      intent.putExtra("multiProcess", true);
      intent.putExtra("multiProcessCircleType", CircleManager.getInstance().getCircleType());
      LogUtil.i("GIO.GrowingIOInstrumentation", "multiProcessCircleType " + CircleManager.getInstance().getCircleType());
      if (CircleManager.getInstance().isAppCircleEnabled()) {
        intent.putExtra("multiProcessCricleToken", LoginAPI.getInstance().getToken());
        intent.putExtra("multiProcessCircleUserid", LoginAPI.getInstance().getUserId());
        LogUtil.i("GIO.GrowingIOInstrumentation", "multiProcessCricleToken " + LoginAPI.getInstance().getToken());
        LogUtil.i("GIO.GrowingIOInstrumentation", "multiProcessCircleUserid " + LoginAPI.getInstance().getUserId());
      } else if (CircleManager.getInstance().isProjection()) {
        intent.putExtra("multiProcessCircleRoomNumber", CircleManager.getInstance().getCircleRoomNumber());
      }
    }

  }
}

```
##### 事件埋点
com.growingio.android.sdk.agent.VdsAgent
通过自动代码注入的方式，将事件处理的内部添加埋点VdsAgent，来处理用户的事件消息。

```
@Instrumented
 protected void onNewIntent(Intent paramIntent)
 {
   VdsAgent.onNewIntent(this, paramIntent); //1
   super.onNewIntent(paramIntent);
   Log.i("MainActivity", "onNewIntent");
 }


 this.clearView.setOnClickListener(new View.OnClickListener()
    {
      @Instrumented
      public void onClick(View paramAnonymousView)
      {
        VdsAgent.onClick(this, paramAnonymousView); //2
        MainActivity.this.editText.setText("");
      }
    });
```

##### 工具总结

查找Acvitity
```
public static Activity findActivity(@NonNull Context context) {
    if (!(context instanceof ContextWrapper)) {
      return null;
    } else {
      ContextWrapper current;
      Context parent;
      for(current = (ContextWrapper)context; !(current instanceof Activity); current = (ContextWrapper)parent) {
        parent = current.getBaseContext();
        if (!(parent instanceof ContextWrapper)) {
          return null;
        }
      }

      return (Activity)current;
    }
  }
```

获取当前window的views
```
public static View[] getWindowViews() {
    View[] result = new View[0];
    if (sWindowManger == null) {
      Activity current = AppState.getInstance().getForegroundActivity();
      return current != null ? new View[]{current.getWindow().getDecorView()} : result;
    } else {
      try {
        View[] views = null;
        if (sArrayListWindowViews) {
          views = (View[])((ArrayList)viewsField.get(sWindowManger)).toArray(result);
        } else if (sViewArrayWindowViews) {
          views = (View[])((View[])viewsField.get(sWindowManger));
        }

        if (views != null) {
          result = views;
        }
      } catch (Exception var2) {
        LogUtil.d(var2);
      }

      return stripNullView(result);
    }
  }

  public static View[] stripNullView(View[] array) {
    int nullViewCount = 0;
    View[] stripNullViews = array;
    int i = array.length;

    int j;
    for(j = 0; j < i; ++j) {
      View view = stripNullViews[j];
      if (view == null) {
        ++nullViewCount;
      }
    }

    if (nullViewCount > 0) {
      stripNullViews = new View[array.length - nullViewCount];
      i = 0;

      for(j = 0; i < array.length && j < stripNullViews.length; ++i) {
        if (array[i] != null) {
          stripNullViews[j++] = array[i];
        }
      }

      array = stripNullViews;
    }

    return array;
  }
```

获取windowManager
```
String windowManagerClassName;
      if (VERSION.SDK_INT >= 17) {
        windowManagerClassName = "android.view.WindowManagerGlobal";
      } else {
        windowManagerClassName = "android.view.WindowManagerImpl";
      }

      Class windowManager = null;

      try {
        windowManager = Class.forName(windowManagerClassName);
        String windowManagerString;
        if (VERSION.SDK_INT >= 17) {
          windowManagerString = "sDefaultWindowManager";
        } else if (VERSION.SDK_INT >= 13) {
          windowManagerString = "sWindowManager";
        } else {
          windowManagerString = "mWindowManager";
        }

        viewsField = windowManager.getDeclaredField("mViews");
        Field instanceField = windowManager.getDeclaredField(windowManagerString);
        viewsField.setAccessible(true);
        if (viewsField.getType() == ArrayList.class) {
          sArrayListWindowViews = true;
        } else if (viewsField.getType() == View[].class) {
          sViewArrayWindowViews = true;
        }

        instanceField.setAccessible(true);
        sWindowManger = instanceField.get((Object)null);
      } catch (NoSuchFieldException var11) {
        LogUtil.d(var11);
      } catch (IllegalAccessException var12) {
        LogUtil.d(var12);
      } catch (ClassNotFoundException var13) {
        LogUtil.d(var13);
      }
```


##### 参考
1. https://docs.growingio.com/
2. https://github.com/growingio
