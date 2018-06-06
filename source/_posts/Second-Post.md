---
title: Generate release mode APK for React-Native project to publish on PlayStore
description: Generate release mode APK for React-Native project to publish on PlayStore
tags: React-Native, APK
---

Good Day All! I hope you are doing Good.

From last couple of days, Me and my friends were doing comparative study of ionic3 and react-native. During that period, we were struggling to generate release mode APK for React-Native project.

Finally, we came across following solution:
 
#### Create and then copy a keystore file to android/app
```bat
keytool -genkey -v -keystore mykeystore.keystore -alias mykeyalias -keyalg RSA -keysize 2048 -validity 10000
```
#### Setup your gradle variables in android/gradle.properties
```Java
MYAPP_RELEASE_STORE_FILE = mykeystore.keystore
MYAPP_RELEASE_KEY_ALIAS = mykeyalias
MYAPP_RELEASE_STORE_PASSWORD = *****
MYAPP_RELEASE_KEY_PASSWORD = *****
 ```
#### Add signing config to android/app/build.gradle
 ```Java
android {
  signingConfigs {
    release {
      storeFile file(MYAPP_RELEASE_STORE_FILE)
      storePassword MYAPP_RELEASE_STORE_PASSWORD
      keyAlias MYAPP_RELEASE_KEY_ALIAS
      keyPassword MYAPP_RELEASE_KEY_PASSWORD
    }
  }
  buildTypes {
    release {
      signingConfig signingConfigs.release
    }
  }
}
```
#### Generate your release APK:
 ```bat
cd android && ./gradlew assembleRelease
```

Your APK will get generated at: *android/app/build/outputs/apk/app-release.apk*

Special Thanks to Tyler Buchea : http://blog.tylerbuchea.com/react-native-publishing-an-android-app/
