## learning-gradle-demo-code

学习 gradle 构建工具 demo 代码。

#### 多模块构建
主 build.gradle :
``` groovy
ext {
    javaVersion = JavaVersion.VERSION_1_8
    springBootVersion = "2.5.2"
}

//allprojects标签为所有项目(包括根项目和所有子项目)定义行为
allprojects {
    //设置group和version属性
    group = GROUPID
    version = VERSION
}

//subprojects 为所有子项目定义行为
subprojects {
    //应用Java插件
    apply plugin: 'java'
    //设置编译
    sourceCompatibility = "$javaVersion"
    targetCompatibility = "$javaVersion"
    tasks.withType(JavaCompile) {
        options.encoding = "UTF-8"
    }

    repositories {
        mavenCentral()
    }

    dependencies {
        compile
        implementation platform("org.springframework.boot:spring-boot-dependencies:$springBootVersion")
    }
}

```

子 build.gradle :
```groovy
//在子项目可以定义自己的特有的行为
plugins {
    id 'war'
}

dependencies {
    //比如project-web依赖project-model和project-service
    compile project(':project-model')
    //排除依赖
    implementation (project(":project-service")) {
        exclude group: "org.springframework", module: "spring-context"
    }
    implementation('org.springframework.boot:spring-boot-starter-web')
}
```