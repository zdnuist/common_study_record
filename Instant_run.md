
##### 重启App
```
//restart app
public static void restartApp(Activity foreground){
    Context context = foreground;
    Intent intent = new Intent(context, foreground.getClass());
    int intentId = 0;
    PendingIntent pendingIntent = PendingIntent.getActivity(context, intentId, intent, PendingIntent.FLAG_CANCEL_CURRENT);

    AlarmManager mgr = (AlarmManager)context.getSystemService(Context.ALARM_SERVICE);
    mgr.set(1, System.currentTimeMillis() + 100L, pendingIntent);
  }

```
##### 重启Activity & 获取栈中的activities
```
private static void restartActivity(Activity activity) {
    if (Log.isLoggable("InstantRun", 2)) {
      Log.v("InstantRun", "About to restart " + activity.getClass().getSimpleName());
    }

    while (activity.getParent() != null) {
      if (Log.isLoggable("InstantRun", 2)) {
        Log.v("InstantRun", activity.getClass().getSimpleName() + " is not a top level activity; restarting " + activity
          .getParent().getClass().getSimpleName() + " instead");
      }
      activity = activity.getParent();
    }

    activity.recreate();
  }

public static Activity getForegroundActivity(Context context)
  {
    List list = getActivities(context, true);
    return list.isEmpty() ? null : (Activity)list.get(0);
  }

  public static List<Activity> getActivities(Context context, boolean foregroundOnly) {
    List list = new ArrayList();
    ArrayMap activities;
    try {
      Class activityThreadClass = Class.forName("android.app.ActivityThread");
      Object activityThread = MonkeyPatcher.getActivityThread(context, activityThreadClass);
      Field activitiesField = activityThreadClass.getDeclaredField("mActivities");
      activitiesField.setAccessible(true);

      Object collection = activitiesField.get(activityThread);
      Collection c;
      if ((collection instanceof HashMap))
      {
        Map activities = (HashMap)collection;
        c = activities.values();
      }
      else
      {
        Collection c;
        if ((Build.VERSION.SDK_INT >= 19) && ((collection instanceof ArrayMap)))
        {
          activities = (ArrayMap)collection;
          c = activities.values();
        } else {
          return list;
        }
      }
      Collection c;
      for (activities = c.iterator(); activities.hasNext(); ) { Object activityRecord = activities.next();
        Class activityRecordClass = activityRecord.getClass();
        if (foregroundOnly) { Field pausedField = activityRecordClass.getDeclaredField("paused");
          pausedField.setAccessible(true);
          if (pausedField.getBoolean(activityRecord));
        }
        else {
          Field activityField = activityRecordClass.getDeclaredField("activity");
          activityField.setAccessible(true);
          Activity activity = (Activity)activityField.get(activityRecord);
          if (activity != null)
            list.add(activity);
        } }
    }
    catch (Throwable localThrowable) {
    }
    return list;
  }
```
##### 获取ActivityThread
```
public static Object getActivityThread(Context context, Class<?> activityThread)
  {
    try
    {
      if (activityThread == null) {
        activityThread = Class.forName("android.app.ActivityThread");
      }
      Method m = activityThread.getMethod("currentActivityThread", new Class[0]);
      m.setAccessible(true);
      Object currentActivityThread = m.invoke(null, new Object[0]);
      Object apk;
      Field mActivityThreadField;
      if ((currentActivityThread == null) && (context != null))
      {
        Field mLoadedApk = context.getClass().getField("mLoadedApk");
        mLoadedApk.setAccessible(true);
        apk = mLoadedApk.get(context);
        mActivityThreadField = apk.getClass().getDeclaredField("mActivityThread");
        mActivityThreadField.setAccessible(true);
      }
      return mActivityThreadField.get(apk);
    }
    catch (Throwable ignore) {
    }
    return null;
  }
```
