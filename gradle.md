##### GUIDE
[GUIDE](https://gradle.org/guides/#getting-started)
[jcenter](https://jcenter.bintray.com)
```
buildscript{
    repositories{
        jcenter()
    }
    dependencies{
        classpath 'com.android.tools.build:gradle:2.3.3'
    }
}

allprojects{
    repositories{
      jcenter()
    }
}

task clean(type: Delete){
    delete rootProject.buildDir
}
```
remove app from device
```
./gradlew uninstallAll
```
###### build scan
```
buildscript {
    repositories {
        jcenter()
        maven { url 'https://plugins.gradle.org/m2' }    
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:2.4.0-alpha7'
        classpath 'com.gradle:build-scan-plugin:1.7.1'   
    }
}

apply plugin: 'com.gradle.build-scan'                    

buildScan {                                              
    licenseAgreementUrl = 'https://gradle.com/terms-of-service'
    licenseAgree = 'yes'
}
```

```
./gradlew build --scan
```  

##### Android Build Cache
```
android {
  defaultConfig {
    // If you do enable multidex, you must also set
    // minSdkVersion to 21 or higher.
    multiDexEnabled false
  }
  buildTypes {
    <build-type> {
      minifyEnabled false
    }
  }
  dexOptions {
    preDexLibraries true
  }
  ...
}
...
```
Change the location of the build Cache
```
// You can specify either an absolute path or a path relative
// to the gradle.properties file.
android.buildCacheDir=<path-to-directory>
```
Clear the build Cache
```
gradlew cleanBuildCach
```

Disable the build cache
```
// To re-enable the build cache, either delete the following
// line or set the property to 'true'.
android.enableBuildCache=false
```
##### 加速gradle 构建
1.offline
```
./gradlew build --offline
```
2.gradle.properties
```
org.gradle.jvmargs=-Xmx3584M -XX:MaxPermSize=1024m -XX:+HeapDumpOnOutOfMemoryError -Dfile.encoding=UTF-8

org.gradle.daemon=true

org.gradle.parallel=true

android.enableBuildCache=true
android.buildCacheDir=G:\\AndroidBuildDir\\
org.gradle.caching=true
```
3.build.gradle
```
dexOptions {
       jumboMode = true
       preDexLibraries true
       javaMaxHeapSize "3g"
       dexInProcess = true
       maxProcessCount 8
   }
```
