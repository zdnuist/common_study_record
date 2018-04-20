##### 注册JFrog
地址：https://bintray.com

##### Create Repository
name: REP_NAME (后面上传的时候需要统一)
type: maven

##### 添加ClassPath
```
classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.6'
classpath 'com.github.dcendents:android-maven-gradle-plugin:1.5'
```

##### 在libraryModule(build.gradle)编写上传脚本
```
apply plugin: 'com.github.dcendents.android-maven'
apply plugin: 'com.jfrog.bintray'

group = 'com.xx.xx.xx'
version = '1.0.0'
def siteUrl = 'https://github.com/xxx/xxxx'
def gitUrl = 'https://github.com/xxx/xxxx.git'
Properties properties = new Properties()
properties.load(project.rootProject.file('local.properties').newDataInputStream())

bintray {

    user = properties.getProperty("bintray.user")
    key = properties.getProperty("bintray.apikey")

    configurations = ['archives'] //When uploading configuration files
    pkg {
        repo = 'REP_NAME' //与上面创建的仓库名统一
        name = 'xxx'
        desc = 'xxx xxx xx'
        websiteUrl = siteUrl
        issueTrackerUrl = 'https://github.com/xxxx/xxx/issues'
        vcsUrl = gitUrl
        licenses = ['MIT']
        labels = ['android']
        publicDownloadNumbers = true
        publish = true
    }
}


install {
    repositories.mavenInstaller {
        pom {
            project {
                packaging 'aar'
                name 'xxx'
//                url siteUrl
                licenses {
                    license {
                        name 'The MIT License (MIT)'
                        url 'http://opensource.org/licenses/MIT'
                    }
                }
//                developers {
//                    developer {
//                        id 'xx'
//                        name 'xx x'
//                        email 'xxxv@gmail.com'
//                    }
//                }
//                scm {
//                    connection 'https://github.com/xx/xxx.git'
//                    developerConnection 'https://github.com/xx/xxx.git'
//                    url siteUrl
//                }
            }
        }
    }
}


task sourcesJar(type: Jar) {
    from android.sourceSets.main.java.srcDirs
    classifier = 'sources'
}

task javadoc(type: Javadoc) {
    source = android.sourceSets.main.java.srcDirs
    classpath += project.files(android.getBootClasspath().join(File.pathSeparator))
}

task javadocJar(type: Jar, dependsOn: javadoc) {
    classifier = 'javadoc'
    from javadoc.destinationDir
}
artifacts {
    archives javadocJar
    archives sourcesJar
}

task findConventions << {
    println project.getConvention()
}

```

##### 在local.properties中配置jfrog参数
```
bintray.user = //jfrog上注册的用户名
bintray.apikey = //jfrog个人主页上查看
```

##### 执行编译脚本
```
gradlew :libraryModule:bintrayUpload
```
