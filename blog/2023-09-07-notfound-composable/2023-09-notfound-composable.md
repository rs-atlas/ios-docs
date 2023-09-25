---
slug: KMP 프로젝트 세팅 - Compose 자동완성 안되는 현상
title: KMP 프로젝트 세팅 - KMP Compose 자동완성 안되는 현상
authors: atlas
tags: [ kmp , compose, android ,error , kotlin , carter-rs ]
---


### 이슈
프로젝트 생성시 commonMain에서 Composable을 사용할 수 없음

![icon](./image01.png)
![icon](./image02.png)

### 해결방법
#### build.gradle.kts(Project: )
변경
```
    id("com.android.application").version("8.0.2").apply(false)
    id("com.android.library").version("8.0.2").apply(false)
```

추가
```
id("org.jetbrains.compose").version("1.5.0").apply(false)
```


#### build.gradle.kts(Moduel:shared )

```
plugins {
    ...
    id("org.jetbrains.compose")
}

sourceSets {
        val commonMain by getting {
            dependencies {
                //put your multiplatform dependencies here
                implementation(compose.ui)
            }
        }
    }

```

### 결과화면

![icon](./image03.png)

## Thanks to Carter-rs