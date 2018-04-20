#####代码混淆，加大代码理解难度
1. -obfuscationdictionary dictionary.txt
https://github.com/qbeenslee/gradle-proguard-dictionary

##### 增加签名验证，防止二次打包
1. 获取当前签名与签名文件是否一致
```
private static String getSignture(Application paramApplication) {
   try {
     String packageName = "";
     List<PackageInfo> packages = paramApplication.getPackageManager().getInstalledPackages(
         PackageManager.GET_SIGNATURES);
     for (PackageInfo packageInfo : packages) {
       Signature[] signs = packageInfo.signatures;
       Signature sign = signs[0];
       String signString = sign.toCharsString();
       packageName = packageInfo.packageName;
       if(!packageName.contains("college")){
         continue;
       }
       System.out.println(signString);

       Log.e("zd_sing" , "p:" + packageName + ",sing:" + signString);
     }

     PackageInfo info = paramApplication.getPackageManager().getPackageInfo("xxxx", PackageManager.GET_SIGNATURES);
     System.out.println(info.signatures[0].toCharsString());

   } catch (Exception e) {
     return "";
   }
   return "";
 }
```


##### apktool 使用
1.解包
apktool d in.apk
2.重新打包
apktool b in -o out.apk
3.创建一个签名
keytool -genkeypair -validity 10000 -alias "fake" -keyalg "RSA" -keystore "fake.keystore"
passwd: fake123
4.查看签名
keytool -list -keystore fake.keystore
5.给apk签名
jarsigner -verbose  -tsa http://sha256timestamp.ws.symantec.com/sha256/timestamp -keystore  fake.keystore -signedjar out-sin.apk out.apk fake















df
