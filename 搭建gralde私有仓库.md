###### Nexus
下载地址 https://www.sonatype.com/download-oss-sonatype
使用的版本为：nexus-2.14.5-02-bundle

根据自己系统环境到达指定目录
eg: nexus-2.14.5-02-bundle/nexus-2.14.5-02/bin/jsw/windows-x86-64
双击console-nexus.bat运行
地址: http://127.0.0.1:8081/nexus/#welcome

Login:
admin/admin123

新建hosted Repositories
```
Repository Policy : Release
Deployment Policy : Allow Redeploy
```

上传第三方jar/aar
```
1.From POM
2.Select POM to UPload
3.Select Artifact(s) to UPload
4.Add Artifact
5.Upload Artifacts
```
