# SpingBootでリリースしたGit情報を表示する

## 概要

spring-boot-actuatorを使用してGit情報を表示する


## バージョン

java ： 1.8.0_121  
springBoot ： 1.5.4.RELEASE  
gradleGitProperties ： 1.4.17  

## 手順

1. build.gradleに設定記述
1. ビルド実施
1. SpringBoot起動
1. Git情報を取得

### build.gradleに設定記述

~~~ groovy

// gradle-git-propertiesを追加
buildscript {
    ext {
        gradleGitPropertiesVersion = '1.4.17'
    }
    repositories {
        maven {
            url("https://plugins.gradle.org/m2/")
        }
    }
    dependencies {
        classpath("gradle.plugin.com.gorylenko.gradle-git-properties:gradle-git-properties:${gradleGitPropertiesVersion}")
    }
}
apply plugin: "com.gorylenko.gradle-git-properties"

// Spring Actuatorを参照
dependencies {
    compile('org.springframework.boot:spring-boot-starter-actuator')
}

// gradle-git-propertiesの設定
gitProperties {
    dateFormat = "yyyy-MM-dd'T'HH:mmZ"
    dateFormatTimeZone = "JST"
    gitRepositoryRoot = new File("${project.rootDir}/..")
}

~~~

* gitRepositoryRoot  
 gitレポジトリの場所を指定（.gitが存在する場所）  
 
* dateFormat  
　日付フォーマットを変更（SimpleDateFormatを使用）

* dateFormatTimeZone  
　タイムゾーンを変更（TimeZone.getTimeZoneを使用）

### ビルド実施

~~~ 

git clone https://github.com/ao-zkn/springboot-samples.git
cd springboot-samples/sampleGradleGitProperties/
chmod 755 gradlew
./gradlew clean build

~~~ 

### SpringBoot起動

~~~

java -jar  build/libs/sampleGradleGitProperties-0.0.1-SNAPSHOT.jar &
curl http://localhost:8080/info

~~~

### Git情報を取得

#### 実行
~~~

curl http://localhost:8080/info

~~~

#### 結果
~~~ json
{
    "git": {
        "commit": {
            "time": "2017-07-25T15:10+0900",
            "id": "eb184cf"
        },
        "branch": "master"
    }
}
~~~

### 参考

https://github.com/n0mer/gradle-git-properties
